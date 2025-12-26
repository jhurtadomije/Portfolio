<script setup>
import { ref, watch } from "vue";
import { professionalProjects, academicProjects } from "./data/projects";
import { onMounted, onBeforeUnmount } from "vue";
import { assetUrl } from "./utils/assetUrl";

function handleResize() {
  // Al cambiar de tamaño, cerramos el menú siempre
  menuOpen.value = false;
}
onMounted(() => {
  handleResize();
  window.addEventListener("resize", handleResize);
  const tiltEls = document.querySelectorAll(".tilt");
  const clamp = (n, min, max) => Math.max(min, Math.min(max, n));

  tiltEls.forEach((el) => {
    let raf = 0;

    const onMove = (e) => {
      const r = el.getBoundingClientRect();
      const x = (e.clientX - r.left) / r.width; // 0..1
      const y = (e.clientY - r.top) / r.height; // 0..1
      const rx = clamp((0.5 - y) * 10, -8, 8); // rotX
      const ry = clamp((x - 0.5) * 12, -10, 10); // rotY

      cancelAnimationFrame(raf);
      raf = requestAnimationFrame(() => {
        el.style.transform = `perspective(900px) rotateX(${rx}deg) rotateY(${ry}deg) translateY(-2px)`;
      });
    };

    const onLeave = () => {
      cancelAnimationFrame(raf);
      el.style.transform = `perspective(900px) rotateX(0deg) rotateY(0deg) translateY(0px)`;
    };

    el.addEventListener("mousemove", onMove);
    el.addEventListener("mouseleave", onLeave);
  });

  const els = document.querySelectorAll(".reveal");
  const io = new IntersectionObserver(
    (entries) => {
      entries.forEach((e) => {
        if (e.isIntersecting) e.target.classList.add("in");
      });
    },
    { threshold: 0.12 }
  );

  els.forEach((el) => io.observe(el));
  // ---- Typing effect (hero subtitle) ----
  const reduced = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
  if (!reduced) {
    let i = 0;
    const tick = () => {
      typingText.value = typingFull.slice(0, i);
      i++;
      if (i <= typingFull.length) setTimeout(tick, i < 20 ? 24 : 14);
    };
    setTimeout(tick, 250);
  } else {
    typingText.value = typingFull;
  }
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
const typingText = ref("");
const typingFull =
  "Full Stack Web Developer · React · Vue · Node.js · MySQL · Remote";
</script>

<template>
  <header id="header" class="header">
    <div class="container header-inner">
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
    </div>
  </header>

  <!-- HERO -->
  <section id="home" class="hero reveal">
    <div class="container hero-inner">
      <div class="hero-content">
        <h1 class="hero-title">Hi, I’m José Ramón</h1>
        <p class="hero-subtitle">
          <span class="dot"></span>
          <span class="typing">{{ typingText }}</span>
          <span class="caret" aria-hidden="true"></span>
        </p>

        <div class="hero-buttons">
          <a href="#portfolio" class="btn">View Projects</a>
          <a
            class="btn btn-secondary"
            :href="assetUrl('/jhurtadomije_cv_es.pdf')"
            target="_blank"
            rel="noreferrer"
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

      <div class="hero-image-container reveal">
        <img src="/imagenes/about.jpg" alt="Tech" class="hero-image" />
      </div>
    </div>
  </section>

  <!-- ABOUT -->
  <section id="about" class="about reveal">
    <div class="container">
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
    </div>
  </section>

  <!-- SERVICES -->
  <section id="services" class="section">
    <div class="container">
      <div class="animated-title"><h2>Services</h2></div>

      <div class="services reveal">
        <div class="service tilt">
          <i class="fas fa-laptop-code service-icon" aria-hidden="true"></i>
          <h3 class="tilt-pop">Web Development</h3>
          <p>Modern, responsive websites and web apps.</p>
        </div>

        <div class="service tilt">
          <i class="fas fa-server service-icon" aria-hidden="true"></i>
          <h3 class="tilt-pop">APIs & Backend</h3>
          <p>REST APIs, business logic and database integration.</p>
        </div>

        <div class="service tilt">
          <i class="fas fa-tools service-icon" aria-hidden="true"></i>
          <h3 class="tilt-pop">Maintenance</h3>
          <p>Improvements, bug fixes, performance and deployment.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- SKILLS -->
  <section id="skills" class="skills-section reveal">
    <div class="container">
      <h2 class="section-title">Skills</h2>

      <div class="skills-grid">
        <div class="skill-card tilt">
          <h4 class="tilt-pop">Frontend</h4>
          <p>React · Vue 3 · HTML5 · CSS3 · SASS</p>
        </div>

        <div class="skill-card tilt">
          <h4 class="tilt-pop">Backend</h4>
          <p>Node.js · Express · REST APIs · Laravel (php)</p>
        </div>

        <div class="skill-card tilt">
          <h4 class="tilt-pop">Databases</h4>
          <p>MySQL · PostgreSQL · Firebase (Auth/Firestore)</p>
        </div>

        <div class="skill-card">
          <h4>Tooling</h4>
          <p>Git/GitHub · Vite · Docker (basic) · Deploy</p>
        </div>
      </div>
    </div>
  </section>

  <!-- PROJECTS -->
  <section id="portfolio" class="portfolio-section reveal">
    <div class="container">
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
                :src="assetUrl(p.cover)"
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
        <div class="container">
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
                  :src="assetUrl(p.cover)"
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
      </div>
    </div>
  </section>

  <!-- CONTACT -->
  <section id="contact" class="contact-section reveal">
    <div class="container">
      <h2 class="section-title">Contact</h2>

      <div class="contact-card">
        <p class="contact-lead">
          Available for remote full-time roles and freelance projects.
        </p>

        <div class="contact-grid">
          <div class="contact-item">
            <span class="contact-label">Email</span>
            <a class="contact-link" href="mailto:jhurtadomije@gmail.com">
              jhurtadomije@gmail.com
            </a>
          </div>

          <div class="contact-item">
            <span class="contact-label">Location</span>
            <span class="contact-text">Granada · Remote</span>
          </div>

          <div class="contact-item">
            <span class="contact-label">Links</span>
            <div class="contact-actions">
              <a
                class="btn small"
                href="https://github.com/jhurtadomije"
                target="_blank"
                rel="noreferrer"
              >
                GitHub
              </a>
              <a
                class="btn small btn-secondary"
                href="https://www.linkedin.com/in/jose-ramon-hurtado-mije-888629342"
                target="_blank"
                rel="noreferrer"
              >
                LinkedIn
              </a>
              <a
                class="btn small btn-secondary"
                :href="assetUrl('/jhurtadomije_cv_es.pdf')"
                target="_blank"
                rel="noreferrer"
              >
                CV
              </a>
            </div>
          </div>
        </div>
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
        :src="assetUrl(activeImg)"
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
          <img :src="assetUrl(img)" :alt="selected?.title" />
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
