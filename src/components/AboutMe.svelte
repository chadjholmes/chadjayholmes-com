<script lang="ts">
  // AboutMe.svelte
  import { onMount, onDestroy } from 'svelte';
  
  // Add function to scroll to sections
  function scrollToSection(sectionId: string) {
    const section = document.getElementById(sectionId);
    if (section) {
      // Get navbar height to use as offset
      const navbar = document.querySelector('.navbar');
      const navbarHeight = navbar ? navbar.getBoundingClientRect().height : 0;
      
      // Calculate the element's position relative to the top of the page
      const sectionPosition = section.getBoundingClientRect().top + window.scrollY;
      
      // Scroll to element with offset for navbar
      window.scrollTo({
        top: sectionPosition - navbarHeight - 20, // Added 20px extra padding
        behavior: 'smooth'
      });
    }
  }
  
  let activeSocialStyle = 'orbit'; // Options: 'orbit', 'rocket', 'portal'
  
  function setSocialStyle(style: string) {
    activeSocialStyle = style;
  }
  
  // Social links data
  const socialLinks = {
    github: "https://github.com/chadjholmes",
    linkedin: "https://linkedin.com/in/chadjayholmes"
  };
  
  let displayText = "";
  let currentIndex = 0;
  let animationStarted = false;
  let typingComplete = false;
  let codeContainerRef: HTMLDivElement; // Add proper type annotation
  
  // Format with each key-value pair on a single line
  let fullText = `const socialLinks = {
  github: "github.com/chadjholmes",
  linkedin: "linkedin.com/in/chadjayholmes"
};`;
  
  // Typing animation
  function typeText() {
    if (currentIndex < fullText.length) {
      displayText += fullText[currentIndex];
      currentIndex++;
      
      // Check if we just typed the semicolon (ending character)
      if (currentIndex === fullText.length) {
        // Wait a moment before applying final styling
        setTimeout(() => {
          typingComplete = true;
        }, 300);
      }
      
      setTimeout(typeText, Math.random() * 50 + 30);
    }
  }
  
  // Start animation only when visible
  function startAnimationIfVisible(entries: IntersectionObserverEntry[]) {
    const [entry] = entries;
    if (entry.isIntersecting && !animationStarted) {
      animationStarted = true;
      setTimeout(typeText, 500); // Start typing after a short delay
    }
  }
  
  let observer: IntersectionObserver;
  
  onMount(() => {
    // Create an observer to watch when the container enters viewport
    observer = new IntersectionObserver(startAnimationIfVisible, {
      root: null, // viewport
      threshold: 0.1 // trigger when at least 10% is visible
    });
    
    // Start observing the container
    if (codeContainerRef) {
      observer.observe(codeContainerRef);
    }
  });
  
  onDestroy(() => {
    // Clean up the observer when component is destroyed
    if (observer && codeContainerRef) {
      observer.unobserve(codeContainerRef);
      observer.disconnect();
    }
  });
  
  // Function to make the JSON links clickable
  function formatTextWithLinks(text: string) {
    // Replace URLs with clickable links
    let formatted = text;
    
    if (formatted.includes("github.com")) {
      const githubUrl = "github.com/chadjholmes";
      const fullGithubUrl = "https://" + githubUrl;
      formatted = formatted.replace(
        `"${githubUrl}"`, 
        `<a href="${fullGithubUrl}" target="_blank" rel="noopener noreferrer" class="json-link ${typingComplete ? 'link-complete' : ''}">"${githubUrl}"</a>`
      );
    }
    
    if (formatted.includes("linkedin.com")) {
      const linkedinUrl = "linkedin.com/in/chadjayholmes";
      const fullLinkedinUrl = "https://" + linkedinUrl;
      formatted = formatted.replace(
        `"${linkedinUrl}"`, 
        `<a href="${fullLinkedinUrl}" target="_blank" rel="noopener noreferrer" class="json-link ${typingComplete ? 'link-complete' : ''}">"${linkedinUrl}"</a>`
      );
    }
    
    return formatted;
  }
</script>

<div class="about-me-container">
  <h1 class="about-me">About Me</h1>
  
  <!-- JSON Code Block Social Links -->
  <div class="json-code-container" bind:this={codeContainerRef} class:syntax-complete={typingComplete}>
    <pre class="json-code"><code>{@html formatTextWithLinks(displayText)}<span class="cursor" class:hidden={typingComplete}>|</span></code></pre>
  </div>
</div>

<div class="triple-card-overview">
  <!-- Career Card -->
  <div class="card">
    <div class="card-header">
      <h2>Career</h2>
      <button class="card-link" on:click={() => scrollToSection('experience')}>View Resume →</button>
    </div>
    <p class="card-summary">I'm an expert backend developer with proven technical leadership and system design skills.</p>
    <div class="card-details">
      <ul class="project-list">
        <li>
          <span class="skill-title">API Services</span>
          <span class="project-tech">REST, Ruby on Rails, Express</span>
          <p class="project-desc">Designing and implementing scalable public and private RESTful API services</p>
        </li>
        <li>
          <span class="skill-title">Frontend Development</span>
          <span class="project-tech">React, Next.js</span>
          <p class="project-desc">Building dynamic and responsive user interfaces which interract with complex data models</p>
        </li>
        <li>
          <span class="project-title">Data Processing Pipelines</span>
          <span class="project-tech">Lambda, SNS, SQS</span>
          <p class="project-desc">Building high-throughput event-based data processing pipelines for real-time operations, analytics, and insights.</p>
        </li>
        <li>
          <span class="project-title">Database Optimization</span>
          <span class="project-tech">PostgreSQL, DynamoDB, OpenSearch</span>
          <p class="project-desc">Implementing performance optimizations for database systems to handle high-volume transactions.</p>
        </li>
        <li>
          <span class="project-title">Integrations</span>
          <span class="project-tech">OAuth2.0, Webhooks, Microservices</span>
          <p class="project-desc">Developing deeply stateful integrations with third-party services</p>
        </li>
      </ul>
    </div>
  </div>

  <!-- Projects Card -->
  <div class="card">
    <div class="card-header">
      <h2>Projects</h2>
      <button on:click={() => scrollToSection('projects')} class="card-link">View All Projects →</button>
    </div>
    <p class="card-summary">Select projects highlighting my expertise in scalable backends, data systems, and full-stack application development.</p>
    <div class="card-details">
      <ul class="project-list">
        <li>
          <a href="/case-studies/service-request-platform" class="project-title">Service Request Orchestrator →</a>
          <span class="project-tech">Ruby on Rails with PostgreSQL</span>
          <p class="project-desc">A scalable middleware solution for exposing and orchestrating complex service requests on any platform.</p>
        </li>
        <li>
          <a href="/case-studies/data-highway" class="project-title">Marketplace Data Highway →</a>
          <span class="project-tech">Ruby on Rails with Opensearch</span>
          <p class="project-desc">On-demand data transit solution for real-time marketplace operations, analytics, and reporting.</p>
        </li>
        <li>
          <a href="/case-studies/bookends" class="project-title">Bookends →</a>
          <span class="project-tech">Swift iOS + Go Backend with DynamoDB</span>
          <p class="project-desc">Mobile application for tracking your personal library, facilitating a reading habit, and sharing it all with your friends.</p>
        </li>
      </ul>
    </div>
  </div>

  <!-- Education Card -->
  <div class="card">
    <div class="card-header">
      <h2>Education</h2>
      <a href="/experience/education" class="card-link">View Transcript →</a>
    </div>
    <p class="card-summary">B.S. in Computer Science from Brigham Young University - 3.94 GPA - Cum Laude (2023)</p>
    <div class="card-details">
      <ul class="project-list">
        <li>
          <span class="course-title">Algorithm Design & Analysis</span>
          <span class="project-tech">Advanced CS Theory, Python</span>
          <p class="project-desc">Study of algorithmic paradigms, data structures, and computational complexity theory.</p>
        </li>
        <li>
          <span class="course-title">Data Structures</span>
          <span class="project-tech">C++</span>
          <p class="project-desc">Study of data structures and their implementation in C++.</p>
        </li>
        <li>
          <span class="course-title">Database Modeling Concepts</span>
          <span class="project-tech">SQL, RDBMS, DynamoDB</span>
          <p class="project-desc">Relational database design principles, normalization, and NoSQL use cases.</p>
        </li>
        <li>
          <span class="course-title">Information Retrieval</span>
          <span class="project-tech">Search, NLP, Statistical Learning</span>
          <p class="project-desc">Text processing, indexing strategies, and search algorithm implementation.</p>
        </li>
        <li>
          <span class="course-title">Web Programming</span>
          <span class="project-tech">MongoDB, Express, Vue, Node.js</span>
          <p class="project-desc">Building dynamic web applications with user authentication, authorization, and data storage.</p>
        </li>

      </ul>
    </div>
  </div>

  
</div>

<style>
  .about-me-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 clamp(1rem, 3vw, 2rem);
    width: 100%;
    position: relative;
    z-index: 2;
    margin-top: 4rem;
    margin-bottom: 3rem;
    font-size: 0.8em;
  }

  .about-me {
    font-size: clamp(1.8rem, 6.5vw, 5.5rem);
    font-weight: 700;
    font-family: 'Inter', sans-serif;
    line-height: 1;
    letter-spacing: -0.02em;
    color: var(--text-primary);
    position: relative;
    text-align: left;
    margin: 0;
    text-shadow: 
      0 0 1px rgba(255, 255, 255, 0.2),
      0 0 15px var(--text-glow),
      0 0 25px var(--text-glow);
    word-break: break-word;
    hyphens: auto;
  }

  /* Card layout styles */
  .triple-card-overview {
    display: flex;
    justify-content: space-between;
    gap: 2rem;
    padding: 3rem 2rem;
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    z-index: 10;
    padding-top: 5rem;
  }

  .card {
    flex: 1;
    background: rgba(13, 25, 42, 0.8);
    backdrop-filter: blur(8px);
    border: 1px solid rgba(125, 211, 252, 0.1);
    border-radius: 10px;
    padding: 1.5rem;
    color: var(--text-primary);
    transition: all 0.3s ease;
    display: flex;
    flex-direction: column;
    position: relative;
    overflow: hidden;
    height: 220px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
    z-index: 10;
  }

  .card:nth-child(1) {
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.9), rgba(52, 152, 219, 0.3));
  }

  .card:nth-child(2) {
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.9), rgba(125, 95, 255, 0.3));
  }

  .card:nth-child(3) {
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.9), rgba(26, 188, 156, 0.3));
  }

  .card h2 {
    color: var(--text-accent);
    margin-top: 0;
    margin-bottom: 0.75rem;
    font-size: 1.15rem;
  }

  .card-summary {
    margin-top: 0.8rem;
    color: var(--text-primary);
    margin-bottom: 1rem;
    font-size: 0.8rem;
  }

  .card-details {
    max-height: 0;
    opacity: 0;
    overflow: hidden;
    transition: all 0.3s ease;
  }

  .card-list {
    list-style-type: disc;
    padding-left: 1.25rem;
    margin: 0.5rem 0;
  }

  .card-list li {
    margin-bottom: 0.5rem;
    color: var(--text-primary);
    line-height: 1.4;
  }

  .card-list li::marker {
    color: var(--text-accent);
  }

  .card-link {
    color: var(--text-accent);
    text-decoration: none;
    font-weight: 500;
    transition: all 0.2s ease;
    background: none;
    border: none;
    padding: 0;
    font: inherit;
    cursor: pointer;
    outline: inherit;
  }
  
  .card-link:hover {
    transform: translateX(3px);
  }

  /* Triple card container with hover effects */
  .triple-card-overview:hover .card {
    filter: brightness(0.75);
    transform: scale(0.95);
  }
  
  .triple-card-overview .card:hover {
    filter: brightness(1);
    transform: scale(1.05) translateY(-5px);
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.3);
    z-index: 20;
    height: auto;
    min-height: 220px;
  }

  .card:hover .card-details {
    max-height: none;
    opacity: 1;
    margin-bottom: 1.5rem;
  }

  .card:hover .card-link {
    opacity: 1;
  }

  .card-link:hover {
    transform: translateX(5px);
  }

  /* Project list styling */
  .project-list {
    list-style-type: none;
    padding-left: 0;
    margin: 0.5rem 0;
  }

  .project-list li {
    margin-bottom: 1rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid rgba(125, 211, 252, 0.1);
  }

  .project-list li:last-child {
    border-bottom: none;
    margin-bottom: 0;
  }

  .project-title {
    display: block;
    font-weight: 600;
    font-size: 0.85rem;
    color: var(--text-accent);
    text-decoration: none;
    margin-bottom: 0.2rem;
    transition: all 0.2s ease;
  }

  .project-title:hover {
    color: white;
    transform: translateX(3px);
  }

  .project-tech {
    display: block;
    font-size: 0.7rem;
    color: var(--text-secondary);
    font-style: italic;
    margin-bottom: 0.4rem;
  }

  .project-desc {
    font-size: 0.75rem;
    margin: 0;
    line-height: 1.4;
  }

  /* Ensure responsive behavior for mobile */
  @media (max-width: 768px) {
    .triple-card-overview {
      flex-direction: column;
      padding: 2rem 1rem;
    }

    .triple-card-overview:hover .card {
      filter: brightness(1);
      transform: none;
    }
    
    .card {
      width: 100%;
      margin-bottom: 1.5rem;
    }

    .card:hover {
      transform: none !important;
      min-height: auto;
    }

    .card-details {
      max-height: none;
      opacity: 1;
    }

    .card-link {
      opacity: 1;
    }
  }

  @media (max-width: 480px) {
    .about-me-container {
      padding: 0 1rem;
      margin-top: 2rem;
    }

    .about-me {
      font-size: clamp(2rem, 12vw, 2.5rem);
    }
  }

  /* Add tablet breakpoint for mid-sized devices */
  @media (min-width: 481px) and (max-width: 768px) {
    .about-me {
      font-size: clamp(2.5rem, 10vw, 4rem);
    }
  }

  
  /* JSON Code Block Styles */
  .json-code-container {
    position: absolute;
    top: 0;
    right: 2rem;
    z-index: 30;
    max-width: 500px;
    margin-bottom: 3rem;
  }
  
  .json-code {
    background: rgba(13, 25, 42, 0.8);
    border: 1px solid rgba(125, 211, 252, 0.3);
    border-radius: 8px;
    padding: 1.2rem;
    color: var(--text-primary);
    font-family: 'Fira Code', 'Consolas', monospace;
    font-size: 0.75rem;
    line-height: 1.6;
    white-space: pre-wrap;
    word-break: break-word;
    margin: 0;
    box-shadow: 0 0 20px var(--text-glow);
    overflow-wrap: break-word;
  }
  
  .cursor {
    display: inline-block;
    width: 8px;
    animation: blink 1s infinite;
    color: var(--text-accent);
  }
  
  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }
  
  /* Syntax highlighting colors */
  :global(.json-code .key) {
    color: #f92672;
  }
  
  :global(.json-code .value) {
    color: #e6db74;
  }
  
  :global(.json-code .punctuation) {
    color: #f8f8f2;
  }
  
  :global(.json-code .keyword) {
    color: #66d9ef;
  }
  
  @media (max-width: 768px) {
    .json-code-container {
      position: relative;
      top: 0;
      right: 0;
      margin: 2rem auto 3rem;
      max-width: 100%;
      width: 90%;
    }
    
    .json-code {
      font-size: 0.7rem;
      padding: 0.8rem;
      line-height: 1.4;
    }
    
    .about-me-container {
      margin-bottom: 2rem;
    }
    
    .triple-card-overview {
      padding-top: 3rem;
    }
  }
  
  @media (max-width: 480px) {
    .json-code {
      font-size: 0.7rem;
      padding: 0.4rem;
      overflow-x: auto;
    }
    
    .json-link {
      word-break: break-all;
    }
  }
  
  @keyframes orbit {
    0% {
      transform: rotate(0deg) translateX(40px) rotate(0deg);
    }
    100% {
      transform: rotate(360deg) translateX(40px) rotate(-360deg);
    }
  }
  
  @keyframes rotate {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }
  
  @keyframes pulse-text {
    0% { opacity: 0.7; transform: scale(1); }
    100% { opacity: 1; transform: scale(1.1); }
  }
  
  .cursor.hidden {
    opacity: 0;
  }
  
  .json-link.link-complete {
    text-decoration: underline;
    text-decoration-style: dashed;
    text-decoration-color: rgba(125, 211, 252, 0.5);
    text-underline-offset: 4px;
  }
  
  .json-link.link-complete:hover {
    color: #ffffff;
    text-shadow: 0 0 8px var(--text-glow);
    text-decoration-color: var(--text-accent);
  }
  
  .syntax-complete :global(.keyword) {
    color: #66d9ef; /* Blue for keywords */
    font-weight: 500;
  }
  
  .syntax-complete :global(.punctuation) {
    color: #f8f8f2; /* White for punctuation */
  }
  
  .syntax-complete :global(.property) {
    color: #f92672; /* Magenta for object properties */
    font-weight: 500;
  }

  /* New styles for skill-title and course-title */
  .skill-title, .course-title {
    display: block;
    font-weight: 600;
    font-size: 0.85rem;
    color: var(--text-accent);
    margin-bottom: 0.2rem;
  }

  /* Add these styles to your existing CSS */
  .card-header {
    position: relative;
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 1.2rem;
  }

  .card .card-link {
    position: absolute;
    right: 0;
    opacity: 0;
    transition: all 0.3s ease;
    align-self: baseline;
  }

  .card:hover .card-link {
    opacity: 1;
    transform: translateX(-5px);
  }

  /* For mobile responsiveness */
  @media (max-width: 768px) {
    .card .card-link {
      position: static;
      opacity: 1;
      transform: none;
      margin-top: 1rem;
      display: inline-block;
    }
  }

  .skills-header h2 {
    font-size: 1.5rem;
    color: var(--text-primary);
    margin-bottom: 1rem;
  }

  .skills-header p {
    color: var(--text-secondary);
    max-width: 800px;
    margin: 0 auto 3rem;
    font-size: 0.8rem;
  }
</style> 