<script setup>
import { ref, watch } from "vue";
import { professionalProjects, academicProjects } from "./data/projects";
import { onMounted, onBeforeUnmount } from "vue";

function handleResize() {
  // Al cambiar de tamaño, cerramos el menú siempre
  menuOpen.value = false;
}
onMounted(() => {
  handleResize();
  window.addEventListener("resize", handleResize);
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", handleResize);
});

const activeImg = ref("");
const menuOpen = ref(false);
const modalOpen = ref(false);
const selected = ref(null);

watch(selected, (p) => {
  activeImg.value = p?.cover || p?.images?.[0] || "";
});

function openModal(p) {
  selected.value = p;
  modalOpen.value = true;
  document.body.style.overflow = "hidden";
}
function closeModal() {
  modalOpen.value = false;
  selected.value = null;
  document.body.style.overflow = "";
}
</script>

<template>
  <header id="header" class="header">
    <div class="logo">
      <img src="/imagenes/logo-jrhm.png" alt="Jose Ramon Hurtado" />
    </div>

    <nav class="menu">
      <button
        class="menuHamburguesa"
        aria-label="Toggle menu"
        @click="menuOpen = !menuOpen"
      >
        <div class="line line1" :class="{ cambiar: menuOpen }"></div>
        <div class="line line2" :class="{ cambiar: menuOpen }"></div>
        <div class="line line3" :class="{ cambiar: menuOpen }"></div>
      </button>

      <ul
        class="menu-list"
        :class="{ open: menuOpen }"
        @click="menuOpen = false"
      >
        <li><a href="#home">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#portfolio">Projects</a></li>
        <li><a href="#services">Services</a></li>
        <li><a href="#skills">Skills</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
  </header>

  <!-- HERO -->
  <section id="home" class="hero">
    <div class="hero-content">
      <h1 class="hero-title">Hi, I’m José Ramón</h1>
      <p>Full Stack Web Developer · React · Vue · Node.js · MySQL · Remote</p>

      <div class="hero-buttons">
        <a href="#portfolio" class="btn">View Projects</a>
        <a
          href="/CV_JoseRamonHurtado_FullStack_ES_ATS.pdf"
          class="btn btn-secondary"
          download="JoseRamonHurtado_CV_EN.pdf"
        >
          Download CV
        </a>
      </div>

      <div class="social-icons">
        <a
          href="https://github.com/jhurtadomije"
          aria-label="GitHub"
          target="_blank"
          rel="noreferrer"
        >
          GitHub
        </a>
        <a
          href="https://www.linkedin.com/in/jose-ramon-hurtado-mije-888629342"
          aria-label="LinkedIn"
          target="_blank"
          rel="noreferrer"
        >
          LinkedIn
        </a>
      </div>
    </div>

    <div class="hero-image-container">
      <img src="/imagenes/html.png" alt="Tech" class="hero-image" />
    </div>
  </section>

  <!-- ABOUT -->
  <section id="about" class="about">
    <div class="about-content">
      <div class="about-image">
        <img src="/imagenes/imagenPortfolio.png" alt="About" />
      </div>
      <div class="about-text">
        <h2 class="about-title">About</h2>
        <p>
          Full Stack Web Developer with real production projects. I build web
          apps end-to-end: frontend, backend, deployment and maintenance.
        </p>
      </div>
    </div>
  </section>

  <!-- SERVICES -->
  <section id="services">
    <div class="animated-title"><h2>Services</h2></div>

    <div class="services">
      <div class="service">
        <i class="fas fa-laptop-code service-icon" aria-hidden="true"></i>
        <h3>Web Development</h3>
        <p>Modern, responsive websites and web apps.</p>
      </div>

      <div class="service">
        <i class="fas fa-server service-icon" aria-hidden="true"></i>
        <h3>APIs & Backend</h3>
        <p>REST APIs, business logic and database integration.</p>
      </div>

      <div class="service">
        <i class="fas fa-tools service-icon" aria-hidden="true"></i>
        <h3>Maintenance</h3>
        <p>Improvements, bug fixes, performance and deployment.</p>
      </div>
    </div>
  </section>

  <!-- SKILLS -->
  <section id="skills" class="skills-section">
    <h2 class="section-title">Skills</h2>

    <div class="skills-grid">
      <div class="skill-card">
        <h4>Frontend</h4>
        <p>React · Vue 3 · HTML5 · CSS3 · SASS</p>
      </div>

      <div class="skill-card">
        <h4>Backend</h4>
        <p>Node.js · Express · REST APIs · Laravel (php)</p>
      </div>

      <div class="skill-card">
        <h4>Databases</h4>
        <p>MySQL · PostgreSQL · Firebase (Auth/Firestore)</p>
      </div>

      <div class="skill-card">
        <h4>Tooling</h4>
        <p>Git/GitHub · Vite · Docker (basic) · Deploy</p>
      </div>
    </div>
  </section>

  <!-- PROJECTS -->
  <section id="portfolio">
    <h2>Projects</h2>

    <div class="portfolio-section">
      <h3>Professional Projects</h3>
      <div class="gallery">
        <button
          v-for="p in professionalProjects"
          :key="p.title"
          class="project-card"
          type="button"
          @click="openModal(p)"
        >
          <div class="project-media">
            <img
              class="project-img"
              :src="p.cover"
              :alt="p.title"
              loading="lazy"
            />
            <div class="project-overlay"></div>

            <div class="project-actions">
              <a
                v-if="p.demo"
                class="action-btn"
                :href="p.demo"
                target="_blank"
                rel="noreferrer"
                @click.stop
              >
                Live
              </a>
              <a
                v-if="p.code"
                class="action-btn ghost"
                :href="p.code"
                target="_blank"
                rel="noreferrer"
                @click.stop
              >
                Code
              </a>
            </div>
          </div>

          <div class="project-body">
            <h4 class="project-title">{{ p.title }}</h4>
            <p class="project-tech">
              <span v-for="t in p.tech" :key="t" class="chip">{{ t }}</span>
            </p>
          </div>
        </button>
      </div>
    </div>

    <div class="portfolio-section">
      <h3>Academic Projects</h3>
      <div class="gallery">
        <button
          v-for="p in academicProjects"
          :key="p.title"
          class="project-card"
          type="button"
          @click="openModal(p)"
        >
          <div class="project-media">
            <img
              class="project-img"
              :src="p.cover"
              :alt="p.title"
              loading="lazy"
            />
            <div class="project-overlay"></div>

            <div class="project-actions">
              <a
                v-if="p.demo"
                class="action-btn"
                :href="p.demo"
                target="_blank"
                rel="noreferrer"
                @click.stop
              >
                Live
              </a>
              <a
                v-if="p.code"
                class="action-btn ghost"
                :href="p.code"
                target="_blank"
                rel="noreferrer"
                @click.stop
              >
                Code
              </a>
            </div>
          </div>

          <div class="project-body">
            <h4 class="project-title">{{ p.title }}</h4>
            <p class="project-tech">
              <span v-for="t in p.tech" :key="t" class="chip">{{ t }}</span>
            </p>
          </div>
        </button>
      </div>
    </div>
  </section>

  <!-- CONTACT -->
  <section id="contact">
    <h2>Contact</h2>
    <div class="contact-container">
      <div class="contact-info">
        <p>
          <strong>Email:</strong>
          <a href="mailto:jhurtadomije@gmail.com">jhurtadomije@gmail.com</a>
        </p>
        <p><strong>Location:</strong> Granada · Remote</p>
      </div>
    </div>
  </section>

  <footer>
    <div class="container">
      <p>
        &copy; {{ new Date().getFullYear() }} José Ramón Hurtado. All rights
        reserved.
      </p>
    </div>
  </footer>

  <!-- MODAL -->
  <div v-if="modalOpen" class="modal" @click.self="closeModal">
    <div class="modal-content">
      <button class="close-modal" type="button" @click="closeModal">×</button>

      <h3>{{ selected?.title }}</h3>
      <img
        v-if="activeImg"
        :src="activeImg"
        :alt="selected?.title"
        style="width: 100%; border-radius: 10px; margin: 10px 0"
      />
      <p>{{ selected?.description }}</p>
      <p><strong>Tech:</strong> {{ selected?.tech?.join(" · ") }}</p>

      <div class="modal-buttons">
        <a
          v-if="selected?.demo"
          class="btn"
          :href="selected.demo"
          target="_blank"
          rel="noreferrer"
        >
          Live
        </a>

        <a
          v-if="selected?.code"
          class="btn btn-secondary"
          :href="selected.code"
          target="_blank"
          rel="noreferrer"
        >
          Code
        </a>
      </div>

      <div v-if="selected?.images?.length" class="thumbs">
        <button
          v-for="img in selected.images"
          :key="img"
          type="button"
          class="thumb"
          @click="activeImg = img"
        >
          <img :src="img" :alt="selected?.title" />
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
}
.menu {
  position: relative; /* para que el desplegable se ancle aquí */
}
.menuHamburguesa {
  background: transparent;
  border: 0;
  cursor: pointer;
  display: none; /* desktop: oculto */
}
.line {
  width: 26px;
  height: 2px;
  background: #fff;
  margin: 5px 0;
}

/* Desktop menu */
.menu-list {
  list-style: none;
  display: flex;
  gap: 12px;
  margin: 0;
  padding: 0;
}
/* Mobile menu */
@media (max-width: 768px) {
  .menuHamburguesa {
    display: block; /* móvil: visible */
  }

  .menu-list {
    display: none; /* cerrado por defecto */
    position: absolute;
    top: 100%;
    right: 0;
    left: 0;
    flex-direction: column;
    gap: 10px;
    padding: 12px 16px;
    background: rgba(0, 0, 0, 0.85);
    backdrop-filter: blur(8px);
    border: 1px solid rgba(255, 255, 255, 0.12);
    border-radius: 14px;
    margin-top: 10px;
  }

  .menu-list.open {
    display: flex; /* abierto */
  }
}

.hero {
  padding: 32px 16px;
}
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 16px;
}

.project-card {
  text-align: left;
  border: 1px solid rgba(255, 255, 255, 0.12);
  background: rgba(17, 17, 17, 0.35);
  padding: 0;
  cursor: pointer;
  border-radius: 16px;
  overflow: hidden;
  transition: transform 160ms ease, border-color 160ms ease;
}

.project-card:hover {
  transform: translateY(-4px);
  border-color: rgba(255, 255, 255, 0.22);
}

.project-media {
  position: relative;
  aspect-ratio: 16 / 9;
  overflow: hidden;
}

.project-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transform: scale(1);
  transition: transform 220ms ease;
}

.project-card:hover .project-img {
  transform: scale(1.04);
}

.project-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to top,
    rgba(0, 0, 0, 0.7),
    rgba(0, 0, 0, 0.15) 55%,
    rgba(0, 0, 0, 0)
  );
}

.project-actions {
  position: absolute;
  right: 12px;
  bottom: 12px;
  display: flex;
  gap: 8px;
}

.action-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 8px 12px;
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.22);
  text-decoration: none;
  color: #fff;
  background: rgba(0, 0, 0, 0.35);
  backdrop-filter: blur(6px);
  font-size: 14px;
}

.action-btn.ghost {
  background: transparent;
}

.project-body {
  padding: 14px 14px 16px;
}

.project-title {
  margin: 0 0 10px;
  font-size: 16px;
  font-weight: 700;
}

.project-tech {
  margin: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.chip {
  display: inline-flex;
  padding: 5px 10px;
  border-radius: 999px;
  font-size: 12px;
  border: 1px solid rgba(255, 255, 255, 0.14);
  opacity: 0.92;
}

.modal {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
}
.modal-content {
  background: #111;
  color: #fff;
  max-width: 720px;
  width: 100%;
  padding: 16px;
  border-radius: 12px;
}
.close-modal {
  float: right;
  background: transparent;
  border: 0;
  color: #fff;
  font-size: 26px;
  cursor: pointer;
}
.modal-buttons {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}
.btn {
  display: inline-block;
  padding: 10px 14px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  text-decoration: none;
  color: #fff;
}
.btn-secondary {
  opacity: 0.85;
}
.thumbs {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin-top: 10px;
}
.thumb {
  padding: 0;
  border: 1px solid rgba(255, 255, 255, 0.18);
  background: transparent;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
}
.thumb img {
  width: 84px;
  height: 54px;
  object-fit: cover;
  display: block;
}
.btn.small {
  padding: 8px 12px;
  font-size: 14px;
}
.card-actions {
  padding: 0 10px 10px;
}
.skills-section {
  padding: 56px 16px;
  max-width: 1100px;
  margin: 0 auto;
}

.section-title {
  margin: 0 0 22px;
  text-align: center;
  font-size: 34px;
  font-weight: 800;
  letter-spacing: 0.5px;
}

.skills-grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 16px;
}

.skill-card {
  grid-column: span 6;
  border: 1px solid rgba(255, 255, 255, 0.12);
  background: rgba(17, 17, 17, 0.28);
  border-radius: 16px;
  padding: 16px;
  transition: transform 160ms ease, border-color 160ms ease;
}

.skill-card:hover {
  transform: translateY(-3px);
  border-color: rgba(255, 255, 255, 0.22);
}

.skill-card h4 {
  margin: 0 0 8px;
  font-size: 16px;
  font-weight: 800;
}

.skill-card p {
  margin: 0;
  opacity: 0.9;
  line-height: 1.45;
}

@media (max-width: 760px) {
  .skill-card {
    grid-column: span 12;
  }
  .section-title {
    font-size: 28px;
  }
}
</style>
