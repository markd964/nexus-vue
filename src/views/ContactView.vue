<template>
  <div class="contact-page">
    <div class="page-hero">
      <div class="container">
        <p class="section-label">/ Contact</p>
        <h1 class="page-title">Let's Work<br /><span>Together</span></h1>
      </div>
    </div>

    <section class="contact-section">
      <div class="container contact-grid">
        <!-- Info Column -->
        <div class="contact-info">
          <p class="info-intro">
            Ready to start a project or just want to chat? Fill out the form and we'll get back to you within 24 hours.
          </p>

          <div class="contact-details">
            <div class="detail-item" v-for="d in contactDetails" :key="d.label">
              <span class="detail-icon">{{ d.icon }}</span>
              <div>
                <p class="detail-label">{{ d.label }}</p>
                <p class="detail-value">{{ d.value }}</p>
              </div>
            </div>
          </div>

          <div class="availability-box">
            <span class="avail-dot"></span>
            <div>
              <p class="avail-title">Currently Available</p>
              <p class="avail-sub">Taking on new projects for Q2 2025 onwards.</p>
            </div>
          </div>

          <div class="social-links">
            <a v-for="social in socials" :key="social.name" :href="social.url" class="social-link" target="_blank" rel="noopener">
              <span class="social-icon">{{ social.icon }}</span>
              <span>{{ social.name }}</span>
              <span class="social-arrow">↗</span>
            </a>
          </div>
        </div>

        <!-- Form Column -->
        <div class="form-column">
          <form class="contact-form" @submit.prevent="submitForm" novalidate>
            <!-- Project Type -->
            <div class="form-group">
              <label class="form-label">Project Type</label>
              <div class="type-options">
                <button
                  type="button"
                  v-for="type in projectTypes"
                  :key="type"
                  class="type-btn"
                  :class="{ active: form.type === type }"
                  @click="form.type = type"
                >{{ type }}</button>
              </div>
            </div>

            <!-- Name & Email -->
            <div class="form-row">
              <div class="form-group">
                <label for="name" class="form-label">Your Name *</label>
                <input
                  id="name"
                  type="text"
                  class="form-input"
                  :class="{ error: errors.name }"
                  v-model="form.name"
                  placeholder="John Doe"
                  @blur="validateField('name')"
                />
                <span class="error-msg" v-if="errors.name">{{ errors.name }}</span>
              </div>
              <div class="form-group">
                <label for="email" class="form-label">Email Address *</label>
                <input
                  id="email"
                  type="email"
                  class="form-input"
                  :class="{ error: errors.email }"
                  v-model="form.email"
                  placeholder="you@company.com"
                  @blur="validateField('email')"
                />
                <span class="error-msg" v-if="errors.email">{{ errors.email }}</span>
              </div>
            </div>

            <!-- Company & Budget -->
            <div class="form-row">
              <div class="form-group">
                <label for="company" class="form-label">Company</label>
                <input id="company" type="text" class="form-input" v-model="form.company" placeholder="Acme Inc." />
              </div>
              <div class="form-group">
                <label for="budget" class="form-label">Budget Range</label>
                <select id="budget" class="form-input form-select" v-model="form.budget">
                  <option value="">Select budget</option>
                  <option v-for="b in budgets" :key="b" :value="b">{{ b }}</option>
                </select>
              </div>
            </div>

            <!-- Message -->
            <div class="form-group">
              <label for="message" class="form-label">Tell us about your project *</label>
              <textarea
                id="message"
                class="form-input form-textarea"
                :class="{ error: errors.message }"
                v-model="form.message"
                placeholder="Describe your vision, goals, timeline, and any specific requirements..."
                rows="5"
                @blur="validateField('message')"
              ></textarea>
              <div class="char-count" :class="{ near: form.message.length > 400 }">
                {{ form.message.length }}/500
              </div>
              <span class="error-msg" v-if="errors.message">{{ errors.message }}</span>
            </div>

            <!-- Submit -->
            <div class="form-footer">
              <p class="form-note">We respond to all inquiries within 24 hours.</p>
              <button type="submit" class="btn-submit" :class="{ loading: isLoading, success: isSuccess }">
                <span v-if="!isLoading && !isSuccess">Send Message →</span>
                <span v-else-if="isLoading" class="spinner">Sending...</span>
                <span v-else>✓ Message Sent!</span>
              </button>
            </div>
          </form>
        </div>
      </div>
    </section>

    <!-- Map / Location Bar -->
    <section class="location-bar">
      <div class="container location-inner">
        <div class="location-item" v-for="loc in locations" :key="loc.city">
          <p class="loc-city">{{ loc.city }}</p>
          <p class="loc-time">{{ getTime(loc.tz) }}</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'ContactView',
  data() {
    return {
      isLoading: false,
      isSuccess: false,
      form: {
        type: 'Web Design',
        name: '',
        email: '',
        company: '',
        budget: '',
        message: '',
      },
      errors: {},
      projectTypes: ['Web Design', 'E-Commerce', 'Branding', 'Motion', 'Other'],
      budgets: ['Under $5k', '$5k – $15k', '$15k – $30k', '$30k – $60k', '$60k+'],
      contactDetails: [
        { icon: '✉', label: 'Email', value: 'hello@nexusstudio.io' },
        { icon: '◷', label: 'Response Time', value: 'Within 24 hours' },
        { icon: '◈', label: 'Location', value: 'Remote-first, Global' },
      ],
      socials: [
        { name: 'Dribbble', icon: '◉', url: '#' },
        { name: 'GitHub', icon: '◎', url: '#' },
        { name: 'LinkedIn', icon: '◆', url: '#' },
        { name: 'Twitter/X', icon: '◇', url: '#' },
      ],
      locations: [
        { city: 'New York', tz: 'America/New_York' },
        { city: 'London', tz: 'Europe/London' },
        { city: 'Berlin', tz: 'Europe/Berlin' },
        { city: 'Tokyo', tz: 'Asia/Tokyo' },
      ],
      clockInterval: null,
      tick: 0,
    }
  },
  mounted() {
    this.clockInterval = setInterval(() => { this.tick++ }, 1000)
  },
  beforeUnmount() {
    clearInterval(this.clockInterval)
  },
  methods: {
    getTime(tz) {
      this.tick // reactive dependency
      return new Date().toLocaleTimeString('en-US', { timeZone: tz, hour: '2-digit', minute: '2-digit', hour12: false })
    },
    validateField(field) {
      const errs = { ...this.errors }
      if (field === 'name') {
        errs.name = this.form.name.trim().length < 2 ? 'Please enter your name.' : ''
      }
      if (field === 'email') {
        const emailRe = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
        errs.email = !emailRe.test(this.form.email) ? 'Please enter a valid email.' : ''
      }
      if (field === 'message') {
        errs.message = this.form.message.trim().length < 20 ? 'Message must be at least 20 characters.' : ''
        if (this.form.message.length > 500) errs.message = 'Message is too long (max 500 characters).'
      }
      this.errors = errs
    },
    validateAll() {
      this.validateField('name')
      this.validateField('email')
      this.validateField('message')
      return !Object.values(this.errors).some(e => e)
    },
    async submitForm() {
      if (!this.validateAll()) return
      this.isLoading = true
      // Simulate API call
      await new Promise(r => setTimeout(r, 2000))
      this.isLoading = false
      this.isSuccess = true
      setTimeout(() => {
        this.isSuccess = false
        this.form = { type: 'Web Design', name: '', email: '', company: '', budget: '', message: '' }
        this.errors = {}
      }, 4000)
    },
  },
}
</script>

<style scoped>
.contact-page { padding-top: 6rem; min-height: 100vh; }
.page-hero { padding: 5rem 0 3rem; border-bottom: 1px solid var(--gray-3); }
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
}
.page-title span { color: var(--acid); }

/* Contact Layout */
.contact-section { padding: 5rem 0; }
.contact-grid {
  display: grid;
  grid-template-columns: 380px 1fr;
  gap: 6rem;
  align-items: start;
}
.info-intro { color: var(--gray-6); font-size: 0.95rem; line-height: 1.7; margin-bottom: 2.5rem; }

.contact-details { display: flex; flex-direction: column; gap: 1.5rem; margin-bottom: 2.5rem; }
.detail-item { display: flex; align-items: flex-start; gap: 1rem; }
.detail-icon { font-size: 1.1rem; color: var(--acid); margin-top: 2px; }
.detail-label { font-family: var(--font-mono); font-size: 0.68rem; color: var(--gray-5); text-transform: uppercase; letter-spacing: 0.1em; margin-bottom: 0.2rem; }
.detail-value { font-size: 0.9rem; color: var(--white); }

.availability-box {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1.2rem;
  border: 1px solid rgba(200,245,59,0.3);
  border-radius: var(--radius);
  background: rgba(200,245,59,0.05);
  margin-bottom: 2.5rem;
}
.avail-dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  background: var(--acid);
  margin-top: 6px;
  flex-shrink: 0;
  animation: pulse 2s infinite;
}
@keyframes pulse { 0%,100% { opacity:1; } 50% { opacity:0.3; } }
.avail-title { font-size: 0.9rem; font-weight: 500; color: var(--acid); margin-bottom: 0.2rem; }
.avail-sub { font-size: 0.82rem; color: var(--gray-5); }

.social-links { display: flex; flex-direction: column; gap: 0.5rem; }
.social-link {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.7rem 0;
  border-bottom: 1px solid var(--gray-3);
  color: var(--gray-6);
  font-size: 0.88rem;
  transition: color var(--transition);
}
.social-link:hover { color: var(--acid); }
.social-icon { font-size: 1rem; }
.social-arrow { margin-left: auto; font-size: 0.8rem; }

/* Form */
.contact-form { display: flex; flex-direction: column; gap: 1.8rem; }
.form-label {
  display: block;
  font-family: var(--font-mono);
  font-size: 0.7rem;
  color: var(--gray-5);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-bottom: 0.7rem;
}
.type-options { display: flex; gap: 0.5rem; flex-wrap: wrap; }
.type-btn {
  padding: 0.4rem 1rem;
  border-radius: 100px;
  border: 1px solid var(--gray-3);
  font-size: 0.82rem;
  color: var(--gray-5);
  background: transparent;
  font-family: var(--font-body);
  transition: all var(--transition);
}
.type-btn:hover { border-color: var(--gray-4); color: var(--white); }
.type-btn.active { background: var(--acid); border-color: var(--acid); color: var(--black); font-weight: 500; }

.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1.5rem; }
.form-group { position: relative; }
.form-input {
  width: 100%;
  background: var(--gray-1);
  border: 1px solid var(--gray-3);
  border-radius: var(--radius);
  padding: 0.85rem 1rem;
  color: var(--white);
  font-family: var(--font-body);
  font-size: 0.9rem;
  transition: border-color var(--transition);
  outline: none;
  appearance: none;
}
.form-input::placeholder { color: var(--gray-4); }
.form-input:focus { border-color: var(--acid); }
.form-input.error { border-color: var(--red); }
.form-select { cursor: pointer; }
.form-textarea { resize: vertical; min-height: 120px; }
.char-count {
  text-align: right;
  font-family: var(--font-mono);
  font-size: 0.65rem;
  color: var(--gray-4);
  margin-top: 0.3rem;
}
.char-count.near { color: var(--red); }
.error-msg { display: block; font-size: 0.75rem; color: var(--red); margin-top: 0.4rem; font-family: var(--font-mono); }

.form-footer { display: flex; align-items: center; justify-content: space-between; gap: 1rem; flex-wrap: wrap; }
.form-note { font-family: var(--font-mono); font-size: 0.72rem; color: var(--gray-5); }
.btn-submit {
  padding: 0.9rem 2rem;
  background: var(--acid);
  color: var(--black);
  font-family: var(--font-body);
  font-size: 0.9rem;
  font-weight: 500;
  border-radius: var(--radius);
  transition: background var(--transition), transform var(--transition);
  min-width: 180px;
}
.btn-submit:hover:not(:disabled) { background: var(--acid-dim); transform: translateY(-1px); }
.btn-submit.loading { background: var(--gray-3); color: var(--gray-5); cursor: not-allowed; }
.btn-submit.success { background: #00b894; color: var(--white); }

/* Location Bar */
.location-bar {
  border-top: 1px solid var(--gray-3);
  padding: 2rem 0;
  background: var(--gray-1);
}
.location-inner {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
}
.location-item { text-align: center; }
.loc-city { font-family: var(--font-mono); font-size: 0.7rem; color: var(--gray-5); text-transform: uppercase; letter-spacing: 0.1em; margin-bottom: 0.3rem; }
.loc-time { font-family: var(--font-display); font-size: 1.8rem; color: var(--white); letter-spacing: 0.05em; }

@media (max-width: 900px) {
  .contact-grid { grid-template-columns: 1fr; gap: 3rem; }
  .location-inner { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 600px) {
  .form-row { grid-template-columns: 1fr; }
  .form-footer { flex-direction: column; align-items: stretch; }
  .btn-submit { width: 100%; }
}
</style>
