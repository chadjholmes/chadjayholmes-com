<script lang="ts">
  export let waveTransitionProgress = 0;
  export let caseStudies: {
    title: string;
    description: string;
    tech: string[];
    color: string;
  }[];
</script>

<section class="case-studies-section" style="--section-opacity: {waveTransitionProgress}">
  <div class="section-heading">
    <h2>Featured Work</h2>
    <p>Selected projects that showcase my expertise</p>
  </div>
  
  <div class="case-studies-grid">
    {#each caseStudies as study, i}
      <div 
        class="case-study-card" 
        style="--card-index: {i}; --card-color: {study.color};"
      >
        <div class="card-content">
          <h3>{study.title}</h3>
          <p>{study.description}</p>
          <div class="tech-tags">
            {#each study.tech as tag}
              <span class="tech-tag">{tag}</span>
            {/each}
          </div>
          <a href="/case-study/{i}" class="view-project">View Details</a>
        </div>
      </div>
    {/each}
  </div>
  
  <div class="cta-container">
    <a href="/experience" class="cta-button primary">View Full Resume</a>
    <a href="/contact" class="cta-button secondary">Get In Touch</a>
  </div>
</section>

<style>
  /* Case Studies Section */
  .case-studies-section {
    min-height: 100vh;
    padding: 5rem 2rem 5rem;
    position: relative;
    z-index: 5;
    background-color: var(--bg-color);
    overflow: visible;
    opacity: var(--section-opacity, 0);
    transition: opacity 0.5s ease;
    background-color: var(--bg-color);
  }

  .section-heading {
    text-align: center;
    margin-bottom: 4rem;
  }

  .section-heading h2 {
    font-size: clamp(2rem, 5vw, 3.5rem);
    font-weight: 700;
    color: var(--text-primary);
    margin-bottom: 1rem;
    text-align: center;
  }

  .section-heading p {
    font-size: clamp(1rem, 2vw, 1.25rem);
    color: var(--text-secondary);
    max-width: 700px;
    margin: 0 auto;
  }

  .case-studies-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    max-width: 1200px;
    margin: 0 auto 4rem;
  }

  .case-study-card {
    background: rgba(17, 34, 64, 0.6);
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 10px 30px -15px rgba(2, 12, 27, 0.7);
    position: relative;
    transition: all 0.3s ease-out;
    height: 100%;
    display: flex;
    flex-direction: column;
    border-top: 3px solid var(--card-color, var(--gradient-blue));
    opacity: 0;
    transform: translateY(20px);
    animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    animation-delay: calc(0.2s * var(--card-index) + 0.5s);
  }

  .case-study-card:hover {
    transform: translateY(-10px);
    box-shadow: 0 20px 30px -15px rgba(2, 12, 27, 0.7);
  }

  .card-content {
    padding: 2rem;
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .case-study-card h3 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
    color: var(--text-primary);
  }

  .case-study-card p {
    color: var(--text-secondary);
    margin-bottom: 1.5rem;
    flex-grow: 1;
    line-height: 1.6;
  }

  .tech-tags {
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 1.5rem;
    gap: 0.5rem;
  }

  .tech-tag {
    font-size: 0.75rem;
    padding: 0.25rem 0.75rem;
    border-radius: 50px;
    background: rgba(255, 255, 255, 0.1);
    color: var(--text-accent);
    display: inline-block;
  }

  .view-project {
    display: inline-block;
    padding: 0.75rem 1.5rem;
    background: transparent;
    color: var(--text-accent);
    border: 1px solid var(--text-accent);
    border-radius: 4px;
    font-size: 0.875rem;
    font-weight: 600;
    text-decoration: none;
    text-align: center;
    transition: all 0.2s ease;
    align-self: flex-start;
  }

  .view-project:hover {
    background: rgba(125, 211, 252, 0.1);
    transform: translateY(-2px);
  }

  .cta-container {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 1.5rem;
    margin-top: 2rem;
    opacity: 0;
    transform: translateY(20px);
    animation: slideUp 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    animation-delay: 1.5s;
  }

  .cta-button {
    display: inline-block;
    padding: 1rem 2rem;
    border-radius: 4px;
    font-size: 1rem;
    font-weight: 600;
    text-decoration: none;
    text-align: center;
    transition: all 0.2s ease;
  }

  .cta-button.primary {
    background: var(--text-accent);
    color: rgba(2, 12, 27, 1);
  }

  .cta-button.secondary {
    background: transparent;
    color: var(--text-accent);
    border: 1px solid var(--text-accent);
  }

  .cta-button:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 20px -10px rgba(125, 211, 252, 0.5);
  }

  @keyframes slideUp {
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @media (max-width: 480px) {
    .case-studies-section {
      padding: 8rem 1rem 3rem;
    }

    .case-studies-grid {
      grid-template-columns: 1fr;
    }

    .cta-container {
      flex-direction: column;
      width: 100%;
    }

    .cta-button {
      width: 100%;
    }
  }

  /* Add tablet breakpoint for mid-sized devices */
  @media (min-width: 481px) and (max-width: 768px) {
    .case-studies-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }
</style> 