<template>
  <div id="nexus-app">
    <!-- Cursor -->
    <div class="cursor" :style="{ left: cursor.x + 'px', top: cursor.y + 'px' }" :class="{ active: cursor.active }"></div>
    <div class="cursor-dot" :style="{ left: cursor.x + 'px', top: cursor.y + 'px' }"></div>

    <!-- Navbar -->
    <nav class="navbar" :class="{ scrolled: isScrolled, 'menu-open': menuOpen }">
      <div class="container nav-inner">
        <router-link to="/" class="nav-logo" @click="menuOpen = false">
          <span class="logo-text">NEXUS</span>
          <span class="logo-dot">.</span>
        </router-link>

        <ul class="nav-links" :class="{ open: menuOpen }">
          <li v-for="link in navLinks" :key="link.to">
            <router-link :to="link.to" @click="menuOpen = false" class="nav-link">
              <span class="link-num">{{ link.num }}</span>
              {{ link.label }}
            </router-link>
          </li>
        </ul>

        <div class="nav-right">
          <router-link to="/contact" class="btn-cta" @click="menuOpen = false">
            Let's Talk
          </router-link>
          <button class="hamburger" @click="menuOpen = !menuOpen" :class="{ open: menuOpen }" aria-label="Toggle menu">
            <span></span><span></span><span></span>
          </button>
        </div>
      </div>
    </nav>

    <!-- Page View -->
    <main>
      <router-view v-slot="{ Component }">
        <transition name="page" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </main>

    <!-- Footer -->
    <footer class="footer">
      <div class="container footer-inner">
        <div class="footer-left">
          <p class="footer-logo">NEXUS<span>.</span></p>
          <p class="footer-tagline">Crafting digital experiences that matter.</p>
        </div>
        <div class="footer-links">
          <router-link v-for="link in navLinks" :key="link.to" :to="link.to">{{ link.label }}</router-link>
        </div>
        <div class="footer-bottom">
          <span>© 2025 NEXUS Studio. All rights reserved.</span>
          <span class="footer-status"><span class="status-dot"></span> All systems operational</span>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      isScrolled: false,
      menuOpen: false,
      cursor: { x: -100, y: -100, active: false },
      navLinks: [
        { to: '/', label: 'Home', num: '01' },
        { to: '/work', label: 'Work', num: '02' },
        { to: '/services', label: 'Services', num: '03' },
        { to: '/contact', label: 'Contact', num: '04' },
      ],
    }
  },
  mounted() {
    window.addEventListener('scroll', this.onScroll)
    window.addEventListener('mousemove', this.onMouse)
    document.querySelectorAll('a, button').forEach(el => {
      el.addEventListener('mouseenter', () => (this.cursor.active = true))
      el.addEventListener('mouseleave', () => (this.cursor.active = false))
    })
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.onScroll)
    window.removeEventListener('mousemove', this.onMouse)
  },
  methods: {
    onScroll() {
      this.isScrolled = window.scrollY > 60
    },
    onMouse(e) {
      this.cursor.x = e.clientX
      this.cursor.y = e.clientY
    },
  },
}
</script>

<style scoped>
/* ---- Custom Cursor ---- */
.cursor {
  position: fixed;
  width: 36px;
  height: 36px;
  border: 1.5px solid var(--acid);
  border-radius: 50%;
  pointer-events: none;
  z-index: 9999;
  transform: translate(-50%, -50%);
  transition: width 0.3s, height 0.3s, border-color 0.3s, background 0.3s;
  mix-blend-mode: difference;
}
.cursor.active {
  width: 60px;
  height: 60px;
  background: rgba(200, 245, 59, 0.1);
}
.cursor-dot {
  position: fixed;
  width: 5px;
  height: 5px;
  background: var(--acid);
  border-radius: 50%;
  pointer-events: none;
  z-index: 10000;
  transform: translate(-50%, -50%);
  transition: none;
}
@media (max-width: 768px) {
  .cursor, .cursor-dot { display: none; }
}

/* ---- Navbar ---- */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  padding: 1.5rem 0;
  transition: background var(--transition), padding var(--transition), border-bottom var(--transition);
  border-bottom: 1px solid transparent;
}
.navbar.scrolled {
  background: rgba(8, 8, 8, 0.92);
  backdrop-filter: blur(20px);
  padding: 1rem 0;
  border-bottom-color: var(--gray-3);
}
.nav-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
}
.nav-logo {
  font-family: var(--font-display);
  font-size: 1.8rem;
  letter-spacing: 0.1em;
  line-height: 1;
}
.logo-dot {
  color: var(--acid);
}
.nav-links {
  display: flex;
  align-items: center;
  gap: 2.5rem;
}
.nav-link {
  font-size: 0.85rem;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--gray-6);
  transition: color var(--transition);
  display: flex;
  align-items: center;
  gap: 0.4rem;
}
.nav-link:hover, .nav-link.router-link-exact-active {
  color: var(--white);
}
.nav-link.router-link-exact-active {
  color: var(--acid);
}
.link-num {
  font-family: var(--font-mono);
  font-size: 0.65rem;
  color: var(--gray-4);
}
.nav-right {
  display: flex;
  align-items: center;
  gap: 1rem;
}
.btn-cta {
  display: inline-flex;
  align-items: center;
  padding: 0.55rem 1.4rem;
  background: var(--acid);
  color: var(--black);
  font-size: 0.8rem;
  font-weight: 500;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  border-radius: var(--radius);
  transition: background var(--transition), transform var(--transition);
}
.btn-cta:hover {
  background: var(--acid-dim);
  transform: translateY(-1px);
}
.hamburger {
  display: none;
  flex-direction: column;
  justify-content: center;
  gap: 5px;
  width: 32px;
  height: 32px;
  padding: 4px;
}
.hamburger span {
  display: block;
  width: 100%;
  height: 1.5px;
  background: var(--white);
  transition: transform var(--transition), opacity var(--transition);
  transform-origin: center;
}
.hamburger.open span:nth-child(1) { transform: translateY(6.5px) rotate(45deg); }
.hamburger.open span:nth-child(2) { opacity: 0; }
.hamburger.open span:nth-child(3) { transform: translateY(-6.5px) rotate(-45deg); }

/* ---- Footer ---- */
.footer {
  border-top: 1px solid var(--gray-3);
  padding: 3rem 0 2rem;
  margin-top: 0;
}
.footer-inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto auto;
  gap: 2rem;
}
.footer-logo {
  font-family: var(--font-display);
  font-size: 2rem;
  letter-spacing: 0.1em;
}
.footer-logo span { color: var(--acid); }
.footer-tagline {
  color: var(--gray-5);
  font-size: 0.9rem;
  margin-top: 0.3rem;
}
.footer-links {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  align-items: flex-end;
}
.footer-links a {
  font-size: 0.85rem;
  color: var(--gray-5);
  text-transform: uppercase;
  letter-spacing: 0.06em;
  transition: color var(--transition);
}
.footer-links a:hover { color: var(--acid); }
.footer-bottom {
  grid-column: 1 / -1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 1.5rem;
  border-top: 1px solid var(--gray-3);
  font-family: var(--font-mono);
  font-size: 0.7rem;
  color: var(--gray-4);
}
.footer-status {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: var(--acid);
}
.status-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--acid);
  animation: pulse 2s infinite;
}
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.3; }
}

/* ---- Page Transitions ---- */
.page-enter-active, .page-leave-active {
  transition: opacity 0.35s ease, transform 0.35s ease;
}
.page-enter-from { opacity: 0; transform: translateY(16px); }
.page-leave-to { opacity: 0; transform: translateY(-8px); }

/* ---- Mobile ---- */
@media (max-width: 768px) {
  .hamburger { display: flex; }
  .nav-links {
    position: fixed;
    inset: 0;
    background: var(--black);
    flex-direction: column;
    justify-content: center;
    gap: 2rem;
    font-size: 1.2rem;
    transform: translateX(100%);
    transition: transform var(--transition-slow);
  }
  .nav-links.open { transform: translateX(0); }
  .btn-cta { display: none; }
  .footer-inner { grid-template-columns: 1fr; }
  .footer-links { align-items: flex-start; }
}
</style>
