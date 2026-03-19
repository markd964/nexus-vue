<template>
  <div class="home">
    <!-- Hero -->
    <section class="hero">
      <div class="hero-bg">
        <div class="grid-lines"></div>
        <div class="orb orb-1"></div>
        <div class="orb orb-2"></div>
      </div>
      <div class="container hero-inner">
        <div class="hero-badge">
          <span class="badge-dot"></span>
          Available for new projects — 2025
        </div>
        <h1 class="hero-title">
          <span class="line animate-1">WE BUILD</span>
          <span class="line accent animate-2">DIGITAL</span>
          <span class="line animate-3">EXPERIENCES</span>
        </h1>
        <p class="hero-sub animate-4">
          A boutique studio crafting high-performance websites,<br />
          interfaces, and digital products for ambitious brands.
        </p>
        <div class="hero-actions animate-5">
          <router-link to="/work" class="btn-primary">View Our Work</router-link>
          <router-link to="/services" class="btn-ghost">Our Services</router-link>
        </div>
        <div class="hero-scroll animate-6">
          <span class="scroll-line"></span>
          <span class="scroll-label">scroll</span>
        </div>
      </div>
    </section>

    <!-- Stats -->
    <section class="stats-section">
      <div class="container">
        <div class="stats-grid">
          <div class="stat-item" v-for="stat in stats" :key="stat.label">
            <span class="stat-number">{{ stat.value }}</span>
            <span class="stat-label">{{ stat.label }}</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Marquee -->
    <div class="marquee-strip">
      <div class="marquee-track">
        <span v-for="n in 3" :key="n">
          <em v-for="tag in techTags" :key="tag + n">{{ tag }}</em>
        </span>
      </div>
    </div>

    <!-- About -->
    <section class="about-section">
      <div class="container about-grid">
        <div class="about-left">
          <p class="section-label">/ About Nexus</p>
          <h2 class="section-title">We don't just <br /><span class="accent-text">build websites</span><br />we build legacies.</h2>
        </div>
        <div class="about-right">
          <p>Founded in 2018, NEXUS is a digital studio that partners with startups and established brands to create web experiences that convert, inspire, and endure.</p>
          <p>We blend sharp design thinking with engineering precision — every pixel intentional, every interaction purposeful.</p>
          <div class="about-values">
            <div class="value-tag" v-for="v in values" :key="v">{{ v }}</div>
          </div>
        </div>
      </div>
    </section>

    <!-- Services Preview -->
    <section class="services-preview">
      <div class="container">
        <p class="section-label">/ What We Do</p>
        <div class="services-list">
          <div class="service-row" v-for="(svc, i) in services" :key="svc.title" @mouseenter="hoveredService = i" @mouseleave="hoveredService = null" :class="{ dimmed: hoveredService !== null && hoveredService !== i }">
            <div class="service-num">{{ String(i + 1).padStart(2, '0') }}</div>
            <div class="service-title">{{ svc.title }}</div>
            <div class="service-tags">
              <span v-for="tag in svc.tags" :key="tag">{{ tag }}</span>
            </div>
            <div class="service-arrow">→</div>
          </div>
        </div>
      </div>
    </section>

    <!-- Featured Work -->
    <section class="work-preview">
      <div class="container">
        <div class="section-header">
          <p class="section-label">/ Selected Work</p>
          <router-link to="/work" class="view-all">View All Projects →</router-link>
        </div>
        <div class="work-grid">
          <div class="work-card" v-for="project in featuredWork" :key="project.title" :style="{ '--accent': project.color }">
            <div class="card-visual">
              <div class="card-bg" :style="{ background: project.bg }"></div>
              <div class="card-icon">{{ project.icon }}</div>
              <div class="card-overlay">
                <span class="overlay-label">View Project →</span>
              </div>
            </div>
            <div class="card-info">
              <div class="card-meta">
                <span class="card-tag">{{ project.tag }}</span>
                <span class="card-year">{{ project.year }}</span>
              </div>
              <h3 class="card-title">{{ project.title }}</h3>
              <p class="card-desc">{{ project.desc }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Testimonials -->
    <section class="testimonials">
      <div class="container">
        <p class="section-label center">/ What Clients Say</p>
        <div class="testimonial-carousel">
          <div class="testimonial-track" :style="{ transform: `translateX(-${activeTestimonial * 100}%)` }">
            <div class="testimonial-slide" v-for="t in testimonials" :key="t.name">
              <blockquote class="quote">{{ t.quote }}</blockquote>
              <div class="quote-author">
                <div class="author-avatar">{{ t.initials }}</div>
                <div>
                  <p class="author-name">{{ t.name }}</p>
                  <p class="author-role">{{ t.role }}</p>
                </div>
              </div>
            </div>
          </div>
          <div class="carousel-dots">
            <button v-for="(t, i) in testimonials" :key="i" :class="{ active: i === activeTestimonial }" @click="activeTestimonial = i"></button>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section class="cta-section">
      <div class="container">
        <div class="cta-box">
          <div class="cta-glow"></div>
          <p class="cta-label">Ready to start?</p>
          <h2 class="cta-title">Let's Build Something <span>Remarkable</span></h2>
          <router-link to="/contact" class="btn-primary large">Start a Project</router-link>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  data() {
    return {
      hoveredService: null,
      activeTestimonial: 0,
      testimonialTimer: null,
      stats: [
        { value: '120+', label: 'Projects Shipped' },
        { value: '7yr', label: 'In Business' },
        { value: '98%', label: 'Client Retention' },
        { value: '3s', label: 'Avg Load Time' },
      ],
      techTags: ['Vue.js', 'React', 'TypeScript', 'Vercel', 'Node.js', 'Figma', 'Tailwind', 'GraphQL', 'WebGL', 'GSAP', 'Three.js', 'Next.js'],
      values: ['Speed', 'Precision', 'Creativity', 'Scalability', 'Partnership'],
      services: [
        { title: 'Web Design & Development', tags: ['UI/UX', 'Vue', 'React'] },
        { title: 'Brand Identity Systems', tags: ['Logo', 'Typography', 'Color'] },
        { title: 'E-Commerce Solutions', tags: ['Shopify', 'Custom', 'Payments'] },
        { title: 'Performance Optimization', tags: ['Core Web Vitals', 'SEO'] },
        { title: 'Motion & Animation', tags: ['GSAP', 'WebGL', 'Three.js'] },
      ],
      featuredWork: [
        { title: 'Apex Finance', desc: 'Trading dashboard with real-time data visualization', tag: 'Fintech', year: '2024', icon: '📈', color: '#c8f53b', bg: 'linear-gradient(135deg, #0a1628 0%, #132340 100%)' },
        { title: 'Luma Studio', desc: 'Portfolio site for a photography collective', tag: 'Creative', year: '2024', icon: '📸', color: '#ff9f43', bg: 'linear-gradient(135deg, #1a0a00 0%, #2d1500 100%)' },
        { title: 'Drift Commerce', desc: 'Headless e-commerce with 3D product previews', tag: 'E-Commerce', year: '2023', icon: '🛍️', color: '#a29bfe', bg: 'linear-gradient(135deg, #0d0a1f 0%, #1a1435 100%)' },
        { title: 'HealthPath AI', desc: 'Patient onboarding & analytics platform', tag: 'HealthTech', year: '2023', icon: '🧬', color: '#55efc4', bg: 'linear-gradient(135deg, #001a10 0%, #002a1a 100%)' },
      ],
      testimonials: [
        { quote: '"NEXUS transformed our online presence completely. The attention to detail and speed of delivery exceeded every expectation. Our conversion rate jumped 40% in the first month."', name: 'Sarah Okonkwo', role: 'CEO, Drift Commerce', initials: 'SO' },
        { quote: '"Working with NEXUS felt like having an in-house team that actually cares. They pushed back when needed and the final product was far beyond our initial vision."', name: 'Marcus Chen', role: 'CTO, Apex Finance', initials: 'MC' },
        { quote: '"The NEXUS team delivered a Webby Award-winning experience on a tight timeline. Their process is tight, communication excellent, and the work speaks for itself."', name: 'Priya Sharma', role: 'Creative Director, Luma Studio', initials: 'PS' },
      ],
    }
  },
  mounted() {
    this.testimonialTimer = setInterval(() => {
      this.activeTestimonial = (this.activeTestimonial + 1) % this.testimonials.length
    }, 5000)
  },
  beforeUnmount() {
    clearInterval(this.testimonialTimer)
  },
}
</script>

<style scoped>
/* Hero */
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  overflow: hidden;
  padding: 8rem 0 4rem;
}
.hero-bg {
  position: absolute;
  inset: 0;
  z-index: 0;
}
.grid-lines {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(var(--gray-3) 1px, transparent 1px),
    linear-gradient(90deg, var(--gray-3) 1px, transparent 1px);
  background-size: 80px 80px;
  opacity: 0.15;
}
.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(100px);
  pointer-events: none;
}
.orb-1 {
  width: 600px; height: 600px;
  background: radial-gradient(circle, rgba(200,245,59,0.12), transparent 70%);
  top: -150px; right: -100px;
  animation: float 8s ease-in-out infinite;
}
.orb-2 {
  width: 400px; height: 400px;
  background: radial-gradient(circle, rgba(100,100,255,0.08), transparent 70%);
  bottom: 50px; left: 10%;
  animation: float 12s ease-in-out infinite reverse;
}
@keyframes float {
  0%, 100% { transform: translate(0, 0); }
  50% { transform: translate(20px, -30px); }
}
.hero-inner {
  position: relative;
  z-index: 1;
}
.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  background: var(--gray-2);
  border: 1px solid var(--gray-3);
  padding: 0.4rem 1rem;
  border-radius: 100px;
  font-size: 0.78rem;
  font-family: var(--font-mono);
  color: var(--gray-6);
  margin-bottom: 2rem;
  animation: fadeUp 0.8s ease both;
}
.badge-dot {
  width: 6px; height: 6px;
  background: var(--acid);
  border-radius: 50%;
  animation: pulse 2s infinite;
}
.hero-title {
  font-family: var(--font-display);
  font-size: clamp(5rem, 12vw, 11rem);
  line-height: 0.9;
  letter-spacing: -0.01em;
  margin-bottom: 2rem;
  display: flex;
  flex-direction: column;
}
.line { display: block; }
.line.accent { color: var(--acid); }
.animate-1 { animation: fadeUp 0.8s 0.1s ease both; }
.animate-2 { animation: fadeUp 0.8s 0.2s ease both; }
.animate-3 { animation: fadeUp 0.8s 0.3s ease both; }
.animate-4 { animation: fadeUp 0.8s 0.4s ease both; }
.animate-5 { animation: fadeUp 0.8s 0.5s ease both; }
.animate-6 { animation: fadeUp 0.8s 0.6s ease both; }
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(40px); }
  to { opacity: 1; transform: translateY(0); }
}
.hero-sub {
  font-size: 1.1rem;
  color: var(--gray-6);
  max-width: 480px;
  line-height: 1.7;
  margin-bottom: 2.5rem;
}
.hero-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
  flex-wrap: wrap;
}
.btn-primary {
  display: inline-flex;
  align-items: center;
  padding: 0.9rem 2rem;
  background: var(--acid);
  color: var(--black);
  font-weight: 500;
  font-size: 0.9rem;
  letter-spacing: 0.04em;
  border-radius: var(--radius);
  transition: transform var(--transition), background var(--transition);
}
.btn-primary:hover { transform: translateY(-2px); background: var(--acid-dim); }
.btn-primary.large { font-size: 1rem; padding: 1.1rem 2.5rem; }
.btn-ghost {
  display: inline-flex;
  align-items: center;
  padding: 0.9rem 2rem;
  border: 1px solid var(--gray-3);
  color: var(--white);
  font-size: 0.9rem;
  border-radius: var(--radius);
  transition: border-color var(--transition), color var(--transition);
}
.btn-ghost:hover { border-color: var(--acid); color: var(--acid); }
.hero-scroll {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-top: 5rem;
  color: var(--gray-5);
  font-family: var(--font-mono);
  font-size: 0.7rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}
.scroll-line {
  display: block;
  width: 60px;
  height: 1px;
  background: var(--gray-4);
  position: relative;
  overflow: hidden;
}
.scroll-line::after {
  content: '';
  position: absolute;
  top: 0; left: -100%;
  width: 100%; height: 100%;
  background: var(--acid);
  animation: scanline 2s ease-in-out infinite;
}
@keyframes scanline {
  0% { left: -100%; } 100% { left: 100%; }
}

/* Stats */
.stats-section {
  padding: 4rem 0;
  border-top: 1px solid var(--gray-3);
  border-bottom: 1px solid var(--gray-3);
  background: var(--gray-1);
}
.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 2rem;
}
.stat-item {
  text-align: center;
  padding: 1rem;
}
.stat-number {
  display: block;
  font-family: var(--font-display);
  font-size: 3.5rem;
  color: var(--acid);
  line-height: 1;
}
.stat-label {
  display: block;
  font-size: 0.8rem;
  color: var(--gray-5);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-top: 0.5rem;
}

/* Marquee */
.marquee-strip {
  padding: 1.2rem 0;
  background: var(--acid);
  overflow: hidden;
  white-space: nowrap;
}
.marquee-track {
  display: inline-flex;
  animation: marquee 20s linear infinite;
}
.marquee-track span {
  display: inline-flex;
}
.marquee-track em {
  font-style: normal;
  font-family: var(--font-display);
  font-size: 1rem;
  color: var(--black);
  letter-spacing: 0.05em;
  padding: 0 2rem;
}
.marquee-track em::after {
  content: '·';
  margin-left: 2rem;
}
@keyframes marquee {
  from { transform: translateX(0); }
  to { transform: translateX(-33.333%); }
}

/* About */
.about-section {
  padding: 8rem 0;
}
.about-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 5rem;
  align-items: start;
}
.section-label {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--acid);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  margin-bottom: 1.5rem;
}
.section-label.center { text-align: center; }
.section-title {
  font-family: var(--font-display);
  font-size: clamp(2.5rem, 5vw, 4rem);
  line-height: 1.05;
  letter-spacing: 0.02em;
}
.accent-text { color: var(--acid); }
.about-right p {
  color: var(--gray-6);
  line-height: 1.8;
  margin-bottom: 1.2rem;
  font-size: 1.05rem;
}
.about-values {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 1.5rem;
}
.value-tag {
  padding: 0.3rem 0.9rem;
  border: 1px solid var(--gray-3);
  border-radius: 100px;
  font-size: 0.78rem;
  color: var(--gray-6);
  font-family: var(--font-mono);
}

/* Services List */
.services-preview {
  padding: 0 0 8rem;
}
.services-list {
  margin-top: 2rem;
  border-top: 1px solid var(--gray-3);
}
.service-row {
  display: grid;
  grid-template-columns: 80px 1fr auto 40px;
  align-items: center;
  padding: 1.8rem 0;
  border-bottom: 1px solid var(--gray-3);
  cursor: pointer;
  transition: opacity var(--transition);
  gap: 2rem;
}
.service-row.dimmed { opacity: 0.3; }
.service-num {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--gray-4);
}
.service-title {
  font-size: 1.3rem;
  font-weight: 400;
  transition: color var(--transition);
}
.service-row:hover .service-title { color: var(--acid); }
.service-tags {
  display: flex;
  gap: 0.5rem;
}
.service-tags span {
  padding: 0.2rem 0.7rem;
  background: var(--gray-2);
  border-radius: 100px;
  font-size: 0.72rem;
  color: var(--gray-5);
  font-family: var(--font-mono);
}
.service-arrow {
  font-size: 1.2rem;
  color: var(--gray-4);
  transition: transform var(--transition), color var(--transition);
}
.service-row:hover .service-arrow {
  transform: translateX(5px);
  color: var(--acid);
}

/* Work Preview */
.work-preview { padding: 0 0 8rem; }
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2.5rem;
}
.view-all {
  font-size: 0.85rem;
  color: var(--gray-5);
  transition: color var(--transition);
  font-family: var(--font-mono);
}
.view-all:hover { color: var(--acid); }
.work-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
}
.work-card {
  border-radius: var(--radius-lg);
  overflow: hidden;
  background: var(--gray-1);
  border: 1px solid var(--gray-3);
  transition: transform var(--transition), border-color var(--transition);
}
.work-card:hover {
  transform: translateY(-6px);
  border-color: var(--accent);
}
.card-visual {
  position: relative;
  height: 220px;
  overflow: hidden;
}
.card-bg {
  position: absolute;
  inset: 0;
}
.card-icon {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 4rem;
  z-index: 1;
}
.card-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity var(--transition);
  z-index: 2;
}
.work-card:hover .card-overlay { opacity: 1; }
.overlay-label {
  font-family: var(--font-mono);
  font-size: 0.85rem;
  color: var(--accent);
  letter-spacing: 0.05em;
}
.card-info { padding: 1.5rem; }
.card-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.7rem;
}
.card-tag {
  font-size: 0.72rem;
  font-family: var(--font-mono);
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: 0.08em;
}
.card-year {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--gray-4);
}
.card-title {
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}
.card-desc {
  font-size: 0.85rem;
  color: var(--gray-5);
  line-height: 1.6;
}

/* Testimonials */
.testimonials { padding: 6rem 0; background: var(--gray-1); }
.testimonial-carousel { overflow: hidden; margin-top: 3rem; }
.testimonial-track {
  display: flex;
  transition: transform 0.6s cubic-bezier(0.4,0,0.2,1);
}
.testimonial-slide {
  min-width: 100%;
  padding: 0 4rem;
  text-align: center;
}
.quote {
  font-size: clamp(1rem, 2vw, 1.3rem);
  line-height: 1.7;
  color: var(--gray-6);
  font-style: normal;
  margin-bottom: 2.5rem;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}
.quote-author {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}
.author-avatar {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--gray-3);
  border: 2px solid var(--acid);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--acid);
}
.author-name {
  font-weight: 500;
  font-size: 0.95rem;
}
.author-role {
  font-size: 0.8rem;
  color: var(--gray-5);
  font-family: var(--font-mono);
}
.carousel-dots {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin-top: 2rem;
}
.carousel-dots button {
  width: 6px;
  height: 6px;
  border-radius: 3px;
  background: var(--gray-3);
  transition: width var(--transition), background var(--transition);
}
.carousel-dots button.active {
  width: 24px;
  background: var(--acid);
}

/* CTA */
.cta-section { padding: 6rem 0 8rem; }
.cta-box {
  position: relative;
  text-align: center;
  padding: 6rem 2rem;
  border: 1px solid var(--gray-3);
  border-radius: var(--radius-lg);
  overflow: hidden;
  background: var(--gray-1);
}
.cta-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 500px;
  height: 300px;
  background: radial-gradient(ellipse, rgba(200,245,59,0.1) 0%, transparent 70%);
  pointer-events: none;
}
.cta-label {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--acid);
  letter-spacing: 0.15em;
  text-transform: uppercase;
  margin-bottom: 1rem;
}
.cta-title {
  font-family: var(--font-display);
  font-size: clamp(2.5rem, 6vw, 5rem);
  letter-spacing: 0.03em;
  margin-bottom: 2.5rem;
  line-height: 1;
}
.cta-title span { color: var(--acid); }

/* Responsive */
@media (max-width: 900px) {
  .stats-grid { grid-template-columns: repeat(2, 1fr); }
  .about-grid { grid-template-columns: 1fr; gap: 3rem; }
  .work-grid { grid-template-columns: 1fr; }
  .service-row { grid-template-columns: 60px 1fr 40px; }
  .service-tags { display: none; }
}
@media (max-width: 600px) {
  .hero-title { font-size: clamp(3.5rem, 16vw, 5rem); }
  .stats-grid { grid-template-columns: repeat(2, 1fr); gap: 1rem; }
  .testimonial-slide { padding: 0 1rem; }
}
</style>
