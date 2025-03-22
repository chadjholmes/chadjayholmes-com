<script lang="ts">
  import { onMount } from 'svelte';
  import WaveHero from '../components/WaveHero.svelte';
  import AboutMe from '../components/AboutMe.svelte';
  import ExperienceSection from '../components/ExperienceSection.svelte';
  import '../styles/global.css';
  
  let scrollY = 0;
  let innerHeight = 0;
  
  // Define Tech interface
  interface Tech {
    name: string;
    icon: string;
    cursorClass: string;
  }
  
  let selectedTech: Tech | null = null;
  let cursorPosition = { x: 0, y: 0 };

  // Tech stack for logo column with brand colors
  const techStack: Tech[] = [
    { name: 'TypeScript', icon: 'devicon-typescript-plain colored', cursorClass: 'cursor-typescript' },
    { name: 'Ruby', icon: 'devicon-ruby-plain colored', cursorClass: 'cursor-ruby' },
    { name: 'Rails', icon: 'devicon-rails-plain colored', cursorClass: 'cursor-rails' },
    { name: 'Swift', icon: 'devicon-swift-plain colored', cursorClass: 'cursor-swift' },
    { name: 'Go', icon: 'devicon-go-plain colored', cursorClass: 'cursor-go' },
    { name: 'Terraform', icon: 'devicon-terraform-plain colored', cursorClass: 'cursor-terraform' },
    { name: 'Docker', icon: 'devicon-docker-plain colored', cursorClass: 'cursor-docker' },
    { name: 'AWS', icon: 'devicon-amazonwebservices-original colored', cursorClass: 'cursor-aws' },
    { name: 'PostgreSQL', icon: 'devicon-postgresql-plain colored', cursorClass: 'cursor-postgresql' }
  ];

  // === Experience Data ===
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
  
  let activeCompany = experiences[0].company;

  // Updated function to also apply cursor hiding class to body
  function setTechCursor(tech: Tech | null): void {
    selectedTech = tech;
    
    // If tech is selected, add class to hide cursor, otherwise remove it
    if (tech) {
      document.body.classList.add('hide-cursor');
    } else {
      document.body.classList.remove('hide-cursor');
    }
  }

  function handleMouseMove(e: MouseEvent): void {
    cursorPosition.x = e.clientX;
    cursorPosition.y = e.clientY;
  }

  onMount(() => {
    // Handle scroll events
    const handleScroll = () => {
      scrollY = window.scrollY;
    };
    
    // Handle window resize
    const handleResize = () => {
      innerHeight = window.innerHeight;
    };
    
    window.addEventListener('scroll', handleScroll);
    window.addEventListener('resize', handleResize);
    
    // Initialize innerHeight
    innerHeight = window.innerHeight;

    window.addEventListener('mousemove', handleMouseMove);

    return () => {
      window.removeEventListener('scroll', handleScroll);
      window.removeEventListener('resize', handleResize);
      window.removeEventListener('mousemove', handleMouseMove);
    };
  });
</script>

<svelte:head>
  <!-- DevIcon font for tech stack icons -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/devicons/devicon@v2.15.1/devicon.min.css">
  <!-- Add page title -->
  <title>Chad Holmes | Senior Software Engineer</title>
  <meta name="description" content="Chad Holmes - Senior Software Engineer specializing in TypeScript, Ruby, and AWS">
  
  <!-- Preload critical resources -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
  <link rel="preload" href="https://cdn.jsdelivr.net/gh/devicons/devicon@v2.15.1/devicon.min.css" as="style">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
</svelte:head>

<div class="page-container {selectedTech ? 'hide-cursor' : ''}">
  <!-- Tech logos column -->
  <div class="tech-logos-column">
    {#each techStack as tech}
      <div 
        class="tech-logo-item {selectedTech === tech ? 'active' : ''}"
        on:click={() => setTechCursor(tech === selectedTech ? null : tech)}
        on:keydown={(e) => e.key === 'Enter' && setTechCursor(tech === selectedTech ? null : tech)}
        tabindex="0"
        role="button"
        aria-pressed={selectedTech === tech}
      >
        <i class={tech.icon}></i>
        <span class="tech-name">{tech.name}</span>
      </div>
    {/each}
  </div>
  
  <!-- Home section with ID -->
  <section id="home">
    <WaveHero {scrollY} {innerHeight} fixedWave={true} />
  </section>
  <section id="about">
    <AboutMe />
  </section>
  <section id="experience">
    <ExperienceSection {experiences} />
  </section>

  <!-- Add a Projects section after Experience -->
  <section id="projects" class="section projects-section">
    <div class="section-container">
      <h2 class="section-title">Projects</h2>
      <p class="section-subtitle">In-depth case studies highlighting my technical challenges and solutions</p>
      
      <div class="projects-vertical">
        <!-- Service Request Platform -->
        <a href="/case-studies/service-request-platform" class="project-item">
          <div class="project-header">
            <div class="project-accent service-accent"></div>
            <span class="project-emoji">⚙️</span>
            <h3>Service Request Middleware</h3>
            <div class="project-arrow">
              <span>→</span>
            </div>
          </div>
          
          <div class="project-bento">
            <div class="bento-grid service-grid">
              <div class="bento-cell main-overview">
                <h4>Project Snapshot</h4>
                <p>Enterprise service orchestration middleware that streamlined request delivery and automated workflows</p>
              </div>
              <div class="bento-cell metrics">
                <span class="bento-icon">👥</span>
                <h5>Leadership</h5>
                <p>Technical leadership for a development team of 4</p>
              </div>
              <div class="bento-cell automation">
                <span class="bento-icon">🚀</span>
                <h5>100% Automation</h5>
                <p>Request placement</p>
              </div>
              <div class="bento-cell tech">
                <h5>Tech Stack</h5>
                <div class="tech-tags">
                  <span>Ruby on Rails</span>
                  <span>PostgreSQL</span>
                  <span>AWS</span>
                  <span>Terraform</span>
                  <span>SNS</span>
                  <span>SQS</span>
                  <span>ECS</span>
                  <span>API Gateway</span>
                </div>
              </div>
            </div>
          </div>
        </a>
        
        <!-- Bookends -->
        <a href="/case-studies/bookends" class="project-item">
          <div class="project-header">
            <div class="project-accent bookends-accent"></div>
            <span class="project-emoji">📱</span>
            <h3>Bookends iOS</h3>
            <div class="project-arrow">
              <span>→</span>
            </div>
          </div>
          
          <div class="project-bento">
            <div class="bento-grid bookends-grid">
              <div class="bento-cell main-overview">
                <h4>Project Snapshot</h4>
                <p>Social reading platform with cloud synchronization and rich interactive features</p>
              </div>
              <div class="bento-cell swift">
                <span class="bento-icon">🔄</span>
                <h5>Swift & SwiftUI</h5>
                <p>Modern iOS development</p>
              </div>
              <div class="bento-cell social">
                <span class="bento-icon">👥</span>
                <h5>Social Features</h5>
                <p>Friend connections & sharing</p>
              </div>
              <div class="bento-cell analytics">
                <span class="bento-icon">☁️</span>
                <h5>Cloud Data Sync</h5>
                <p>Go API and DynamoDB</p>
              </div>
              <div class="bento-cell tech">
                <h5>Tech Stack</h5>
                <div class="tech-tags">
                  <span>Swift</span>
                  <span>SwiftUI</span>
                  <span>Go</span>
                  <span>Lambda</span>
                  <span>DynamoDB</span>
                  <span>API Gateway</span>
                </div>
              </div>
            </div>
          </div>
        </a>
        
        <!-- Data Highway -->
        <a href="/case-studies/data-highway" class="project-item">
          <div class="project-header">
            <div class="project-accent highway-accent"></div>
            <span class="project-emoji">📊</span>
            <h3>Marketplace Data Highway</h3>
            <div class="project-arrow">
              <span>→</span>
            </div>
          </div>
          
          <div class="project-bento">
            <div class="bento-grid highway-grid">
              <div class="bento-cell main-overview">
                <h4>Project Snapshot</h4>
                <p>Real-time data transit solution for marketplace analytics and operations</p>
              </div>
              <div class="bento-cell realtime">
                <span class="bento-icon">⚡</span>
                <h5>Real-time Processing</h5>
                <p>15M+ events daily</p>
              </div>
              <div class="bento-cell search">
                <span class="bento-icon">🔍</span>
                <h5>Opensearch</h5>
                <p>Fast data retrieval</p>
              </div>
              <div class="bento-cell metrics">
                <span class="bento-icon">📈</span>
                <h5>Analytics</h5>
                <p>Custom metrics dashboard</p>
              </div>
              <div class="bento-cell tech">
                <h5>Tech Stack</h5>
                <div class="tech-tags">
                  <span>Ruby on Rails</span>
                  <span>Opensearch</span>
                  <span>Analytics</span>
                </div>
              </div>
            </div>
          </div>
        </a>
      </div>
    </div>
  </section>

  {#if selectedTech}
    <div 
      class="custom-cursor"
      style="left: {cursorPosition.x}px; top: {cursorPosition.y}px"
    >
      <i class={selectedTech.icon}></i>
    </div>
  {/if}
</div>

<style>
  :root {
    /* === Wave Gradient Controls === */
    --gradient-deep-ocean: linear-gradient(
      135deg,
      rgba(41, 128, 185, 1.0),     /* Deep blue base */
      rgba(93, 63, 211, 1.0),      /* Rich purple */
      rgba(52, 152, 219, 1.0),     /* Bright blue */
      rgba(125, 95, 255, 1.0),     /* Vibrant purple */
      rgba(41, 128, 185, 1.0),     /* Back to blue */
      rgba(41, 128, 185, 0.3),     /* Start fade */
      rgba(41, 128, 185, 0.1),     /* Quick fade */
      rgba(41, 128, 185, 0)        /* Sharp cutoff */
    );

    /* Typography Colors */
    --text-primary: rgb(248, 250, 252);    /* Crisp white with slight warmth for confidence */
    --text-accent: rgb(125, 211, 252);     /* Bright sky blue for impact without aggression */
    --text-secondary: rgb(186, 230, 253);  /* Lighter blue for supporting text */
    --text-glow: rgba(125, 211, 252, 0.3); /* Subtle glow for sophistication */
    
    /* Card Colors */
    --gradient-blue: linear-gradient(135deg, #3498db, #2980b9);
    --gradient-purple: linear-gradient(135deg, #9b59b6, #8e44ad);
    --gradient-teal: linear-gradient(135deg, #1abc9c, #16a085);
    
    /* Ticker Controls */
    --ticker-duration: 60s;  /* Increased from 25s to 60s for slower scrolling */
    --ticker-item-gap: 2rem;

    /* Background Colors */
    --bg-primary: rgb(2, 12, 27);  /* Dark blue base */
    --bg-dark: rgb(2, 12, 27);     /* Same dark blue for all sections */
    --bg-secondary: rgb(2, 12, 27); /* Match secondary to the same color */
  }
  
  @keyframes ticker {
    0% {
      transform: translateX(0);
    }
    100% {
      transform: translateX(-50%); /* Move to exactly half of the duplicated set */
    }
  }
  
  .page-container {
    min-height: 300vh;
    position: relative;
    overflow: visible;
    background-color: var(--bg-color); /* Consistent background color */
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  @keyframes bounce {
    0%, 20%, 50%, 80%, 100% { transform: translateY(0) rotate(45deg); }
    40% { transform: translateY(-10px) rotate(45deg); }
    60% { transform: translateY(-5px) rotate(45deg); }
  }

  @keyframes fadeIn {
    to {
      opacity: 1;
    }
  }

  @keyframes slideUp {
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  /* Add responsive breakpoints for fine-tuning */
  @media (max-width: 768px) {
    .tech-logos-column {
      display: none !important;
    }

    .tech-logo-item {
      display: none !important;
    }

    .custom-cursor {
      display: none !important;
    }

    :global(.hero h1) {
      font-size: 2.5rem !important;
      margin-bottom: 1rem !important;
    }
    
    :global(.hero h2) {
      font-size: 1.8rem !important;
    }
    
    #experience {
      margin-top: 0 !important;
      position: relative;
      z-index: 1;
      padding-bottom: 20rem !important;
      background-color: var(--bg-color) !important;
    }
    
    .page-container {
      display: flex;
      flex-direction: column;
      background-color: var(--bg-color) !important;
    }
    
    section {
      width: 100%;
      position: relative;
      background-color: var(--bg-color) !important;
    }
    
    #projects {
      position: relative;
      z-index: 2;
      margin-top: 15rem !important;
      background-color: var(--bg-color) !important;
      padding-top: 5rem !important;
    }

    .projects-section {
      background-color: var(--bg-color) !important;
      min-height: auto !important;
      height: auto !important;
      padding: 3rem 0 !important;
    }
    
    .section-title {
      margin-top: 2rem !important;
      position: relative;
      z-index: 10;
    }

    .project-bento {
      max-height: 800px !important;
      height: auto !important;
      opacity: 1 !important;
      visibility: visible !important;
      display: block !important;
      overflow: visible !important;
      transition: none !important;
      transform: none !important;
      position: relative !important;
      z-index: 6 !important;
    }
    
    .project-item .project-bento {
      display: block !important;
      visibility: visible !important;
      opacity: 1 !important;
      position: relative !important;
      z-index: 6 !important;
    }
    
    .bento-grid {
      display: grid !important;
      visibility: visible !important;
      opacity: 1 !important;
      max-height: none !important;
      overflow: visible !important;
      position: relative !important;
      z-index: 7 !important;
    }
    
    .bento-cell {
      opacity: 1 !important;
      visibility: visible !important;
      display: block !important;
      transform: none !important;
      border-width: 2px !important;
      position: relative !important;
      z-index: 8 !important;
      background-color: rgba(13, 25, 42, 0.9) !important;
      border-color: rgba(125, 211, 252, 0.3) !important;
    }
    
    .bento-cell:hover {
      transform: none !important;
      border-color: rgba(125, 211, 252, 0.5) !important;
    }
    
    .service-grid .bento-cell,
    .bookends-grid .bento-cell,
    .highway-grid .bento-cell {
      background-color: rgba(13, 25, 42, 0.9) !important;
    }
    
    .service-grid .bento-cell:hover,
    .bookends-grid .bento-cell:hover,
    .highway-grid .bento-cell:hover {
      border-color: rgba(125, 211, 252, 0.5) !important;
    }
    
    .project-item:hover .project-bento,
    .project-item .project-bento {
      max-height: 800px !important;
      visibility: visible !important;
      opacity: 1 !important;
      overflow: visible !important;
    }

    .custom-cursor {
      display: none !important;
    }

    :global(h3.company-name) {
      font-size: 1.5rem !important;
    }

    .tech-logo-item, .project-arrow, button, a {
      min-height: 44px;
      min-width: 44px;
    }

    :global(nav) {
      padding: 0.75rem 1rem !important;
      background-color: rgba(2, 12, 27, 0.95) !important;
      backdrop-filter: blur(10px) !important;
    }
    
    .project-item {
      margin-bottom: 3rem !important;
      width: 100% !important;
      position: relative !important;
      z-index: 5 !important;
    }

    .bento-cell p, .tech-tags span {
      color: var(--text-primary) !important;
      font-weight: 500 !important;
    }

    section > div {
      padding: 0 1rem;
    }
  }

  @media (max-width: 480px) {
    /* Further refinements for smallest screens */
    .tech-logos-column {
      gap: 0.5rem;
    }

    /* Even larger name on smallest screens */
    :global(.hero h1) {
      font-size: 2.8rem !important;
      line-height: 1.2 !important;
    }
    
    :global(.hero h2) {
      font-size: 1.6rem !important;
    }
    
    /* More section separation for smallest screens */
    #experience {
      padding-bottom: 25rem !important; /* Even more padding on smallest screens */
    }
    
    #projects {
      margin-top: 20rem !important; /* Even more margin on smallest screens */
      padding-top: 7rem !important;
    }

    /* Additional optimizations for smallest screens */
    .bento-grid {
      gap: 1rem !important;
    }

    /* Prevent text overflow on small screens */
    h1, h2, h3, h4, h5, p {
      overflow-wrap: break-word !important;
      word-wrap: break-word !important;
      hyphens: auto !important;
    }

    /* Enhance touch targets further */
    .tech-tags span {
      padding: 0.5rem 0.75rem !important;
      margin-bottom: 0.5rem !important;
    }

    /* Improve scrolling experience */
    html, body {
      scroll-padding-top: 70px !important; /* Adjust for fixed header */
    }

    /* Make sure hover states don't persist on touch devices */
    .project-item:hover {
      transform: none !important;
    }
  }

  /* Add tablet breakpoint for mid-sized devices */
  @media (min-width: 481px) and (max-width: 768px) {
   
  }

  /* Ensure z-index hierarchy works correctly */
  .page-container {
    min-height: 300vh;
    position: relative;
    overflow: visible;
  }

  /* Component z-index ordering */
  :global(.hero .content) {
    z-index: 20 !important; /* Ensure hero content is above parallax and wave */
  }
  
  :global(.navbar) {
    z-index: 100 !important; /* Keep navbar on top */
  }

  :global(.gradient-container) {
    z-index: 10 !important; /* Keep wave above content sections but below navbar and hero content */
  }

  /* Section spacing */
  #home {
    background-color: var(--bg-color);
  }
  
  #about {
    padding-top: 5rem; /* Add padding to the top for better visibility below navbar */
    background-color: var(--bg-color);
  }
  
  #experience {
    margin-top: 50vh; /* Reduced from 100vh to create reasonable spacing without exaggerating */
    padding-top: 5rem; /* Add padding to the top for better visibility below navbar */
    padding-bottom: 20vh; /* Add padding at the bottom for better spacing */
    background-color: var(--bg-color);
  }

  /* Tech logos column styling */
  .tech-logos-column {
    position: fixed;
    top: 6rem;
    left: 1rem;
    right: auto;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    z-index: 50;
  }

  .tech-logo-item {
    display: flex;
    align-items: center;
    background: rgba(13, 25, 42, 0.8);
    backdrop-filter: blur(8px);
    border: 1px solid rgba(125, 211, 252, 0.1);
    border-radius: 50%;
    width: 2.25rem;
    height: 2.25rem;
    justify-content: center;
    position: relative;
    transition: all 0.3s ease;
    cursor: pointer;
    overflow: visible;
  }

  .tech-logo-item i {
    font-size: 1.2rem;
    transition: all 0.3s ease;
  }

  .tech-logo-item .tech-name {
    position: absolute;
    left: 3.25rem;
    right: auto;
    top: 50%;
    transform: translate(-20px, -50%);
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.95), rgba(41, 128, 185, 0.9));
    padding: 0.35rem 0.75rem;
    border-radius: 0.4rem;
    opacity: 0;
    transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    pointer-events: none;
    white-space: nowrap;
    color: var(--text-primary);
    font-size: 0.75rem;
    border: 1px solid rgba(125, 211, 252, 0.3);
    box-shadow: 
      0 4px 12px rgba(0, 0, 0, 0.3),
      0 0 15px var(--text-glow);
    z-index: 100;
    font-weight: 500;
    min-width: 80px;
    text-align: center;
    letter-spacing: 0.02em;
  }

  /* Add arrow/triangle to tooltip */
  .tech-logo-item .tech-name::before {
    content: '';
    position: absolute;
    left: -6px;
    top: 50%;
    transform: translateY(-50%);
    width: 0;
    height: 0;
    border-top: 5px solid transparent;
    border-bottom: 5px solid transparent;
    border-right: 5px solid rgba(125, 211, 252, 0.3);
  }

  .tech-logo-item .tech-name::after {
    content: '';
    position: absolute;
    left: -4px;
    top: 50%;
    transform: translateY(-50%);
    width: 0;
    height: 0;
    border-top: 4px solid transparent;
    border-bottom: 4px solid transparent;
    border-right: 4px solid rgba(41, 128, 185, 0.9);
  }

  .tech-logo-item:hover .tech-name {
    opacity: 1;
    transform: translate(0, -50%) scale(1);
    animation: pulseGlow 2s infinite alternate;
  }

  /* Make sure tooltip stays visible when tech is selected as cursor */
  .tech-logo-item.active .tech-name {
    opacity: 1;
    transform: translate(0, -50%) scale(1);
    animation: pulseGlow 2s infinite alternate;
  }

  @keyframes pulseGlow {
    0% {
      box-shadow: 
        0 4px 12px rgba(0, 0, 0, 0.3),
        0 0 15px var(--text-glow);
    }
    100% {
      box-shadow: 
        0 4px 12px rgba(0, 0, 0, 0.3),
        0 0 25px var(--text-glow);
    }
  }

  /* Add subtle floating animation to the icon when active */
  .tech-logo-item.active i {
    animation: float 3s ease-in-out infinite;
  }

  @keyframes float {
    0% {
      transform: translateY(0px);
    }
    50% {
      transform: translateY(-5px);
    }
    100% {
      transform: translateY(0px);
    }
  }

  /* Cursor styles for each technology */
  .cursor-typescript {
    cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><text y="20" font-size="20">TS</text></svg>'), auto;
  }
  
  .cursor-ruby {
    cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><text y="20" font-size="20" fill="red">♦</text></svg>'), auto;
  }
  
  .cursor-rails {
    cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><text y="20" font-size="20" fill="red">🛤️</text></svg>'), auto;
  }
  
  .cursor-swift {
    cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><text y="20" font-size="20" fill="orange">⚡</text></svg>'), auto;
  }
  
  .cursor-go {
    cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><text y="20" font-size="20" fill="skyblue">GO</text></svg>'), auto;
  }
  
  .custom-cursor {
    position: fixed;
    width: 32px;
    height: 32px;
    transform: translate(-50%, -50%);
    pointer-events: none;
    z-index: 9999;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .custom-cursor i {
    font-size: 24px;
    /* Optional - add a slight drop shadow for visibility */
    filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
  }

  /* Add this to your styles */
  .hide-cursor, 
  .hide-cursor * {
    cursor: none !important;
  }

  /* Projects Section Styles */
  .projects-section {
    background-color: var(--bg-color);
    padding: 6rem 0;
    height: 100vh;
    /* Update text scaling for projects section */
    font-size: 0.8em;
  }
  
  .section-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 clamp(1rem, 3vw, 2rem);
    width: 100%;
  }
  
  .section-title {
    font-size: clamp(1.8rem, 6.5vw, 5.5rem); /* Increased to match About Me and Experience sections */
    color: var(--text-primary);
    margin: 0 0 1rem;
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
  
  .section-subtitle {
    font-size: clamp(0.8rem, 1.6vw, 1rem); /* Slightly increased */
    color: var(--text-secondary);
    margin: 0 0 4rem;
    text-align: left;
    max-width: 700px;
  }
  
  /* Vertical Projects with Interactive Bento */
  .projects-vertical {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
    max-width: 900px;
    margin: 0 auto;
  }
  
  .project-item {
    position: relative;
    background: rgba(13, 25, 42, 0.7);
    border: 1px solid rgba(125, 211, 252, 0.15);
    border-radius: 1rem;
    overflow: hidden;
    text-decoration: none;
    color: var(--text-primary);
    transition: all 0.3s ease;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  }
  
  .project-item:hover {
    transform: translateY(-5px);
    border-color: rgba(125, 211, 252, 0.3);
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
  }
  
  .project-header {
    display: flex;
    align-items: center;
    padding: 1.5rem;
    position: relative;
  }
  
  .project-accent {
    width: 4px;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
  }
  
  .service-accent {
    background: linear-gradient(to bottom, #3498db, #2980b9);
  }
  
  .bookends-accent {
    background: linear-gradient(to bottom, #9b59b6, #8e44ad);
  }
  
  .highway-accent {
    background: linear-gradient(to bottom, #2ecc71, #27ae60);
  }
  
  .project-emoji {
    font-size: 1.3rem; /* Down from 1.4rem */
    margin-right: 1rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .project-item h3 {
    font-size: 0.95rem; /* Down from 1rem */
    color: var(--text-accent);
    margin: 0;
    flex: 1;
  }
  
  .project-arrow {
    font-size: 0.95rem; /* Down from 1rem */
    color: var(--text-accent);
    opacity: 0.5;
    transition: all 0.3s ease;
    margin-left: 1rem;
  }
  
  .project-item:hover .project-arrow {
    opacity: 1;
    transform: translateX(5px);
  }
  
  /* Bento Grid that appears on hover */
  .project-bento {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    opacity: 0; /* Start with opacity 0 */
    visibility: hidden; /* Start hidden */
  }
  
  .project-item:hover .project-bento {
    max-height: 800px; /* Increased from 400px to ensure enough space */
    opacity: 1; /* Fade in */
    visibility: visible; /* Make visible */
  }
  
  /* Bento Grid Styles */
  .bento-grid {
    display: grid;
    grid-gap: 0.6rem; /* Reduced from 0.7rem */
    opacity: 0;
    visibility: hidden;
    max-height: 0;
    overflow: hidden;
    transition: all 0.4s ease;
    will-change: transform, opacity;
    padding: 0 1.5rem 1.5rem;
    height: 100%;
  }
  
  .project-item:hover .bento-grid {
    opacity: 1;
    visibility: visible;
    max-height: 800px;
    overflow: visible;
  }
  
  /* Service Request Platform Grid */
  .service-grid {
    grid-template-areas:
      "main main main main"
      "metrics metrics automation automation"
      "tech tech tech tech";
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: auto auto auto;
  }
  
  /* Bookends Grid */
  .bookends-grid {
    grid-template-areas:
      "main main main main"
      "swift swift social social"
      "analytics analytics tech tech";
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: auto auto auto;
  }
  
  /* Data Highway Grid */
  .highway-grid {
    grid-template-areas:
      "main main main main"
      "realtime realtime search search"
      "metrics metrics tech tech";
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: auto auto auto;
  }
  
  /* Project bento cell background styles override */
  .main-overview, .metrics, .automation, .tech, 
  .swift, .social, .analytics, .realtime, .search {
    background-color: rgba(13, 25, 42, 0.8);
    border: 1px solid rgba(125, 211, 252, 0.2);
    transition: all 0.3s ease;
  }
  
  /* Cell Placements */
  .main-overview {
    grid-area: main;
  }
  
  .metrics {
    grid-area: metrics;
  }
  
  .automation {
    grid-area: automation;
  }
  
  .swift {
    grid-area: swift;
  }
  
  .social {
    grid-area: social;
  }
  
  .analytics {
    grid-area: analytics;
  }
  
  .tech {
    grid-area: tech;
  }
  
  .realtime {
    grid-area: realtime;
  }
  
  .search {
    grid-area: search;
  }
  
  .bento-cell {
    background: rgba(13, 25, 42, 0.6);
    border-radius: 0.5rem;
    padding: 1rem;
    transition: all 0.3s ease;
    animation: fadeIn 0.5s forwards;
    position: relative;
    overflow: hidden;
  }
  
  /* Service Request Platform Cells */
  .service-grid .bento-cell {
    border: 1px solid rgba(52, 152, 219, 0.2);
  }
  
  .service-grid .main-overview {
    background: linear-gradient(135deg, rgba(41, 128, 185, 0.15), rgba(52, 152, 219, 0.05));
  }
  
  .service-grid .metrics {
    background: linear-gradient(135deg, rgba(41, 128, 185, 0.2), rgba(52, 152, 219, 0.05));
  }
  
  .service-grid .automation {
    background: linear-gradient(135deg, rgba(41, 128, 185, 0.1), rgba(52, 152, 219, 0.03));
  }
  
  .service-grid .tech {
    background: linear-gradient(135deg, rgba(41, 128, 185, 0.05), rgba(52, 152, 219, 0.01));
  }
  
  /* Bookends Cells */
  .bookends-grid .bento-cell {
    border: 1px solid rgba(155, 89, 182, 0.2);
  }
  
  .bookends-grid .main-overview {
    background: linear-gradient(135deg, rgba(142, 68, 173, 0.15), rgba(155, 89, 182, 0.05));
  }
  
  .bookends-grid .swift {
    background: linear-gradient(135deg, rgba(142, 68, 173, 0.2), rgba(155, 89, 182, 0.05));
  }
  
  .bookends-grid .social {
    background: linear-gradient(135deg, rgba(142, 68, 173, 0.1), rgba(155, 89, 182, 0.03));
  }
  
  .bookends-grid .analytics {
    background: linear-gradient(135deg, rgba(142, 68, 173, 0.15), rgba(155, 89, 182, 0.05));
  }
  
  .bookends-grid .tech {
    background: linear-gradient(135deg, rgba(142, 68, 173, 0.05), rgba(155, 89, 182, 0.01));
  }
  
  /* Highway Cells */
  .highway-grid .bento-cell {
    border: 1px solid rgba(46, 204, 113, 0.2);
  }
  
  .highway-grid .main-overview {
    background: linear-gradient(135deg, rgba(39, 174, 96, 0.15), rgba(46, 204, 113, 0.05));
  }
  
  .highway-grid .realtime {
    background: linear-gradient(135deg, rgba(39, 174, 96, 0.2), rgba(46, 204, 113, 0.05));
  }
  
  .highway-grid .search {
    background: linear-gradient(135deg, rgba(39, 174, 96, 0.1), rgba(46, 204, 113, 0.03));
  }
  
  .highway-grid .metrics {
    background: linear-gradient(135deg, rgba(39, 174, 96, 0.15), rgba(46, 204, 113, 0.05));
  }
  
  .highway-grid .tech {
    background: linear-gradient(135deg, rgba(39, 174, 96, 0.05), rgba(46, 204, 113, 0.01));
  }
  
  .bento-cell:hover {
    transform: translateY(-3px);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  }
  
  .service-grid .bento-cell:hover {
    border-color: rgba(52, 152, 219, 0.4);
  }
  
  .bookends-grid .bento-cell:hover {
    border-color: rgba(155, 89, 182, 0.4);
  }
  
  .highway-grid .bento-cell:hover {
    border-color: rgba(46, 204, 113, 0.4);
  }
  
  .main-overview {
    background: rgba(13, 25, 42, 0.8);
  }
  
  .bento-icon {
    font-size: 0.95rem; /* Down from 1rem */
    margin-bottom: 0.5rem;
    display: inline-block;
  }
  
  .bento-cell h4 {
    margin: 0 0 0.5rem;
    color: var(--text-accent);
    font-size: 0.8rem; /* Down from 0.85rem */
  }
  
  .bento-cell h5 {
    margin: 0 0 0.5rem;
    color: var(--text-accent);
    font-size: 0.7rem; /* Down from 0.75rem */
  }
  
  .bento-cell p {
    color: var(--text-secondary);
    font-size: 0.6rem; /* Down from 0.65rem */
    margin: 0;
    line-height: 1.4;
  }
  
  /* Tech tags */
  .tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  
  .tech-tags span {
    font-size: 0.5rem; /* Down from 0.55rem */
    background: rgba(125, 211, 252, 0.1);
    color: var(--text-accent);
    padding: 0.25rem 0.5rem;
    border-radius: 4px;
    border: 1px solid rgba(125, 211, 252, 0.2);
  }
  
  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  /* Responsive adjustments */
  @media (max-width: 768px) {
    .service-grid, .bookends-grid, .highway-grid {
      grid-template-areas:
        "main main"
        "metrics automation"
        "tech tech";
      grid-template-columns: 1fr 1fr;
    }
    
    .bookends-grid {
      grid-template-areas:
        "main main"
        "swift social"
        "analytics tech";
    }
    
    .highway-grid {
      grid-template-areas:
        "main main"
        "realtime search"
        "metrics tech";
    }
  }
  
  @media (max-width: 480px) {
    .service-grid, .bookends-grid, .highway-grid {
      grid-template-areas:
        "main"
        "metrics"
        "automation"
        "tech";
      grid-template-columns: 1fr;
    }
    
    .bookends-grid {
      grid-template-areas:
        "main"
        "swift"
        "social"
        "analytics"
        "tech";
    }
    
    .highway-grid {
      grid-template-areas:
        "main"
        "realtime"
        "search"
        "metrics"
        "tech";
    }
  }

  /* Add landscape mode optimizations */
  @media (max-height: 480px) and (orientation: landscape) {
    .tech-logos-column {
      bottom: 0.5rem;
      padding: 0.25rem;
    }

    .tech-logo-item {
      width: 1.5rem;
      height: 1.5rem;
    }

    .tech-logo-item i {
      font-size: 0.8rem;
    }

    /* Adjust section spacing for landscape */
    .section {
      padding: 1.5rem 1rem;
    }
    
    #about, #experience, #projects {
      padding-top: 2rem;
      margin-top: 0;
    }
    
    /* Adjust name size for landscape */
    :global(.hero h1) {
      font-size: 2.2rem !important;
    }
    
    :global(.hero h2) {
      font-size: 1.4rem !important;
    }

    /* Additional landscape optimizations */
    /* Adjust content to be more horizontally oriented */
    .projects-vertical {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 1rem;
    }

    /* Make experience cards more compact */
    :global(.role-highlight) {
      margin-bottom: 0.5rem !important;
    }

    /* Reduce vertical padding in all elements */
    .bento-cell, .project-header {
      padding: 0.5rem !important;
    }

    /* Keep content visible without excessive scrolling */
    .page-container {
      min-height: auto !important;
    }

    /* Ensure navbar is minimal */
    :global(nav) {
      height: 40px !important;
    }

    /* Make hero section more compact */
    :global(.hero) {
      min-height: 70vh !important;
    }
  }
</style>
