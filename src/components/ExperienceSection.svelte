<script lang="ts">
  import { onMount } from 'svelte';
  
  export let experiences = [];
  
  interface Role {
    title: string;
    period: string;
    highlights: string[];
    technologies?: string[];
  }

  interface Experience {
    company: string;
    roles: Role[];
  }
  
  if (experiences.length === 0) {
    experiences = [
      {
        company: "Example Company",
        roles: [
          {
            title: "Senior Developer",
            period: "2022 - Present",
            highlights: [
              "Implemented key features that increased user engagement by 40%",
              "Led team of 5 developers on mission-critical projects"
            ]
          },
          {
            title: "Developer",
            period: "2020 - 2022",
            highlights: [
              "Rebuilt legacy codebase to modern standards",
              "Developed CI/CD pipeline that reduced deployment time by 60%"
            ]
          }
        ]
      }
    ];
  }
  
  let activeCompany = experiences[0].company;
  let activeExperience = experiences[0];
  
  function setActiveCompany(company: string) {
    activeCompany = company;
    activeExperience = experiences.find(exp => exp.company === company) || experiences[0];
  }
  
  onMount(() => {
    activeExperience = experiences.find(exp => exp.company === activeCompany) || experiences[0];
  });
</script>

<section id="experience" class="section experience-section">
  <div class="section-container">
    <h2 class="section-title">Experience</h2>
    <div class="experience-content">
      <div class="companies-list">
        {#each experiences as experience}
          <button 
            class="company-button {activeCompany === experience.company ? 'active' : ''}"
            on:click={() => setActiveCompany(experience.company)}
          >
            <h3 class="company-name">{experience.company}</h3>
          </button>
        {/each}
      </div>
      
      <div class="roles-container">
        {#if activeExperience}
          {#each activeExperience.roles as role}
            <div class="role-card">
              <div class="role-header">
                <h3 class="role-title">{role.title}</h3>
                <span class="role-period">{role.period}</span>
              </div>
              <div class="role-content">
                <ul class="highlights-list">
                  {#each role.highlights as highlight}
                    <li class="role-highlight">{highlight}</li>
                  {/each}
                </ul>
                {#if role.technologies && role.technologies.length > 0}
                  <div class="tech-tags">
                    {#each role.technologies as tech}
                      <span class="tech-tag">{tech}</span>
                    {/each}
                  </div>
                {/if}
              </div>
            </div>
          {/each}
        {/if}
      </div>
    </div>
  </div>
</section>

<style>
  .experience-section {
    min-height: 100vh;
    background-color: var(--bg-color);
    color: var(--text-primary);
    padding: 5rem 0;
  }
  
  .section-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 clamp(1rem, 3vw, 2rem);
    width: 100%;
  }
  
  .section-title {
    font-size: clamp(1.8rem, 6.5vw, 5.5rem);
    color: var(--text-primary);
    margin: 0 0 3rem;
    text-align: left;
    font-weight: 700;
    font-family: 'Inter', sans-serif;
    line-height: 1;
    letter-spacing: -0.02em;
    text-shadow: 
      0 0 1px rgba(255, 255, 255, 0.2),
      0 0 15px var(--text-glow),
      0 0 25px var(--text-glow);
    word-break: break-word;
    hyphens: auto;
  }
  
  .experience-content {
    display: grid;
    grid-template-columns: 1fr 3fr;
    gap: 2rem;
    align-items: flex-start;
  }
  
  .companies-list {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    position: sticky;
    top: 7rem;
  }
  
  .company-button {
    background: transparent;
    border: none;
    padding: 1rem;
    text-align: left;
    cursor: pointer;
    border-left: 3px solid rgba(125, 211, 252, 0.2);
    transition: all 0.3s ease;
    outline: none;
    color: var(--text-secondary);
  }
  
  .company-button:hover {
    border-left-color: rgba(125, 211, 252, 0.5);
    color: var(--text-accent);
  }
  
  .company-button.active {
    border-left-color: var(--text-accent);
    color: var(--text-primary);
    background: rgba(125, 211, 252, 0.05);
  }
  
  .company-name {
    margin: 0;
    font-size: 1.25rem;
    font-weight: 600;
    letter-spacing: -0.01em;
  }
  
  .roles-container {
    display: flex;
    flex-direction: column;
    gap: 2rem;
  }
  
  .role-card {
    padding: 1.5rem;
    background: rgba(125, 211, 252, 0.05);
    border: 1px solid rgba(125, 211, 252, 0.1);
    border-radius: 0.5rem;
    transition: all 0.3s ease;
  }
  
  .role-card:hover {
    border-color: rgba(125, 211, 252, 0.2);
    box-shadow: 0 8px 20px -8px rgba(0, 0, 0, 0.3);
    transform: translateY(-5px);
  }
  
  .role-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  
  .role-title {
    margin: 0;
    font-size: 1.5rem;
    color: var(--text-accent);
    letter-spacing: -0.01em;
  }
  
  .role-period {
    font-size: 0.9rem;
    color: var(--text-secondary);
    background: rgba(125, 211, 252, 0.1);
    padding: 0.25rem 0.75rem;
    border-radius: 0.25rem;
    white-space: nowrap;
  }
  
  .highlights-list {
    list-style: none;
    padding: 0;
    margin: 0 0 1.5rem;
  }
  
  .role-highlight {
    position: relative;
    padding-left: 1.5rem;
    margin-bottom: 0.75rem;
    color: var(--text-secondary);
    line-height: 1.5;
  }
  
  .role-highlight::before {
    content: '▹';
    position: absolute;
    left: 0;
    color: var(--text-accent);
  }
  
  .tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  
  .tech-tag {
    font-size: 0.8rem;
    padding: 0.25rem 0.5rem;
    background: rgba(125, 211, 252, 0.1);
    color: var(--text-secondary);
    border-radius: 0.25rem;
    white-space: nowrap;
  }
  
  @media (max-width: 768px) {
    .experience-content {
      grid-template-columns: 1fr;
      gap: 1rem;
    }
    
    .companies-list {
      flex-direction: row;
      overflow-x: auto;
      padding-bottom: 0.5rem;
      position: relative;
      top: 0;
      scrollbar-width: thin;
    }
    
    .company-button {
      border-left: none;
      border-bottom: 3px solid rgba(125, 211, 252, 0.2);
      white-space: nowrap;
      padding: 0.5rem 1rem;
    }
    
    .company-button:hover,
    .company-button.active {
      border-left-color: transparent;
      border-bottom-color: var(--text-accent);
    }
    
    .role-header {
      flex-direction: column;
      align-items: flex-start;
    }
    
    .role-period {
      align-self: flex-start;
    }
    
    .role-card {
      padding: 1.25rem;
    }
  }
  
  @media (max-width: 480px) {
    .section-title {
      margin-bottom: 2rem;
    }
    
    .company-name {
      font-size: 1rem;
    }
    
    .role-title {
      font-size: 1.3rem;
    }
    
    .role-highlight {
      font-size: 0.9rem;
    }
    
    .tech-tag {
      font-size: 0.7rem;
    }
  }
</style> 