<script lang="ts">
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  
  let visible = false;
  
  onMount(() => {
    visible = true;
  });
</script>

<svelte:head>
  <title>Case Study: Service Request Middleware Platform | Chad Holmes</title>
  <meta name="description" content="A case study on building a scalable service request platform at Pattern Inc." />
</svelte:head>

<div class="case-study-container">
  {#if visible}
    <header class="case-study-header" transition:fade={{ delay: 100, duration: 300 }}>
      <h1>Service Request Middleware Platform</h1>
      <h2>Building a Scalable System for Enterprise Service Orchestration</h2>
    </header>

    <!-- Bento Cluster for Overview -->
    <div class="bento-cluster" transition:fade={{ delay: 200, duration: 300 }}>
      <div class="bento-item main">
        <h3>Project Snapshot</h3>
        <p>A robust middleware platform that connects client applications with service provider systems, enabling programmatic service requests with complete contextual information.</p>
      </div>
      
      <div class="bento-item metrics1">
        <span class="bento-icon">⏱️</span>
        <h4>40% Faster</h4>
        <p>Reduction in service delivery time</p>
      </div>
      
      <div class="bento-item metrics2">
        <span class="bento-icon">🚀</span>
        <h4>100% Automation</h4>
        <p>Complete elimination of manual request placement</p>
      </div>
      
      <div class="bento-item metrics3">
        <span class="bento-icon">👥</span>
        <h4>Leadership</h4>
        <p>Led team of 4 engineers in project development</p>
      </div>
      
      <div class="bento-item tech1">
        <span class="bento-icon">🔧</span>
        <h4>Ruby on Rails</h4>
        <p>RESTful API service with PostgreSQL database</p>
      </div>
      
      <div class="bento-item tech2">
        <span class="bento-icon">☁️</span>
        <h4>AWS Infrastructure</h4>
        <p>SNS/SQS event processing</p>
      </div>
    </div>

    <div class="case-study-content" transition:fade={{ delay: 200, duration: 400 }}>
      <section class="overview-section" transition:fly={{ y: 20, delay: 300, duration: 400 }}>
        <h3><span class="section-icon">🔍</span> Overview</h3>
        <div class="section-content">
          <p>Led the design and implementation of a service request middleware platform at Pattern, creating a robust system that connects client applications with service provider platforms. This headless integration hub serves two distinct user groups—requesters and service providers—while maintaining separation of concerns between their respective systems. The platform's API-first approach allows client systems to programmatically initiate service requests with full contextual information, eliminating manual form entry for requesters while providing comprehensive task context for service providers, all without disrupting existing workflows.</p>
          
          <div class="original-metrics">
            <div class="metric">
              <span class="metric-value">40%</span>
              <span class="metric-label">Reduction in Service Delivery Time</span>
            </div>
            <div class="metric">
              <span class="metric-value">100%</span>
              <span class="metric-label">Reduction in Service Request Placement Time</span>
            </div>
            <div class="metric">
              <span class="metric-value">4</span>
              <span class="metric-label">Engineers led in project development</span>
            </div>
          </div>
        </div>
      </section>

      <section class="challenge-section" transition:fly={{ y: 20, delay: 400, duration: 400 }}>
        <h3><span class="section-icon">⚠️</span> Challenge</h3>
        <div class="section-content">
          <p>Pattern faced significant inefficiencies with service requests flowing between different platforms, with both technical teams and business users experiencing major friction:</p>
          
          <ul class="challenge-list">
            <li>Brand managers and SAAS customers were spending excessive time on troubleshooting rather than core business tasks</li>
            <li>Marketplace integrations required custom development for each new service type</li>
            <li>No standardized way to track service requests across different provider platforms</li>
            <li>Service requesters wasted valuable time manually copying context from source platforms into separate request forms</li>
            <li>Complex security requirements for cross-platform authentication and data validation</li>
          </ul>
        </div>
      </section>

      <section class="solution-section" transition:fly={{ y: 20, delay: 500, duration: 400 }}>
        <h3><span class="section-icon">💡</span> Solution Architecture</h3>
        
        <div class="solution-columns">
          <div class="solution-column">
            <h4>Application Design</h4>
            <ul class="feature-list">
              <li>
                <strong>Decoupled Service Architecture</strong>
                <p>Designed a middleware API that separates client platforms from provider platforms, allowing them to evolve independently</p>
              </li>
              <li>
                <strong>Versioned Request Type System</strong>
                <p>Implemented a flexible request type framework that supports versioning and parameter validation</p>
              </li>
              <li>
                <strong>Webhook Integration</strong>
                <p>Created bidirectional communication with task management platforms using secure webhook implementation</p>
              </li>
              <li>
                <strong>Event-Driven Notifications</strong>
                <p>Built an SNS-based notification system for real-time updates to client platforms</p>
              </li>
            </ul>
          </div>

          <div class="solution-column">
            <h4>Technical Innovations</h4>
            <ul class="feature-list">
              <li>
                <strong>JSON Schema Parameter Dependencies</strong>
                <p>Engineered a sophisticated validation system using JSON Schema to handle complex parameter relationships</p>
              </li>
              <li>
                <strong>Custom Field Transformation Engine</strong>
                <p>Developed a mapping system to transform fields between different platforms with support for complex data types</p>
              </li>
              <li>
                <strong>Secure Webhook Authentication</strong>
                <p>Implemented nonce verification and trusted source validation for webhook security</p>
              </li>
              <li>
                <strong>Contextual Request Automation</strong>
                <p>Designed an API-driven approach allowing client systems to generate service requests with pre-populated context, eliminating manual form entry and reducing friction for requesters</p>
              </li>
            </ul>
          </div>
        </div>

        <div class="infrastructure-section">
          <h4>Infrastructure Implementation</h4>
          <div class="infra-grid">
            <div class="infra-item">
              <div class="infra-icon">🐳</div>
              <h5>Containerized Deployment</h5>
              <p>Utilized Docker and ECS for consistent deployment across environments</p>
            </div>
            <div class="infra-item">
              <div class="infra-icon">🗄️</div>
              <h5>Database Strategy</h5>
              <p>Implemented PostgreSQL with proper indexing for efficient request tracking</p>
            </div>
            <div class="infra-item">
              <div class="infra-icon">⚡</div>
              <h5>Caching Layer</h5>
              <p>Deployed Redis for improved performance and background job processing</p>
            </div>
            <div class="infra-item">
              <div class="infra-icon">📨</div>
              <h5>Messaging Architecture</h5>
              <p>Configured SQS for reliable credential updates and service communication</p>
            </div>
          </div>
        </div>
      </section>

      <section class="user-experience-section" transition:fly={{ y: 20, delay: 550, duration: 400 }}>
        <h3><span class="section-icon">👥</span> Dual-User Experience Design</h3>
        <div class="section-content">
          <p>A key innovation in this platform was designing for two distinct user groups with different needs:</p>
          
          <div class="user-experience-container">
            <div class="user-experience-columns">
              <div class="user-experience-column">
                <div class="user-icon">🧑‍💼</div>
                <h4>For Requesters</h4>
                <ul class="benefit-list">
                  <li>Eliminated duplicate data entry through contextual automation</li>
                  <li>Integrated request creation directly within existing workflows</li>
                  <li>Provided real-time status updates without platform switching</li>
                  <li>Reduced request creation time from minutes to seconds</li>
                </ul>
              </div>
              
              <div class="user-experience-column">
                <div class="user-icon">🧑‍🔧</div>
                <h4>For Service Providers</h4>
                <ul class="benefit-list">
                  <li>Delivered comprehensive context with every request</li>
                  <li>Standardized request formats across different source platforms</li>
                  <li>Surfaced requests in providers' preferred task management tools</li>
                  <li>Automated routine status updates back to requesters</li>
                </ul>
              </div>
            </div>
          
            <p class="design-philosophy">This dual-focus approach ensured both user groups experienced significant workflow improvements without being forced to adopt new tools or interfaces—a critical factor in achieving rapid adoption across the organization.</p>
          </div>
        </div>
      </section>

      <section class="tech-section" transition:fly={{ y: 20, delay: 600, duration: 400 }}>
        <h3><span class="section-icon">⚙️</span> Key Technologies</h3>
        <div class="section-content">
          <div class="tech-tags">
            <span class="tech-tag">Ruby on Rails</span>
            <span class="tech-tag">Sidekiq</span>
            <span class="tech-tag">Redis</span>
            <span class="tech-tag">PostgreSQL</span> 
            <span class="tech-tag">AWS ECS</span>
            <span class="tech-tag">AWS RDS</span>
            <span class="tech-tag">ElastiCache</span>
            <span class="tech-tag">SQS</span>
            <span class="tech-tag">SNS</span>
            <span class="tech-tag">Terraform</span>
            <span class="tech-tag">Docker</span>
            <span class="tech-tag">Swagger</span>
           
          </div>
        </div>
      </section>

      <section class="results-section" transition:fly={{ y: 20, delay: 700, duration: 400 }}>
        <h3><span class="section-icon">🏆</span> Results</h3>
        <div class="section-content">
          <div class="results-grid">
            <div class="result-card">
              <div class="result-icon">⏱️</div>
              <p>Reduced troubleshooting time by 40% for brand managers and customers</p>
            </div>
            <div class="result-card">
              <div class="result-icon">🚀</div>
              <p>Enabled rapid deployment of new service types without custom development</p>
            </div>
            <div class="result-card">
              <div class="result-icon">📊</div>
              <p>Provided real-time status updates and history tracking for all service requests</p>
            </div>
            <div class="result-card">
              <div class="result-icon">🔐</div>
              <p>Established a secure, auditable system for sensitive request handling</p>
            </div>
            <div class="result-card">
              <div class="result-icon">🔄</div>
              <p>Eliminated 100% of manual form entry through contextual automation, improving requester experience</p>
            </div>
            <div class="result-card">
              <div class="result-icon">🔀</div>
              <p>Maintained workflow continuity for both requesters and providers by integrating with their existing tools</p>
            </div>
          </div>
        </div>
      </section>

      <section class="leadership-section" transition:fly={{ y: 20, delay: 800, duration: 400 }}>
        <h3><span class="section-icon">👨‍💻</span> Leadership Impact</h3>
        <div class="section-content">
          <ul class="leadership-list">
            <li>Designed and documented a comprehensive system architecture that has become a model for other integration projects</li>
            <li>Created detailed onboarding documentation and API guides to ensure developer productivity</li>
            <li>Championed a dual-user experience approach that balanced the needs of both requesters and service providers</li>
            <li>Implemented Agile practices including sprint structure, refinement processes, and code review standards</li>
            <li>Mentored team members on API design, security best practices, and cloud infrastructure</li>
          </ul>
        </div>
      </section>
    </div>
  {/if}
</div>

<style>
  .case-study-container {
    margin: 0 auto;
    max-width: 1000px;
    padding: 2rem;
    color: var(--text-primary);
    padding-top: 6rem;
  }

  .case-study-header {
    margin-bottom: 3rem;
    text-align: center;
  }

  .case-study-header h1 {
    font-size: clamp(2.4rem, 5vw, 3.5rem);
    font-weight: 700;
    margin-bottom: 1rem;
    color: var(--text-primary);
    line-height: 1.2;
    text-shadow: 0 0 10px var(--text-glow);
  }

  .case-study-header h2 {
    font-size: clamp(1.2rem, 3vw, 1.5rem);
    color: var(--text-secondary);
    font-weight: 400;
    margin-top: 0;
  }

  /* Bento Cluster Styles */
  .bento-cluster {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: auto auto auto;
    grid-template-areas:
      "main main main main"
      "metrics1 metrics1 metrics2 metrics3"
      "tech1 tech1 tech2 tech2";
    gap: 1rem;
    margin-bottom: 4rem;
  }

  .bento-item {
    background: rgba(13, 25, 42, 0.8);
    border: 1px solid rgba(125, 211, 252, 0.2);
    border-radius: 12px;
    padding: 1.5rem;
    transition: all 0.3s ease;
    display: flex;
    flex-direction: column;
  }

  .bento-item:hover {
    transform: translateY(-3px);
    border-color: rgba(125, 211, 252, 0.4);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  }

  .bento-item.main {
    grid-area: main;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.9), rgba(52, 152, 219, 0.3));
  }

  .bento-item.metrics1 {
    grid-area: metrics1;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(46, 204, 113, 0.2));
  }

  .bento-item.metrics2 {
    grid-area: metrics2;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(52, 152, 219, 0.2));
  }

  .bento-item.metrics3 {
    grid-area: metrics3;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(155, 89, 182, 0.2));
  }

  .bento-item.tech1 {
    grid-area: tech1;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(230, 126, 34, 0.2));
  }

  .bento-item.tech2 {
    grid-area: tech2;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(41, 128, 185, 0.2));
  }

  .bento-icon {
    font-size: 1.75rem;
    margin-bottom: 0.75rem;
  }

  .bento-item h3 {
    font-size: 1.5rem;
    margin-top: 0;
    margin-bottom: 0.75rem;
    color: var(--text-accent);
  }

  .bento-item h4 {
    font-size: 1.25rem;
    margin-top: 0;
    margin-bottom: 0.5rem;
    color: var(--text-accent);
  }

  .bento-item p {
    margin: 0;
    line-height: 1.5;
    color: var(--text-secondary);
  }

  /* Responsive Bento Grid */
  @media (max-width: 768px) {
    .bento-cluster {
      grid-template-columns: 1fr 1fr;
      grid-template-areas:
        "main main"
        "metrics1 metrics2"
        "metrics3 tech1"
        "tech2 tech2";
    }
  }

  @media (max-width: 480px) {
    .bento-cluster {
      grid-template-columns: 1fr;
      grid-template-areas:
        "main"
        "metrics1"
        "metrics2"
        "metrics3"
        "tech1"
        "tech2";
    }
  }

  /* Content Section Styles */
  .case-study-content section {
    margin-bottom: 4rem;
  }

  .case-study-content h3 {
    font-size: 1.8rem;
    margin-bottom: 1.5rem;
    color: var(--text-primary);
    display: flex;
    align-items: center;
    gap: 0.5rem;
    border-bottom: 1px solid rgba(125, 211, 252, 0.2);
    padding-bottom: 0.5rem;
  }

  .section-icon {
    font-size: 1.5rem;
  }

  .section-content {
    font-size: 1.1rem;
    line-height: 1.6;
    color: var(--text-secondary);
  }

  .section-content h4 {
    font-size: 1.4rem;
    margin-top: 2rem;
    margin-bottom: 1rem;
    color: var(--text-accent);
  }

  /* Original metrics styling (avoiding conflict with bento) */
  .original-metrics {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    gap: 1.5rem;
    margin: 2rem 0;
    text-align: center;
  }

  .original-metrics .metric {
    text-align: center;
    padding: 1.5rem;
    background: rgba(0, 0, 0, 0.15);
    border-radius: 8px;
    min-width: 200px;
    border: 1px solid rgba(125, 211, 252, 0.15);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: all 0.2s ease;
  }

  .original-metrics .metric:hover {
    transform: translateY(-5px);
    border-color: rgba(125, 211, 252, 0.3);
  }

  .original-metrics .metric-value {
    display: block;
    font-size: 2.5rem;
    font-weight: 700;
    color: var(--text-accent);
    margin-bottom: 0.5rem;
  }

  .original-metrics .metric-label {
    font-size: 1.1rem;
    color: var(--text-secondary);
  }

  @media (max-width: 480px) {
    .original-metrics {
      flex-direction: column;
      gap: 1rem;
    }

    .original-metrics .metric {
      width: 100%;
      min-width: unset;
    }
  }

  /* Remove the key-metrics display:none style */
  .key-metrics {
    display: none; /* Hiding this class since we're using original-metrics */
  }

  /* Update Feature List Styling to match bookends */
  .feature-list {
    list-style: none;
    padding: 0;
  }

  .feature-list li {
    margin-bottom: 1.5rem;
    background: rgba(13, 25, 42, 0.5);
    border-radius: 8px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
    transition: all 0.3s ease;
  }

  .feature-list li:hover {
    transform: translateY(-3px);
    border-color: rgba(125, 211, 252, 0.3);
  }

  .feature-list li strong {
    display: block;
    font-size: 1.1rem;
    margin-bottom: 0.5rem;
    color: var(--text-accent);
  }

  /* Solution Columns Update */
  .solution-columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    margin-bottom: 2rem;
  }

  @media (max-width: 900px) {
    .solution-columns {
      grid-template-columns: 1fr;
    }
  }

  /* User Experience Columns Update */
  .user-experience-container {
    background: rgba(13, 25, 42, 0.3);
    border-radius: 12px;
    padding: 2rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
    margin: 2rem 0;
    transition: all 0.3s ease;
  }

  .user-experience-container:hover {
    border-color: rgba(125, 211, 252, 0.3);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  }

  .user-experience-columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    margin-bottom: 1.5rem;
  }

  .user-experience-column {
    background: rgba(13, 25, 42, 0.5);
    border-radius: 8px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
    text-align: center;
    transition: all 0.3s ease;
  }

  .user-experience-column:hover {
    transform: translateY(-5px);
    border-color: rgba(125, 211, 252, 0.3);
  }

  .user-icon {
    font-size: 2.5rem;
    margin-bottom: 1rem;
  }

  .benefit-list {
    list-style: none;
    padding: 0;
    text-align: left;
    margin: 1.5rem 0 0;
  }

  .benefit-list li {
    position: relative;
    padding-left: 1.5rem;
    margin-bottom: 0.75rem;
    color: var(--text-secondary);
    line-height: 1.6;
  }

  .benefit-list li::before {
    content: "✓";
    position: absolute;
    left: 0;
    color: var(--text-accent);
  }

  .design-philosophy {
    font-style: italic;
    margin: 0;
    padding-top: 1.5rem;
    border-top: 1px solid rgba(125, 211, 252, 0.15);
    text-align: center;
  }

  @media (max-width: 900px) {
    .user-experience-columns {
      grid-template-columns: 1fr;
    }
  }

  .case-study-content .challenge-list {
    list-style: none;
    padding: 0;
    margin: 1.5rem 0;
  }

  .case-study-content .challenge-list li {
    position: relative;
    padding-left: 1.5rem;
    margin-bottom: 1rem;
    color: var(--text-secondary);
    line-height: 1.6;
  }

  .case-study-content .challenge-list li::before {
    content: "⚠️";
    position: absolute;
    left: 0;
    font-size: 0.9rem;
  }

  .case-study-content .infrastructure-section {
    margin-top: 3rem;
  }

  .case-study-content .infra-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
    margin: 2rem 0;
  }

  .case-study-content .infra-item {
    background: rgba(13, 25, 42, 0.5);
    border-radius: 8px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
    text-align: center;
    transition: all 0.3s ease;
  }

  .case-study-content .infra-item:hover {
    transform: translateY(-3px);
    border-color: rgba(125, 211, 252, 0.3);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  }

  .case-study-content .infra-icon {
    font-size: 2rem;
    margin-bottom: 0.5rem;
  }

  .case-study-content .tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
  }

  .case-study-content .tech-tag {
    background: rgba(125, 211, 252, 0.15);
    color: var(--text-accent);
    padding: 0.5rem 1rem;
    border-radius: 4px;
    font-size: 1rem;
    border: 1px solid rgba(125, 211, 252, 0.3);
    transition: all 0.2s ease;
  }

  .case-study-content .tech-tag:hover {
    background: rgba(125, 211, 252, 0.25);
    transform: translateY(-2px);
  }

  .case-study-content .results-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
  }

  .case-study-content .result-card {
    background: rgba(13, 25, 42, 0.5);
    border-radius: 8px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
    text-align: center;
    transition: all 0.3s ease;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .case-study-content .result-card:hover {
    transform: translateY(-3px);
    border-color: rgba(125, 211, 252, 0.3);
  }

  .case-study-content .result-icon {
    font-size: 2rem;
    margin-bottom: 1rem;
  }

  .case-study-content .leadership-list {
    list-style: none;
    padding: 0;
  }

  .case-study-content .leadership-list li {
    position: relative;
    padding-left: 1.5rem;
    margin-bottom: 1rem;
    color: var(--text-secondary);
    line-height: 1.6;
  }

  .case-study-content .leadership-list li::before {
    content: "▹";
    position: absolute;
    left: 0;
    color: var(--text-accent);
  }

  .case-study-content .design-philosophy {
    font-style: italic;
    margin-top: 1.5rem;
    padding-top: 1.5rem;
    border-top: 1px solid rgba(125, 211, 252, 0.15);
  }

  /* Clean up other prefixed classes */
  .challenge-list {
    list-style: none;
    padding: 0;
    margin: 1.5rem 0;
  }

  .challenge-list li {
    position: relative;
    padding-left: 1.5rem;
    margin-bottom: 1rem;
    color: var(--text-secondary);
    line-height: 1.6;
  }

  .challenge-list li::before {
    content: "⚠️";
    position: absolute;
    left: 0;
    font-size: 0.9rem;
  }

  .infrastructure-section {
    margin-top: 3rem;
  }

  .infra-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
    margin: 2rem 0;
  }

  .infra-item {
    background: rgba(13, 25, 42, 0.5);
    border-radius: 8px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
    text-align: center;
    transition: all 0.3s ease;
  }

  .infra-item:hover {
    transform: translateY(-3px);
    border-color: rgba(125, 211, 252, 0.3);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  }

  .infra-icon {
    font-size: 2rem;
    margin-bottom: 0.5rem;
  }

  .tech-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
  }

  .tech-tag {
    background: rgba(125, 211, 252, 0.15);
    color: var(--text-accent);
    padding: 0.5rem 1rem;
    border-radius: 4px;
    font-size: 1rem;
    border: 1px solid rgba(125, 211, 252, 0.3);
    transition: all 0.2s ease;
  }

  .tech-tag:hover {
    background: rgba(125, 211, 252, 0.25);
    transform: translateY(-2px);
  }

  .results-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
  }

  .result-card {
    background: rgba(13, 25, 42, 0.5);
    border-radius: 8px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
    text-align: center;
    transition: all 0.3s ease;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .result-card:hover {
    transform: translateY(-3px);
    border-color: rgba(125, 211, 252, 0.3);
  }

  .result-icon {
    font-size: 2rem;
    margin-bottom: 1rem;
  }

  .leadership-list {
    list-style: none;
    padding: 0;
  }

  .leadership-list li {
    position: relative;
    padding-left: 1.5rem;
    margin-bottom: 1rem;
    color: var(--text-secondary);
    line-height: 1.6;
  }

  .leadership-list li::before {
    content: "▹";
    position: absolute;
    left: 0;
    color: var(--text-accent);
  }

  .design-philosophy {
    font-style: italic;
    margin-top: 1.5rem;
    padding-top: 1.5rem;
    border-top: 1px solid rgba(125, 211, 252, 0.15);
  }

  /* Additional responsive styles */
  @media (max-width: 768px) {
    .infra-grid,
    .results-grid {
      grid-template-columns: 1fr 1fr;
    }
  }

  @media (max-width: 480px) {
    .infra-grid,
    .results-grid {
      grid-template-columns: 1fr;
    }
  }
</style> 