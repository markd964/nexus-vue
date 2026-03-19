<template>
  <div class="work-page">
    <div class="page-hero">
      <div class="container">
        <p class="section-label">/ Our Work</p>
        <h1 class="page-title">Selected<br /><span>Projects</span></h1>
        <p class="page-sub">A curated selection of our most impactful work across industries and disciplines.</p>
      </div>
    </div>

    <!-- Filter Bar -->
    <div class="filter-bar">
      <div class="container filter-inner">
        <button
          v-for="f in filters"
          :key="f"
          class="filter-btn"
          :class="{ active: activeFilter === f }"
          @click="activeFilter = f"
        >{{ f }}</button>
      </div>
    </div>

    <!-- Projects Grid -->
    <div class="container projects-container">
      <transition-group name="project" tag="div" class="projects-grid">
        <div
          v-for="project in filteredProjects"
          :key="project.id"
          class="project-card"
          :class="project.size"
          :style="{ '--accent': project.color }"
        >
          <div class="proj-visual" :style="{ background: project.bg }">
            <div class="proj-emoji">{{ project.icon }}</div>
            <div class="proj-hover-layer">
              <div class="proj-hover-content">
                <p class="proj-category">{{ project.category }}</p>
                <h3>{{ project.title }}</h3>
                <p class="proj-tags-list">{{ project.stack.join(' · ') }}</p>
              </div>
            </div>
          </div>
          <div class="proj-footer">
            <div>
              <span class="proj-cat-badge">{{ project.category }}</span>
              <h3 class="proj-name">{{ project.title }}</h3>
            </div>
            <span class="proj-yr">{{ project.year }}</span>
          </div>
        </div>
      </transition-group>
      <p v-if="filteredProjects.length === 0" class="empty-state">No projects in this category yet.</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WorkView',
  data() {
    return {
      activeFilter: 'All',
      filters: ['All', 'Web App', 'E-Commerce', 'Branding', 'Motion', 'HealthTech'],
      projects: [
        { id: 1, title: 'Apex Finance Dashboard', category: 'Web App', year: '2024', icon: '📊', color: '#c8f53b', bg: 'linear-gradient(135deg,#0a1628,#132340)', size: 'wide', stack: ['Vue.js', 'D3.js', 'Node'] },
        { id: 2, title: 'Drift Commerce', category: 'E-Commerce', year: '2024', icon: '🛍️', color: '#a29bfe', bg: 'linear-gradient(135deg,#0d0a1f,#1a1435)', size: 'normal', stack: ['React', 'Shopify', '3D'] },
        { id: 3, title: 'Luma Photography', category: 'Branding', year: '2024', icon: '📸', color: '#ff9f43', bg: 'linear-gradient(135deg,#1a0a00,#2d1500)', size: 'normal', stack: ['Figma', 'Nuxt', 'GSAP'] },
        { id: 4, title: 'HealthPath AI', category: 'HealthTech', year: '2023', icon: '🧬', color: '#55efc4', bg: 'linear-gradient(135deg,#001a10,#002a1a)', size: 'wide', stack: ['React', 'Python', 'AI/ML'] },
        { id: 5, title: 'Nova Brand System', category: 'Branding', year: '2023', icon: '✦', color: '#fd79a8', bg: 'linear-gradient(135deg,#1a001a,#2d0028)', size: 'normal', stack: ['Figma', 'Brand', 'Motion'] },
        { id: 6, title: 'Kinetic Motion Reel', category: 'Motion', year: '2023', icon: '🎬', color: '#fdcb6e', bg: 'linear-gradient(135deg,#1a1200,#2d2000)', size: 'normal', stack: ['After Effects', 'GSAP', 'WebGL'] },
        { id: 7, title: 'Bloom Wellness App', category: 'HealthTech', year: '2023', icon: '🌿', color: '#00b894', bg: 'linear-gradient(135deg,#001a0f,#002d1f)', size: 'normal', stack: ['Vue.js', 'Firebase', 'PWA'] },
        { id: 8, title: 'Solaris E-Shop', category: 'E-Commerce', year: '2022', icon: '☀️', color: '#e17055', bg: 'linear-gradient(135deg,#1a0800,#2d1400)', size: 'wide', stack: ['Next.js', 'Stripe', 'Sanity'] },
      ],
    }
  },
  computed: {
    filteredProjects() {
      if (this.activeFilter === 'All') return this.projects
      return this.projects.filter(p => p.category === this.activeFilter)
    },
  },
}
</script>

<style scoped>
.work-page { padding-top: 6rem; min-height: 100vh; }
.page-hero {
  padding: 5rem 0 3rem;
  border-bottom: 1px solid var(--gray-3);
}
.section-label {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--acid);
  letter-spacing: 0.15em;
  text-transform: uppercase;
  margin-bottom: 1.5rem;
}
.page-title {
  font-family: var(--font-display);
  font-size: clamp(4rem, 10vw, 8rem);
  line-height: 0.95;
  margin-bottom: 1.5rem;
}
.page-title span { color: var(--acid); }
.page-sub {
  color: var(--gray-5);
  max-width: 500px;
  font-size: 1rem;
  line-height: 1.7;
}

/* Filter Bar */
.filter-bar {
  padding: 1.5rem 0;
  border-bottom: 1px solid var(--gray-3);
  position: sticky;
  top: 70px;
  background: rgba(8,8,8,0.95);
  backdrop-filter: blur(20px);
  z-index: 100;
}
.filter-inner {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
}
.filter-btn {
  padding: 0.4rem 1.1rem;
  border-radius: 100px;
  font-size: 0.8rem;
  letter-spacing: 0.04em;
  color: var(--gray-5);
  border: 1px solid var(--gray-3);
  background: transparent;
  font-family: var(--font-body);
  transition: all var(--transition);
}
.filter-btn:hover { color: var(--white); border-color: var(--gray-4); }
.filter-btn.active {
  background: var(--acid);
  color: var(--black);
  border-color: var(--acid);
  font-weight: 500;
}

/* Projects Grid */
.projects-container { padding: 3rem 0 6rem; }
.projects-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  align-items: start;
}
.project-card {
  border-radius: var(--radius-lg);
  overflow: hidden;
  background: var(--gray-1);
  border: 1px solid var(--gray-3);
  transition: transform var(--transition), border-color var(--transition);
  cursor: pointer;
}
.project-card.wide { grid-column: span 2; }
.project-card:hover { transform: translateY(-4px); border-color: var(--accent); }

.proj-visual {
  position: relative;
  height: 240px;
  overflow: hidden;
}
.project-card.wide .proj-visual { height: 300px; }
.proj-emoji {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 4rem;
}
.proj-hover-layer {
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.75);
  opacity: 0;
  transition: opacity var(--transition);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}
.project-card:hover .proj-hover-layer { opacity: 1; }
.proj-hover-content { text-align: center; }
.proj-category {
  font-family: var(--font-mono);
  font-size: 0.7rem;
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: 0.12em;
  margin-bottom: 0.5rem;
}
.proj-hover-content h3 {
  font-family: var(--font-display);
  font-size: 1.8rem;
  color: var(--white);
  margin-bottom: 0.75rem;
}
.proj-tags-list {
  font-size: 0.78rem;
  color: var(--gray-5);
  font-family: var(--font-mono);
}
.proj-footer {
  padding: 1.2rem 1.4rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.proj-cat-badge {
  font-family: var(--font-mono);
  font-size: 0.65rem;
  color: var(--accent);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  display: block;
  margin-bottom: 0.2rem;
}
.proj-name {
  font-size: 1rem;
  font-weight: 400;
}
.proj-yr {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--gray-4);
}
.empty-state {
  text-align: center;
  color: var(--gray-5);
  padding: 4rem;
  font-family: var(--font-mono);
  font-size: 0.9rem;
}
.project-enter-active, .project-leave-active { transition: all 0.4s ease; }
.project-enter-from { opacity: 0; transform: scale(0.95); }
.project-leave-to { opacity: 0; transform: scale(0.95); }

@media (max-width: 900px) {
  .projects-grid { grid-template-columns: repeat(2, 1fr); }
  .project-card.wide { grid-column: span 2; }
}
@media (max-width: 600px) {
  .projects-grid { grid-template-columns: 1fr; }
  .project-card.wide { grid-column: span 1; }
}
</style>
