<script lang="ts">
  import { fade, slide } from 'svelte/transition';

  let showFullResume = false;
  let showContact = false;

  const summaryPoints = [
    "Back-end specialist + front-end and cloud infrastructure generalist",
    "Architect of high-throughput event processing systems on AWS",
    "Expert in Ruby on Rails, TypeScript, and Node.js ecosystems",
    "Technical team lead with proven product delivery experience"
  ];

  interface Experience {
    company: string;
    roles: {
      title: string;
      period: string;
      highlights: string[];
      technologies?: string[];
    }[];
    manager?: {
      name: string;
      email?: string;
      phone?: string;
    };
  }

  const experiences: Experience[] = [
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
      ],
      manager: {
        name: "Rob Bailey",
        email: "robertdbailey@gmail.com"
      }
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
      ],
      manager: {
        name: "Stephanie Cruz",
        email: "stephanie.cruz@ukg.com"
      }
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
      ],
      manager: {
        name: "Marr Bowen",
        phone: "801-690-0677"
      }
    }
  ];

  let activeCompany = experiences[0].company;
  let visible = true;

  function setActiveCompany(company: string) {
    activeCompany = company;
  }
</script>

<div class="experience-page">
  <div class="content">
    <header class="resume-header">
      <div class="title-section">
        <h1>Chad Holmes</h1>
        <h2>Senior Software Engineer</h2>
        
        <div class="actions">
          <button 
            class="contact-button" 
            on:click={() => showContact = !showContact}
            aria-expanded={showContact}
          >
            Contact
          </button>
          <button 
            class="resume-toggle" 
            on:click={() => showFullResume = !showFullResume}
            aria-expanded={showFullResume}
          >
            {showFullResume ? 'Hide Resume' : 'View Resume'}
          </button>
        </div>
      </div>
    </header>

    {#if showContact}
      <div class="contact-info" transition:slide={{ duration: 150 }}>
        <a href="tel:9517190522" class="contact-link">
          <span class="icon">📱</span> 951-719-0522
        </a>
        <a href="mailto:chadjayholmes@gmail.com" class="contact-link">
          <span class="icon">📧</span> chadjayholmes@gmail.com
        </a>
      </div>
    {/if}

    <div class="summary-section">
      <ul class="summary-bullets">
        {#each summaryPoints as point}
          <li>{point}</li>
        {/each}
      </ul>
      
      <div class="key-skills">
        <span class="skill-tag">Ruby</span>
        <span class="skill-tag">TypeScript</span>
        <span class="skill-tag">AWS</span>
        <span class="skill-tag">Terraform</span>
        <span class="skill-tag">Node.js</span>
        <span class="skill-tag">API Development</span>
      </div>
    </div>

    {#if showFullResume}
      <div class="resume-container" transition:slide={{ duration: 300 }}>
        <div class="two-column-layout">
          <section class="main-column">
            <h3>Professional Experience</h3>
            
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

            <div class="experience-details">
              {#each experiences as exp}
                {#if activeCompany === exp.company}
                  <div class="company-section" transition:fade={{ duration: 150 }}>
                    <div class="company-info">
                      <h4 class="company-name">{exp.company}</h4>
                    </div>
                    
                    {#each exp.roles as role}
                      <div class="role">
                        <div class="role-header">
                          <h4>{role.title}</h4>
                          <span class="period">{role.period}</span>
                        </div>
                        <ul class="highlights">
                          {#each role.highlights as highlight}
                            <li>{highlight}</li>
                          {/each}
                        </ul>
                        {#if role.technologies}
                          <div class="role-technologies">
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
          </section>

          <section class="sidebar">
            <!-- Education card removed -->
          </section>
        </div>
      </div>
    {:else}
      <div class="career-highlights">
        <h3>Highlights</h3>
        <div class="highlight-cards">
          <a href="/case-studies/service-request-platform" class="highlight-card-link">
            <div class="highlight-card">
              <div class="highlight-icon">👨‍💻</div>
              <h4>Technical Leadership</h4>
              <p>Led a team of 4 engineers to deliver a green-field product from concept to production.</p>
              <span class="read-case-study">Read Case Study →</span>
            </div>
          </a>
          <div class="highlight-card">
            <div class="highlight-icon">⚡</div>
            <h4>Performance Optimization</h4>
            <p>Reduced authentication speeds from >1000ms to under 10ms through custom authorizer implementation.</p>
          </div>
          <a href="/experience/education" class="highlight-card-link">
            <div class="highlight-card">
              <div class="highlight-icon">🏆</div>
              <h4>Academic Excellence</h4>
              <p>Graduated with a 3.92 major GPA in Computer Science from Brigham Young University.</p>
              <span class="read-case-study">View Transcript →</span>
            </div>
          </a>
        </div>
      </div>
    {/if}
  </div>
</div>

<style>
  .experience-page {
    min-height: 100vh;
    background-color: var(--bg-color);
    color: var(--text-color);
    position: relative;
  }

  .content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 3rem clamp(1rem, 3vw, 2rem);
    position: relative;
  }

  .resume-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 2rem;
  }

  .title-section h1 {
    font-size: clamp(2.5rem, 6vw, 4rem);
    color: var(--text-primary);
    margin: 0;
    line-height: 1.1;
  }

  .title-section h2 {
    font-size: clamp(1.25rem, 3vw, 1.75rem);
    color: var(--text-accent);
    margin: 0.5rem 0 1rem;
    font-weight: 400;
  }

  .actions {
    display: flex;
    gap: 1rem;
    margin-top: 1rem;
    flex-wrap: wrap;
  }

  .contact-button, 
  .resume-toggle {
    background: rgba(125, 211, 252, 0.1);
    border: 1px solid var(--text-accent);
    color: var(--text-accent);
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-size: 0.9rem;
  }

  .resume-toggle {
    background: rgba(125, 211, 252, 0.2);
  }

  .contact-button:hover,
  .resume-toggle:hover {
    background: rgba(125, 211, 252, 0.3);
    transform: translateY(-2px);
  }

  .contact-info {
    background: rgba(0, 0, 0, 0.2);
    border: 1px solid rgba(125, 211, 252, 0.2);
    border-radius: 8px;
    padding: 1rem;
    margin-bottom: 2rem;
    display: flex;
    gap: 2rem;
  }

  .contact-link {
    color: var(--text-accent);
    text-decoration: none;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    transition: color 0.3s ease;
  }

  .contact-link:hover {
    color: var(--text-primary);
  }

  .icon {
    font-size: 1.2rem;
  }

  .summary-section {
    margin: 3rem 0;
    max-width: 800px;
  }

  .summary-section p {
    font-size: clamp(1.1rem, 2vw, 1.3rem);
    line-height: 1.6;
    color: var(--text-secondary);
    margin-bottom: 1.5rem;
  }

  .key-skills {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 2rem;
  }

  .skill-tag {
    background: rgba(125, 211, 252, 0.15);
    color: var(--text-accent);
    padding: 0.35rem 1rem;
    border-radius: 4px;
    font-size: 1rem;
    border: 1px solid rgba(125, 211, 252, 0.3);
  }

  .career-highlights {
    margin-top: 3rem;
  }

  .highlight-cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    margin-top: 1.5rem;
  }

  .highlight-card {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 8px;
    padding: 2rem;
    border: 1px solid rgba(125, 211, 252, 0.2);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .highlight-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  }

  .highlight-icon {
    font-size: 2.5rem;
    margin-bottom: 1rem;
  }

  .highlight-card h4 {
    font-size: 1.4rem;
    color: var(--text-accent);
    margin: 0 0 1rem;
  }

  .highlight-card p {
    color: var(--text-secondary);
    line-height: 1.6;
    font-size: 1.1rem;
  }

  .highlight-card-link {
    text-decoration: none;
    color: inherit;
    display: block;
  }

  .read-case-study {
    display: inline-block;
    margin-top: 1rem;
    color: var(--text-accent);
    font-size: 0.9rem;
    transition: transform 0.2s ease;
  }

  .highlight-card:hover .read-case-study {
    transform: translateX(5px);
  }

  .resume-container {
    margin-top: 3rem;
  }

  .two-column-layout {
    display: grid;
    grid-template-columns: 1fr 300px;
    gap: 2rem;
    align-items: start;
  }

  .main-column {
    width: 100%;
  }

  .sidebar {
    position: sticky;
    top: 2rem;
    width: 100%;
  }

  h3 {
    font-size: clamp(1.4rem, 3vw, 1.8rem);
    color: var(--text-primary);
    margin-bottom: 1.5rem;
  }

  .company-tabs {
    display: flex;
    gap: 0.5rem;
    margin-bottom: 1.5rem;
    flex-wrap: wrap;
    border-bottom: 1px solid rgba(125, 211, 252, 0.2);
    padding-bottom: 0.5rem;
  }

  .tab-button {
    background: transparent;
    border: none;
    color: var(--text-secondary);
    font-size: 1.1rem;
    padding: 0.5rem 1rem;
    cursor: pointer;
    transition: all 0.3s ease;
    border-radius: 4px 4px 0 0;
    position: relative;
  }

  .tab-button:hover {
    color: var(--text-primary);
    background: rgba(125, 211, 252, 0.1);
  }

  .tab-button.active {
    color: var(--text-primary);
    background: rgba(125, 211, 252, 0.15);
  }

  .tab-button.active::after {
    content: '';
    position: absolute;
    bottom: -0.5rem;
    left: 0;
    right: 0;
    height: 2px;
    background-color: var(--text-accent);
  }

  .experience-details {
    position: relative;
    min-height: 500px; /* Adjust based on your tallest content */
  }

  .company-section {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
  }

  .company-name {
    font-size: 1.4rem;
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
    font-size: 1.25rem;
    color: var(--text-accent);
    margin: 0;
  }

  .period {
    font-family: var(--font-mono);
    color: var(--text-secondary);
    font-size: 0.9rem;
  }

  .highlights {
    list-style: none;
    padding: 0;
    margin: 1rem 0;
  }

  .highlights li {
    position: relative;
    padding-left: 1.5rem;
    margin-bottom: 0.75rem;
    color: var(--text-secondary);
    line-height: 1.6;
  }

  .highlights li::before {
    content: "▹";
    position: absolute;
    left: 0;
    color: var(--text-accent);
  }

  .role-technologies {
    margin-top: 1rem;
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .tech-tag {
    background: rgba(125, 211, 252, 0.1);
    color: var(--text-secondary);
    padding: 0.25rem 0.75rem;
    border-radius: 4px;
    font-size: 0.9rem;
    border: 1px solid rgba(125, 211, 252, 0.2);
  }

  .education-card {
    background: rgba(0, 0, 0, 0.15);
    border-radius: 8px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  }

  .education-card h3 {
    font-size: 1.4rem;
    margin-bottom: 1rem;
    color: var(--text-primary);
    border-bottom: 1px solid rgba(125, 211, 252, 0.2);
    padding-bottom: 0.5rem;
  }

  .education-content {
    padding: 0.5rem 0;
  }

  .education-header {
    margin-bottom: 1rem;
  }

  .education-header h4 {
    font-size: 1.25rem;
    color: var(--text-accent);
    margin: 0;
  }

  .degree {
    color: var(--text-secondary);
    margin: 0.5rem 0;
    font-family: var(--font-mono);
  }

  .gpa {
    color: var(--text-primary);
    margin: 0.5rem 0 1rem;
    font-weight: 500;
  }

  .honors {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .honor-tag {
    background: rgba(125, 211, 252, 0.1);
    color: var(--text-accent);
    padding: 0.25rem 0.75rem;
    border-radius: 4px;
    font-size: 0.9rem;
    border: 1px solid rgba(125, 211, 252, 0.2);
  }

  .summary-bullets {
    list-style: none;
    padding: 0;
    margin: 2rem 0;
  }

  .summary-bullets li {
    position: relative;
    padding-left: 2rem;
    margin-bottom: 1.5rem;
    color: var(--text-secondary);
    line-height: 1.7;
    font-size: clamp(1.1rem, 2vw, 1.3rem);
  }

  .summary-bullets li::before {
    content: "▹";
    position: absolute;
    left: 0;
    color: var(--text-accent);
    font-size: 1.2em;
    top: 0.1em;
  }

  @media (max-width: 900px) {
    .two-column-layout {
      grid-template-columns: 1fr;
    }

    .sidebar {
      position: relative;
      top: 0;
      margin-top: 2rem;
    }

    .education-card {
      max-width: 100%;
    }

    .highlight-cards {
      grid-template-columns: 1fr;
    }
  }

  @media (max-width: 768px) {
    .resume-header {
      flex-direction: column;
      gap: 1rem;
    }

    .contact-info {
      flex-direction: column;
      gap: 1rem;
    }

    .tab-button {
      font-size: 1rem;
      padding: 0.4rem 0.8rem;
    }

    .role {
      padding: 1rem;
    }

    .role-header {
      flex-direction: column;
      gap: 0.5rem;
    }
  }

  @media (max-width: 480px) {
    .content {
      padding: 1.5rem 1rem;
    }

    .title-section h1 {
      font-size: 2rem;
    }

    .title-section h2 {
      font-size: 1.25rem;
    }

    .company-tabs {
      overflow-x: auto;
      padding-bottom: 1rem;
      margin: 0 -1rem 1rem -1rem;
      padding: 0 1rem 1rem 1rem;
    }

    .tech-tag,
    .honor-tag,
    .skill-tag {
      font-size: 0.8rem;
      padding: 0.2rem 0.5rem;
    }
  }
</style>