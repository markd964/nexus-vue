<template>
  <div class="services-page">
    <div class="page-hero">
      <div class="container">
        <p class="section-label">/ Services</p>
        <h1 class="page-title">What We<br /><span>Offer</span></h1>
        <p class="page-sub">End-to-end digital craftsmanship — from strategy to deployment and beyond.</p>
      </div>
    </div>

    <!-- Services Detail -->
    <section class="services-detail">
      <div class="container">
        <div class="service-blocks">
          <div
            class="service-block"
            v-for="(service, i) in services"
            :key="service.title"
            :class="{ open: openIndex === i }"
            @click="openIndex = openIndex === i ? null : i"
          >
            <div class="service-block-header">
              <div class="service-block-left">
                <span class="svc-num">{{ String(i + 1).padStart(2, '0') }}</span>
                <span class="svc-icon">{{ service.icon }}</span>
                <h3 class="svc-title">{{ service.title }}</h3>
              </div>
              <div class="service-block-right">
                <span class="svc-price">{{ service.price }}</span>
                <span class="svc-chevron">{{ openIndex === i ? '−' : '+' }}</span>
              </div>
            </div>
            <transition name="expand">
              <div class="service-block-body" v-if="openIndex === i">
                <p class="svc-desc">{{ service.desc }}</p>
                <div class="svc-deliverables">
                  <p class="deliverables-label">What's included:</p>
                  <ul>
                    <li v-for="item in service.deliverables" :key="item">
                      <span class="check">✓</span> {{ item }}
                    </li>
                  </ul>
                </div>
                <div class="svc-tags">
                  <span v-for="tag in service.tags" :key="tag">{{ tag }}</span>
                </div>
              </div>
            </transition>
          </div>
        </div>
      </div>
    </section>

    <!-- Process -->
    <section class="process-section">
      <div class="container">
        <p class="section-label">/ How We Work</p>
        <h2 class="section-title">Our Process</h2>
        <div class="process-steps">
          <div class="process-step" v-for="(step, i) in process" :key="step.title">
            <div class="step-connector" v-if="i < process.length - 1"></div>
            <div class="step-num">{{ String(i + 1).padStart(2, '0') }}</div>
            <div class="step-icon">{{ step.icon }}</div>
            <h4 class="step-title">{{ step.title }}</h4>
            <p class="step-desc">{{ step.desc }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Pricing Tiers -->
    <section class="pricing-section">
      <div class="container">
        <p class="section-label center">/ Pricing</p>
        <h2 class="section-title center">Simple, Transparent<br /><span>Pricing</span></h2>
        <div class="pricing-grid">
          <div
            class="pricing-card"
            v-for="plan in plans"
            :key="plan.name"
            :class="{ featured: plan.featured }"
          >
            <div class="plan-badge" v-if="plan.featured">Most Popular</div>
            <p class="plan-name">{{ plan.name }}</p>
            <p class="plan-price">{{ plan.price }}<span v-if="plan.suffix">{{ plan.suffix }}</span></p>
            <p class="plan-desc">{{ plan.desc }}</p>
            <ul class="plan-features">
              <li v-for="feat in plan.features" :key="feat">
                <span class="feat-check">✓</span> {{ feat }}
              </li>
            </ul>
            <router-link to="/contact" class="plan-cta" :class="{ featured: plan.featured }">
              Get Started →
            </router-link>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'ServicesView',
  data() {
    return {
      openIndex: 0,
      services: [
        {
          title: 'Web Design & Development',
          icon: '⬡',
          price: 'From $8,000',
          desc: 'Full-stack web development with a designer\'s eye. We build fast, accessible, and visually exceptional websites that convert visitors into customers.',
          deliverables: ['Custom UI/UX design in Figma', 'Responsive Vue.js or React build', 'CMS integration (Sanity, Contentful)', 'Performance optimization (90+ Lighthouse)', 'Vercel deployment + CI/CD pipeline', '30-day post-launch support'],
          tags: ['Vue.js', 'React', 'TypeScript', 'Tailwind', 'GSAP'],
        },
        {
          title: 'Brand Identity Systems',
          icon: '◈',
          price: 'From $4,500',
          desc: 'We create bold, cohesive brand identities that feel alive across every touchpoint — from logo to design system to motion guidelines.',
          deliverables: ['Logo design (3 concepts)', 'Full brand guidelines document', 'Typography & color system', 'Icon & illustration system', 'Social media templates', 'Brand motion principles'],
          tags: ['Figma', 'Illustrator', 'Brand Strategy', 'Motion'],
        },
        {
          title: 'E-Commerce Solutions',
          icon: '▦',
          price: 'From $12,000',
          desc: 'Revenue-optimized e-commerce experiences. We design and build custom storefronts that look incredible and convert at industry-leading rates.',
          deliverables: ['Custom Shopify or headless build', '3D product visualization', 'Checkout optimization', 'Inventory & analytics integration', 'Email automation setup', 'A/B testing framework'],
          tags: ['Shopify', 'Next.js', 'Stripe', 'Three.js', 'Analytics'],
        },
        {
          title: 'Performance Optimization',
          icon: '⬕',
          price: 'From $2,500',
          desc: 'We audit and fix slow websites — reducing load times, improving Core Web Vitals, and boosting SEO rankings through technical excellence.',
          deliverables: ['Full performance audit report', 'Core Web Vitals optimization', 'Image & asset optimization', 'Caching & CDN setup', 'SEO technical improvements', 'Monthly monitoring dashboard'],
          tags: ['Lighthouse', 'WebPageTest', 'CDN', 'SEO', 'Analytics'],
        },
        {
          title: 'Motion & Animation',
          icon: '◎',
          price: 'From $3,000',
          desc: 'From subtle UI micro-interactions to full WebGL experiences — we bring interfaces to life with motion that feels purposeful and memorable.',
          deliverables: ['Micro-interaction design', 'Page transition system', 'Scroll-triggered animations', 'WebGL / Three.js scenes', 'Lottie animation production', 'Motion guidelines document'],
          tags: ['GSAP', 'Three.js', 'WebGL', 'Lottie', 'CSS Animation'],
        },
      ],
      process: [
        { icon: '🔍', title: 'Discovery', desc: 'We dig deep into your goals, users, and competitive landscape to define the strategic foundation.' },
        { icon: '✏️', title: 'Design', desc: 'Wireframes evolve into high-fidelity prototypes. Every pixel is deliberate, every interaction mapped.' },
        { icon: '⚙️', title: 'Build', desc: 'Clean, performant code with modern frameworks. We build for scale from day one.' },
        { icon: '🚀', title: 'Launch', desc: 'Deployed with CI/CD pipelines. Your site goes live with zero downtime and full confidence.' },
        { icon: '📈', title: 'Grow', desc: 'Post-launch analytics, iteration cycles, and ongoing optimization to keep improving.' },
      ],
      plans: [
        {
          name: 'Starter',
          price: '$4,500',
          desc: 'Perfect for early-stage startups and personal brands.',
          features: ['Up to 5 pages', 'Custom design', 'Mobile responsive', 'CMS integration', 'Vercel deployment', '14-day support'],
          featured: false,
        },
        {
          name: 'Studio',
          price: '$12,000',
          desc: 'Our most popular package for growing businesses.',
          features: ['Up to 15 pages', 'Custom design system', 'Advanced animations', 'E-commerce ready', 'Analytics & SEO', '60-day support', 'Monthly retainer option'],
          featured: true,
        },
        {
          name: 'Enterprise',
          price: 'Custom',
          suffix: '',
          desc: 'Tailored solutions for complex, high-traffic platforms.',
          features: ['Unlimited pages', 'Full design system', 'WebGL & 3D', 'Custom integrations', 'Performance SLA', 'Dedicated team', 'Ongoing partnership'],
          featured: false,
        },
      ],
    }
  },
}
</script>

<style scoped>
.services-page { padding-top: 6rem; min-height: 100vh; }
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
.section-label.center { text-align: center; }
.page-title {
  font-family: var(--font-display);
  font-size: clamp(4rem, 10vw, 8rem);
  line-height: 0.95;
  margin-bottom: 1.5rem;
}
.page-title span { color: var(--acid); }
.page-sub { color: var(--gray-5); max-width: 500px; font-size: 1rem; line-height: 1.7; }
.section-title {
  font-family: var(--font-display);
  font-size: clamp(2.5rem, 5vw, 4rem);
  margin-bottom: 3rem;
  line-height: 1.05;
}
.section-title.center { text-align: center; }
.section-title span { color: var(--acid); }

/* Accordion */
.services-detail { padding: 4rem 0 6rem; }
.service-blocks { display: flex; flex-direction: column; }
.service-block {
  border-bottom: 1px solid var(--gray-3);
  overflow: hidden;
  cursor: pointer;
}
.service-block-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2rem 0;
  gap: 1rem;
}
.service-block-left {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}
.svc-num { font-family: var(--font-mono); font-size: 0.75rem; color: var(--gray-4); min-width: 28px; }
.svc-icon { font-size: 1.5rem; }
.svc-title {
  font-size: clamp(1.1rem, 2.5vw, 1.5rem);
  font-weight: 400;
  transition: color var(--transition);
}
.service-block:hover .svc-title,
.service-block.open .svc-title { color: var(--acid); }
.service-block-right { display: flex; align-items: center; gap: 2rem; }
.svc-price { font-family: var(--font-mono); font-size: 0.85rem; color: var(--gray-5); white-space: nowrap; }
.svc-chevron {
  width: 32px; height: 32px;
  border: 1px solid var(--gray-3);
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 1.1rem;
  color: var(--gray-5);
  transition: border-color var(--transition), color var(--transition);
  flex-shrink: 0;
}
.service-block.open .svc-chevron { border-color: var(--acid); color: var(--acid); }
.service-block-body { padding: 0 0 2.5rem 7rem; }
.svc-desc {
  color: var(--gray-6);
  font-size: 1rem;
  line-height: 1.7;
  margin-bottom: 1.5rem;
  max-width: 600px;
}
.svc-deliverables { margin-bottom: 1.5rem; }
.deliverables-label {
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--gray-4);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-bottom: 0.8rem;
}
.svc-deliverables ul { display: grid; grid-template-columns: repeat(2, 1fr); gap: 0.5rem; }
.svc-deliverables li {
  font-size: 0.9rem;
  color: var(--gray-6);
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.check { color: var(--acid); font-size: 0.8rem; }
.svc-tags { display: flex; gap: 0.5rem; flex-wrap: wrap; }
.svc-tags span {
  padding: 0.25rem 0.7rem;
  background: var(--gray-2);
  border: 1px solid var(--gray-3);
  border-radius: 100px;
  font-family: var(--font-mono);
  font-size: 0.72rem;
  color: var(--gray-5);
}
.expand-enter-active, .expand-leave-active { transition: opacity 0.3s ease, max-height 0.4s ease; max-height: 500px; overflow: hidden; }
.expand-enter-from, .expand-leave-to { opacity: 0; max-height: 0; }

/* Process */
.process-section {
  padding: 6rem 0;
  border-top: 1px solid var(--gray-3);
  background: var(--gray-1);
}
.process-steps {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 1rem;
  position: relative;
}
.process-step {
  position: relative;
  text-align: center;
  padding: 1rem;
}
.step-connector {
  position: absolute;
  top: 2rem;
  right: -50%;
  width: 100%;
  height: 1px;
  background: linear-gradient(90deg, var(--gray-3), var(--acid), var(--gray-3));
  z-index: 0;
}
.step-num {
  font-family: var(--font-mono);
  font-size: 0.65rem;
  color: var(--acid);
  margin-bottom: 0.75rem;
}
.step-icon { font-size: 2rem; margin-bottom: 1rem; }
.step-title {
  font-size: 1rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}
.step-desc { font-size: 0.82rem; color: var(--gray-5); line-height: 1.6; }

/* Pricing */
.pricing-section { padding: 6rem 0 8rem; }
.pricing-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  align-items: start;
}
.pricing-card {
  border-radius: var(--radius-lg);
  padding: 2.5rem;
  border: 1px solid var(--gray-3);
  background: var(--gray-1);
  position: relative;
  transition: transform var(--transition), border-color var(--transition);
}
.pricing-card:hover { transform: translateY(-4px); }
.pricing-card.featured {
  border-color: var(--acid);
  background: var(--gray-2);
  transform: scale(1.02);
}
.pricing-card.featured:hover { transform: scale(1.02) translateY(-4px); }
.plan-badge {
  position: absolute;
  top: -12px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--acid);
  color: var(--black);
  font-size: 0.7rem;
  font-weight: 600;
  padding: 0.2rem 1rem;
  border-radius: 100px;
  letter-spacing: 0.06em;
  white-space: nowrap;
}
.plan-name {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--gray-5);
  text-transform: uppercase;
  letter-spacing: 0.12em;
  margin-bottom: 0.75rem;
}
.plan-price {
  font-family: var(--font-display);
  font-size: 3rem;
  color: var(--white);
  margin-bottom: 0.5rem;
  line-height: 1;
}
.plan-price span { font-family: var(--font-body); font-size: 1rem; color: var(--gray-5); }
.plan-desc { font-size: 0.85rem; color: var(--gray-5); margin-bottom: 2rem; line-height: 1.6; }
.plan-features { display: flex; flex-direction: column; gap: 0.6rem; margin-bottom: 2.5rem; }
.plan-features li { font-size: 0.88rem; color: var(--gray-6); display: flex; align-items: center; gap: 0.6rem; }
.feat-check { color: var(--acid); font-size: 0.8rem; }
.plan-cta {
  display: block;
  text-align: center;
  padding: 0.8rem;
  border: 1px solid var(--gray-3);
  border-radius: var(--radius);
  font-size: 0.85rem;
  color: var(--gray-6);
  transition: all var(--transition);
}
.plan-cta:hover { border-color: var(--acid); color: var(--acid); }
.plan-cta.featured {
  background: var(--acid);
  border-color: var(--acid);
  color: var(--black);
  font-weight: 500;
}
.plan-cta.featured:hover { background: var(--acid-dim); }

@media (max-width: 900px) {
  .process-steps { grid-template-columns: repeat(2, 1fr); gap: 2rem; }
  .step-connector { display: none; }
  .pricing-grid { grid-template-columns: 1fr; }
  .pricing-card.featured { transform: none; }
  .svc-deliverables ul { grid-template-columns: 1fr; }
  .service-block-body { padding-left: 0; }
}
@media (max-width: 600px) {
  .process-steps { grid-template-columns: 1fr; }
}
</style>
