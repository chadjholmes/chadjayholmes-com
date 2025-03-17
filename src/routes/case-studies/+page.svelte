<script lang="ts">
  import { fade } from 'svelte/transition';
</script>

<svelte:head>
  <title>Case Studies | Chad Holmes</title>
  <meta name="description" content="Technical case studies by Chad Holmes" />
</svelte:head>

<div class="case-studies-container">
  <header class="case-studies-header" transition:fade>
    <h1>Case Studies</h1>
    <p class="subtitle">In-depth looks at technical problems I've solved</p>
  </header>

  <div class="case-studies-grid">
    <a href="/case-studies/service-request-platform" class="case-study-card">
      <h2>Service Request Middleware Platform</h2>
      <p class="case-study-description">Building a scalable system for enterprise service orchestration at Pattern Inc.</p>
      <div class="tags">
        <span class="tag">Architecture</span>
        <span class="tag">API Design</span>
        <span class="tag">Cloud Infrastructure</span>
      </div>
      <span class="read-more">Read Case Study →</span>
    </a>
    
    <!-- Add more case study cards as you develop them -->
  </div>
</div>

<style>
  .case-studies-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 3rem clamp(1rem, 3vw, 2rem);
    color: var(--text-color);
    min-height: 100vh;
  }

  .case-studies-header {
    margin-bottom: 3rem;
  }

  .back-link {
    display: inline-block;
    margin-bottom: 1.5rem;
    color: var(--text-accent);
    text-decoration: none;
    font-size: 1rem;
    transition: all 0.2s ease;
  }

  .back-link:hover {
    transform: translateX(-3px);
    color: var(--text-primary);
  }

  h1 {
    font-size: clamp(2.5rem, 6vw, 4rem);
    color: var(--text-primary);
    margin: 0;
    line-height: 1.1;
  }

  .subtitle {
    font-size: 1.25rem;
    color: var(--text-secondary);
    margin: 0.5rem 0 0;
  }

  .case-studies-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 2rem;
    margin-top: 2rem;
  }

  .case-study-card {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 8px;
    padding: 2rem;
    text-decoration: none;
    color: var(--text-color);
    border: 1px solid rgba(125, 211, 252, 0.2);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    display: flex;
    flex-direction: column;
  }

  .case-study-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  }

  .case-study-card h2 {
    font-size: 1.5rem;
    color: var(--text-accent);
    margin: 0 0 1rem;
  }

  .case-study-description {
    color: var(--text-secondary);
    line-height: 1.6;
    margin-bottom: 1.5rem;
  }

  .tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-top: auto;
    margin-bottom: 1.5rem;
  }

  .tag {
    background: rgba(125, 211, 252, 0.1);
    color: var(--text-secondary);
    padding: 0.25rem 0.75rem;
    border-radius: 4px;
    font-size: 0.9rem;
    border: 1px solid rgba(125, 211, 252, 0.2);
  }

  .read-more {
    color: var(--text-accent);
    font-size: 0.9rem;
    margin-top: auto;
    transition: transform 0.2s ease;
  }

  .case-study-card:hover .read-more {
    transform: translateX(5px);
  }

  @media (max-width: 768px) {
    .case-studies-grid {
      grid-template-columns: 1fr;
    }
  }
</style> 