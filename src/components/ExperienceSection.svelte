<script lang="ts">
  import { fade } from 'svelte/transition';

  // Define types for the experience data
  interface Role {
    title: string;
    period: string;
    highlights: string[];
    technologies?: string[];
  }

  interface Experience {
    company: string;
    roles: Role[];
    manager?: {
      name: string;
      email?: string;
      phone?: string;
    };
  }

  // Default experiences data
  export let experiences: Experience[] = [
    {
      company: "Pattern Inc.",
      roles: [
        {
          title: "Senior Software Engineer",
          period: "11/2024 - Present",
          highlights: [],
          technologies: ["Ruby", "TypeScript", "AWS", "Terraform"]
        },
        {
          title: "Software Engineer II",
          period: "04/2023 - 11/2024",
          highlights: [
            "Led a team of 4 engineers in designing and delivering a green-field product",
            "Developed highly available backend services in Ruby and TypeScript",
            "Architected high-throughput event-based data processing pipeline",
            "Designed and delivered prototype API connector for SaaS project"
          ],
          technologies: ["Ruby", "TypeScript", "AWS Lambda", "Event Processing"]
        },
        {
          title: "Software Engineer",
          period: "04/2022 - 04/2023",
          highlights: [
            "Led work-stream for central async job processing system",
            "Developed custom Lambda caching client package for private NPM registry",
            "Reduced MTM auth speeds from >1000ms to < 10ms through custom authorizer",
            "Deployed production AWS infrastructure using Terraform and Github Actions"
          ],
          technologies: ["AWS", "Terraform", "Node.js", "CI/CD"]
        }
      ]
    },
    {
      company: "Ultimate Kronos Group",
      roles: [
        {
          title: "Product Support Specialist II",
          period: "09/2021 - 04/2022",
          highlights: [
            "Resolved complex customer product issues via multiple channels",
            "Documented issues using Jira and Salesforce for development teams",
            "Developed SQL scripts for efficient troubleshooting processes",
            "Provided HRMS software consultation to industry professionals"
          ],
          technologies: ["SQL", "Jira", "Salesforce", "HRMS"]
        }
      ]
    },
    {
      company: "Instructure Inc.",
      roles: [
        {
          title: "Technical Support Engineer",
          period: "07/2020 - 09/2021",
          highlights: [
            "Managed 100+ weekly support cases with 95% CSAT score",
            "Achieved 90% first-contact resolution rate",
            "Ranked in top 12% of department performers (Q2 2021)",
            "Utilized API documentation for advanced troubleshooting"
          ],
          technologies: ["API Integration", "Technical Support", "Problem Resolution"]
        }
      ]
    }
  ];

  // Track active company for UI state
  export let activeCompany = experiences.length > 0 ? experiences[0].company : '';

  export let showAllExperiences = false;
  export let sectionTitle = "Experience";

  function setActiveCompany(company: string) {
    activeCompany = company;
  }
</script>

<section class="experience-section">
  <div class="section-container">
    <h1 class="section-title">{sectionTitle}</h1>
    
    <div class="experience-content">
      <!-- Company selector tabs -->
      <div class="company-tabs">
        {#each experiences as exp}
          <button 
            class="tab-button" 
            class:active={activeCompany === exp.company}
            on:click={() => setActiveCompany(exp.company)}
          >
            {exp.company}
          </button>
        {/each}
      </div>
      
      <!-- Experience details -->
      <div class="experience-details">
        {#each experiences as exp}
          {#if activeCompany === exp.company}
            <div class="company-section" transition:fade={{ duration: 200 }}>
              <div class="company-info">
                <h3 class="company-name">{exp.company}</h3>
              </div>
              
              {#each exp.roles as role}
                <div class="role">
                  <div class="role-header">
                    <h4 class="role-title">{role.title}</h4>
                    <span class="role-period">{role.period}</span>
                  </div>
                  
                  <ul class="highlights-list">
                    {#each role.highlights as highlight}
                      <li>{highlight}</li>
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
              {/each}
            </div>
          {/if}
        {/each}
      </div>
    </div>
  </div>
</section>

<style>
  .experience-section {
    padding: 0 0 5rem;
    width: 100%;
    position: relative;
    z-index: 10;
    font-size: 0.8em;
  }
  
  .section-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 clamp(1rem, 3vw, 2rem);
    width: 100%;
    position: relative;
    margin-top: 0;
  }
  
  .section-title {
    font-size: clamp(1.8rem, 6.5vw, 5.5rem);
    font-weight: 700;
    font-family: 'Inter', sans-serif;
    line-height: 1;
    letter-spacing: -0.02em;
    color: var(--text-primary);
    position: relative;
    text-align: left;
    margin: 0 0 2rem;
    text-shadow: 
      0 0 1px rgba(255, 255, 255, 0.2),
      0 0 15px var(--text-glow),
      0 0 25px var(--text-glow);
    word-break: break-word;
    hyphens: auto;
  }
  
  .experience-content {
    display: flex;
    gap: 2rem;
    padding-top: 5rem;
  }
  
  .company-tabs {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
    min-width: 180px;
    position: relative;
  }
  
  .tab-button {
    background: none;
    border: none;
    text-align: left;
    padding: 0.75rem 1rem;
    color: var(--text-secondary);
    cursor: pointer;
    border-left: 2px solid rgba(125, 211, 252, 0.1);
    transition: all 0.2s ease;
    font-family: inherit;
    font-size: 0.75rem;
  }
  
  .tab-button:hover {
    background: rgba(125, 211, 252, 0.05);
    color: var(--text-accent);
  }
  
  .tab-button.active {
    color: var(--text-accent);
    border-left: 2px solid var(--text-accent);
    background: rgba(125, 211, 252, 0.05);
  }
  
  .experience-details {
    position: relative;
    flex: 1;
    min-height: 750px;
  }
  
  .company-section {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
  }
  
  .company-name {
    font-size: 1.15rem;
    color: var(--text-primary);
    margin: 0 0 1.5rem;
  }
  
  .role {
    margin-bottom: 2rem;
    padding: 1.5rem;
    background: rgba(0, 0, 0, 0.2);
    border-radius: 8px;
    border: 1px solid rgba(125, 211, 252, 0.2);
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }
  
  .role:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }
  
  .role-header {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 1rem;
  }
  
  .role-header h4 {
    font-size: 1rem;
    font-weight: 600;
    color: var(--text-accent);
    margin: 0 0 0.5rem;
  }
  
  .role-header .period {
    font-size: 0.7rem;
    color: var(--text-secondary);
    margin: 0 0 1rem;
  }
  
  .highlights-list {
    list-style-type: disc;
    padding-left: 1.25rem;
    margin: 0.5rem 0 1.5rem;
  }
  
  .highlights-list li {
    margin-bottom: 0.75rem;
    color: var(--text-primary);
    line-height: 1.45;
    font-size: 0.75rem;
  }
  
  .tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-top: 1.5rem;
  }
  
  .tech-tag {
    font-size: 0.6rem;
    background: rgba(125, 211, 252, 0.08);
    color: var(--text-accent);
    padding: 0.25rem 0.5rem;
    border-radius: 4px;
    border: 1px solid rgba(125, 211, 252, 0.2);
  }
  
  /* Responsive styles */
  @media (max-width: 768px) {
    .experience-content {
      flex-direction: column;
    }
    
    .company-tabs {
      flex-direction: row;
      overflow-x: auto;
      white-space: nowrap;
      padding-bottom: 0.5rem;
      min-width: auto;
    }
    
    .tab-button {
      border-left: none;
      border-bottom: 2px solid rgba(125, 211, 252, 0.1);
    }
    
    .tab-button.active {
      border-left: none;
      border-bottom: 2px solid var(--text-accent);
    }
    
    .experience-details {
      min-height: 500px;
    }
  }
  
  @media (max-width: 480px) {
    .role-header {
      flex-direction: column;
      gap: 0.5rem;
    }
    
    .experience-section {
      padding: 3rem 0;
    }
  }
</style> 