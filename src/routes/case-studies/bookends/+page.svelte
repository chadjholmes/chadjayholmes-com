<script lang="ts">
  import { onMount } from 'svelte';
  import { fade, fly } from 'svelte/transition';
  
  let visible = false;
  
  onMount(() => {
    visible = true;
  });
</script>

<svelte:head>
  <title>Project: Bookends iOS App | Chad Holmes</title>
  <meta name="description" content="A case study on building a social reading platform with iOS SwiftData and cloud synchronization." />
</svelte:head>

<div class="case-study-container">
  {#if visible}
    <header class="case-study-header" transition:fade={{ delay: 100, duration: 300 }}>
      <h1>Bookends iOS: A Development Journey</h1>
      <h2>Building a Comprehensive Social Reading Platform</h2>
    </header>

    <!-- Bento Cluster for Overview -->
    <div class="bento-cluster" transition:fade={{ delay: 200, duration: 300 }}>
      <div class="bento-item main">
        <h3>Project Snapshot</h3>
        <p>Bookends evolved from a simple reading tracker into a comprehensive social reading platform with cloud synchronization and rich interactive features.</p>
      </div>
      
      <div class="bento-item ios">
        <span class="bento-icon">📱</span>
        <h4>iOS Platform</h4>
        <p>Built with Swift, SwiftUI, and SwiftData for persistence</p>
      </div>
      
      <div class="bento-item sync">
        <span class="bento-icon">🔄</span>
        <h4>Cloud Sync</h4>
        <p>DynamoDB-based synchronization with offline support</p>
      </div>
      
      <div class="bento-item social">
        <span class="bento-icon">👥</span>
        <h4>Social Features</h4>
        <p>User profiles, friend connections, and activity feed</p>
      </div>
      
      <div class="bento-item analytics">
        <span class="bento-icon">📊</span>
        <h4>Reading Analytics</h4>
        <p>Sophisticated tracking and visualization of reading habits</p>
      </div>
      
      <div class="bento-item users">
        <span class="bento-icon">📚</span>
        <h4>100+ Beta Users</h4>
        <p>Active user base providing regular feedback</p>
      </div>
    </div>

    <div class="case-study-content" transition:fade={{ delay: 200, duration: 400 }}>
      <!-- Project Overview Section -->
      <section class="overview-section" transition:fly={{ y: 20, delay: 300, duration: 400 }}>
        <h3><span class="section-icon">🔍</span> Project Overview</h3>
        <div class="section-content">
          <p>Bookends is an iOS application that evolved from a simple reading tracker into a comprehensive social reading platform with cloud synchronization and rich interactive features. This case study examines the technical challenges encountered during development and the solutions implemented to overcome them.</p>
        </div>
      </section>

      <!-- Architecture & Design Decisions Section -->
      <section class="architecture-section" transition:fly={{ y: 20, delay: 400, duration: 400 }}>
        <h3><span class="section-icon">🏗️</span> Architecture & Design Decisions</h3>
        
        <div class="section-content">
          <h4>SwiftData Implementation</h4>
          <p>One of the core design decisions was to leverage SwiftData for the app's data model. This decision provided significant benefits for state management and persistence while reducing boilerplate code.</p>

          <p>
            SwiftData is a new way to store data in Swift. It is a lightweight, type-safe, and performant persistence layer that is designed to work seamlessly with SwiftUI. However it is new, and not yet fully mature, so there are some tradeoffs to consider. Community support is still growing, and the documentation is not yet as comprehensive as some other persistence frameworks. However I was able to work through the learning curve and implement a robust data model that met the needs of the app.
          </p>
      </section>
      
      <!-- More sections will be added in subsequent edits -->
      
      <!-- Front-end Complexity Section -->
      <section class="frontend-section" transition:fly={{ y: 20, delay: 500, duration: 400 }}>
        <h3><span class="section-icon">🎨</span> Front-end Complexity</h3>
        
        <div class="section-content">
          <h4>Custom UI Component System</h4>
          
          <div class="challenge-solution">
            <div class="challenge">
              <h5>Challenge</h5>
              <p>Creating a visually distinctive app with consistent UI elements while maintaining code reusability.</p>
            </div>
            
            <div class="solution">
              <h5>Solution</h5>
              <p>Developed a system of custom components that could be reused throughout the app, ensuring visual consistency and code efficiency.</p>
            </div>
          </div>
          
          <p>This component system enabled:</p>
          <ul class="feature-list">
            <li>
              <strong>Consistent Visual Styling</strong>
              <p>Maintained a unified look and feel across the app</p>
            </li>
            <li>
              <strong>Customization Options</strong>
              <p>Provided users with flexible personalization choices</p>
            </li>
            <li>
              <strong>Development Efficiency</strong>
              <p>Reusable code components that reduced development time and bugs</p>
            </li>
          </ul>
          
          <h4>Live Activities Integration</h4>
          
          <div class="challenge-solution">
            <div class="challenge">
              <h5>Challenge</h5>
              <p>Providing real-time reading progress information outside the app.</p>
            </div>
            
            <div class="solution">
              <h5>Solution</h5>
              <p>Implemented iOS Live Activities to display reading session information on the lock screen.</p>
            </div>
          </div>
          
          <div class="technical-achievement">
            <h5>Technical Achievement</h5>
            <p>This implementation required:</p>
            <ul>
              <li>Building custom ActivityKit extensions</li>
              <li>Managing app state across extension boundaries</li>
              <li>Ensuring battery-efficient updates</li>
            </ul>
          </div>
        </div>
      </section>

      <!-- Technical Challenges Section -->
      <section class="technical-section" transition:fly={{ y: 20, delay: 600, duration: 400 }}>
        <h3><span class="section-icon">🔧</span> Solving Data Problems</h3>
        
        <div class="section-content">
          <h4>Cloud Synchronization Architecture</h4>
          
          <div class="challenge-solution">
            <div class="challenge">
              <h5>Challenge</h5>
              <p>Ensuring user data consistency across devices while allowing offline functionality.</p>
            </div>
            
            <div class="solution">
              <h5>Solution</h5>
              <p>Implemented a sophisticated synchronization system using DynamoDB and local caching.</p>
            </div>
          </div>
          
          <div class="sync-description">
            <h5>Custom Data Sync Service Architecture</h5>
            <p>The DataSyncService was architected as a robust middleware layer between the app's local SwiftData storage and the cloud DynamoDB backend. Key components include:</p>
            
            <div class="feature-cards">
              <div class="feature-card">
                <h6>Multi-Observer Pattern</h6>
                <p>The service implements multiple observers that monitor authentication state, network connectivity, offline mode preferences, and model changes. When any of these states change, the appropriate sync actions are triggered automatically.</p>
              </div>
              
              <div class="feature-card">
                <h6>Intelligent Change Detection</h6>
                <p>Rather than syncing entire datasets, the service uses a sophisticated change detection system that identifies only modified properties to minimize data transfer and reduce battery consumption.</p>
              </div>
              
              <div class="feature-card">
                <h6>Conflict Resolution Strategy</h6>
                <p>When conflicting changes occur on multiple devices, the system uses a deterministic resolution approach based on timestamps, modification context, and change priority. This ensures data consistency while preserving user intentions across devices.</p>
              </div>
              
              <div class="feature-card">
                <h6>Offline Operation Queue</h6>
                <p>Changes made while offline are stored in a persistent operation queue using a combination of SwiftData and UserDefaults. When connectivity is restored, these operations are processed in order with proper transaction handling to ensure atomic updates.</p>
              </div>
            </div>
          </div>
          
          <h4>Goodreads Import System</h4>
          
          <div class="challenge-solution">
            <div class="challenge">
              <h5>Challenge</h5>
              <p>Importing potentially thousands of books from external sources without affecting app performance.</p>
            </div>
            
            <div class="solution">
              <h5>Solution</h5>
              <p>Built a scalable import system that processes data in chunks.</p>
            </div>
          </div>

          <div class="technical-achievement">
            <h5>Technical Achievement</h5>
            <ul>
              <li>Implemented background processing using Swift concurrency</li>
              <li>Created a progress tracking system for long-running imports</li>
              <li>Developed data transformation utilities to normalize external data</li>
            </ul>
          </div>
        </div>
      </section>

      <!-- Social Feed Performance Section -->
      <section class="performance-section" transition:fly={{ y: 20, delay: 700, duration: 400 }}>
        <h3><span class="section-icon">⚡</span> Social Feed Performance Optimization</h3>
        
        <div class="section-content">
          <div class="challenge-solution">
            <div class="challenge">
              <h5>Challenge</h5>
              <p>As the social features grew, the activity feed performance degraded with increasing friend connections.</p>
            </div>
            
            <div class="solution">
              <h5>Solution</h5>
              <p>Redesigned the data structure and implemented feed flattening.</p>
            </div>
          </div>

          <div class="performance-metrics">
            <div class="metric">
              <span class="metric-value">80%</span>
              <span class="metric-label">Reduction in Feed Loading Time</span>
            </div>
            <div class="metric">
              <span class="metric-value">↓</span>
              <span class="metric-label">Decreased Memory Usage</span>
            </div>
            <div class="metric">
              <span class="metric-value">∞</span>
              <span class="metric-label">Pagination for Active Feeds</span>
            </div>
          </div>
        </div>
      </section>
      
      <!-- Evolution and Iteration Section -->
      <section class="evolution-section" transition:fly={{ y: 20, delay: 800, duration: 400 }}>
        <h3><span class="section-icon">📈</span> Evolution and Iteration</h3>
        
        <div class="section-content">
          <p>The app demonstrates a clear pattern of iterative development, with each phase building on previous work:</p>
          
          <div class="timeline">
            <div class="timeline-item">
              <div class="timeline-marker">1</div>
              <div class="timeline-content">
                <h4>Core Functionality</h4>
                <p>Basic reading tracking with local storage</p>
              </div>
            </div>
            
            <div class="timeline-item">
              <div class="timeline-marker">2</div>
              <div class="timeline-content">
                <h4>Enhanced UX</h4>
                <p>Animations and visual improvements</p>
              </div>
            </div>
            
            <div class="timeline-item">
              <div class="timeline-marker">3</div>
              <div class="timeline-content">
                <h4>iOS Native Extensions</h4>
                <p>Live Activities and widgets</p>
              </div>
            </div>
            
            <div class="timeline-item">
              <div class="timeline-marker">4</div>
              <div class="timeline-content">
                <h4>Social Features</h4>
                <p>Profiles and friend connections</p>
              </div>
            </div>
            
            <div class="timeline-item">
              <div class="timeline-marker">5</div>
              <div class="timeline-content">
                <h4>Cloud Integration</h4>
                <p>Synchronization and backup</p>
              </div>
            </div>
            
            <div class="timeline-item">
              <div class="timeline-marker">6</div>
              <div class="timeline-content">
                <h4>Performance Optimization</h4>
                <p>Feed flattening and response time improvements</p>
              </div>
            </div>
          </div>
        </div>
      </section>
      
      <!-- Technical Skills Section -->
      <section class="skills-section" transition:fly={{ y: 20, delay: 900, duration: 400 }}>
        <h3><span class="section-icon">🧠</span> Technical Skills Demonstrated</h3>
        
        <div class="section-content">
          <p>This project showcases expertise in:</p>
          
          <div class="skills-grid">
            <div class="skill-card">
              <span class="skill-icon">📱</span>
              <h4>Swift and SwiftUI</h4>
              <p>Modern iOS development with Swift 5.9+ and SwiftUI 5</p>
            </div>
            
            <div class="skill-card">
              <span class="skill-icon">💾</span>
              <h4>Data Modeling</h4>
              <p>Complex persistence with SwiftData and relationships</p>
            </div>
            
            <div class="skill-card">
              <span class="skill-icon">☁️</span>
              <h4>Cloud Integration</h4>
              <p>AWS services including DynamoDB, Lambda, and S3</p>
            </div>
            
            <div class="skill-card">
              <span class="skill-icon">🎨</span>
              <h4>Complex UI</h4>
              <p>Advanced UI with animations and custom controls</p>
            </div>
            
            <div class="skill-card">
              <span class="skill-icon">⚡</span>
              <h4>Performance</h4>
              <p>Optimization for memory, CPU, and battery usage</p>
            </div>
            
            <div class="skill-card">
              <span class="skill-icon">📊</span>
              <h4>Platform Features</h4>
              <p>Integration with iOS-specific capabilities</p>
            </div>
            
            <div class="skill-card">
              <span class="skill-icon">🧵</span>
              <h4>Concurrency</h4>
              <p>Swift concurrency with async/await and background tasks</p>
            </div>
            
            <div class="skill-card">
              <span class="skill-icon">👥</span>
              <h4>Social Integration</h4>
              <p>Friend networks and activity feeds</p>
            </div>
          </div>
        </div>
      </section>
      
      <!-- Conclusion Section -->
      <section class="conclusion-section" transition:fly={{ y: 20, delay: 1000, duration: 400 }}>
        <h3><span class="section-icon">🏁</span> Conclusion</h3>
        
        <div class="section-content">
          <p>The Bookends iOS app represents a sophisticated application that tackles numerous technical challenges while maintaining a focus on user experience. The evolution from a simple reading tracker to a full-featured social platform demonstrates the ability to architect, implement, and optimize complex mobile software systems.</p>
          
          <p>The project highlights not just technical proficiency but the capacity to solve real user problems through thoughtful design decisions and innovative technical solutions.</p>
          
          <div class="final-status">
            <h4>Current Status</h4>
            <p>With over 100 active beta users already enjoying the platform and providing regular feedback, Bookends is now being prepared for a full App Store launch. This real-world validation demonstrates both the app's market potential and the robustness of its technical implementation.</p>
          </div>
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
      "ios ios sync social"
      "analytics analytics users users";
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
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.9), rgba(26, 188, 156, 0.2));
  }

  .bento-item.ios {
    grid-area: ios;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(88, 86, 214, 0.2));
  }

  .bento-item.sync {
    grid-area: sync;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(0, 122, 255, 0.2));
  }

  .bento-item.social {
    grid-area: social;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(255, 45, 85, 0.2));
  }

  .bento-item.analytics {
    grid-area: analytics;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(52, 199, 89, 0.2));
  }

  .bento-item.users {
    grid-area: users;
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(255, 149, 0, 0.2));
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
        "ios sync"
        "social analytics"
        "users users";
    }
  }

  @media (max-width: 480px) {
    .bento-cluster {
      grid-template-columns: 1fr;
      grid-template-areas:
        "main"
        "ios"
        "sync"
        "social"
        "analytics"
        "users";
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

  .section-content h5 {
    font-size: 1.1rem;
    margin-top: 1.5rem;
    margin-bottom: 0.5rem;
    color: var(--text-accent);
  }

  .section-content p {
    margin-bottom: 1.2rem;
  }

  .section-content ul {
    margin-bottom: 1.5rem;
    padding-left: 1.2rem;
  }

  .section-content li {
    margin-bottom: 0.8rem;
    position: relative;
  }

  /* Code Container Styles */
  .code-container {
    margin: 1.5rem 0;
    border-radius: 8px;
    overflow: hidden;
  }

  .code-block {
    background: rgba(0, 0, 0, 0.3);
    padding: 1.5rem;
    border-radius: 8px;
    overflow-x: auto;
    font-family: 'Fira Code', 'Consolas', monospace;
    font-size: 0.9rem;
    line-height: 1.5;
    border: 1px solid rgba(125, 211, 252, 0.2);
  }

  .code-block.swift {
    color: #f8f8f2;
  }

  /* Feature List Styles */
  .feature-list {
    list-style: none;
    padding: 0;
  }

  .feature-list li {
    margin-bottom: 1.5rem;
    padding-left: 0;
  }

  .feature-list strong {
    display: block;
    color: var(--text-accent);
    margin-bottom: 0.3rem;
  }

  /* Challenge-Solution Container */
  .challenge-solution {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    margin: 2rem 0;
  }

  .challenge, .solution {
    background: rgba(13, 25, 42, 0.5);
    border-radius: 8px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
  }

  .challenge {
    border-left: 3px solid rgba(231, 76, 60, 0.7);
  }

  .solution {
    border-left: 3px solid rgba(46, 204, 113, 0.7);
  }

  .challenge h5, .solution h5 {
    margin-top: 0;
    font-size: 1.1rem;
  }

  .challenge h5 {
    color: rgba(231, 76, 60, 0.9);
  }

  .solution h5 {
    color: rgba(46, 204, 113, 0.9);
  }

  @media (max-width: 768px) {
    .challenge-solution {
      grid-template-columns: 1fr;
      gap: 1rem;
    }
  }

  /* Technical Achievement Box */
  .technical-achievement {
    background: rgba(52, 152, 219, 0.1);
    border: 1px solid rgba(52, 152, 219, 0.3);
    border-radius: 8px;
    padding: 1.5rem;
    margin: 2rem 0;
  }

  .technical-achievement h5 {
    color: rgba(52, 152, 219, 0.9);
    margin-top: 0;
    font-size: 1.1rem;
  }

  .technical-achievement ul {
    margin-bottom: 0;
  }

  /* Performance Metrics Styles */
  .performance-metrics {
    display: flex;
    justify-content: space-around;
    gap: 1rem;
    margin: 2rem 0;
    text-align: center;
  }

  .performance-metrics .metric {
    background: rgba(13, 25, 42, 0.5);
    border: 1px solid rgba(125, 211, 252, 0.2);
    border-radius: 8px;
    padding: 1.5rem;
    min-width: 150px;
    transition: all 0.3s ease;
  }

  .performance-metrics .metric:hover {
    transform: translateY(-5px);
    border-color: rgba(125, 211, 252, 0.4);
  }

  .metric-value {
    display: block;
    font-size: 2rem;
    font-weight: 700;
    color: var(--text-accent);
    margin-bottom: 0.5rem;
  }

  .metric-label {
    display: block;
    font-size: 0.9rem;
    color: var(--text-secondary);
  }

  /* Timeline Styles */
  .timeline {
    margin: 2rem 0;
    position: relative;
  }

  .timeline::before {
    content: '';
    position: absolute;
    top: 0;
    bottom: 0;
    left: 15px;
    width: 2px;
    background: rgba(125, 211, 252, 0.3);
  }

  .timeline-item {
    display: flex;
    margin-bottom: 2rem;
    position: relative;
  }

  .timeline-marker {
    width: 30px;
    height: 30px;
    background: var(--text-accent);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
    z-index: 1;
    margin-right: 1.5rem;
    color: var(--bg-primary);
  }

  .timeline-content {
    flex: 1;
    padding-top: 0.25rem;
  }

  .timeline-content h4 {
    margin-top: 0;
    margin-bottom: 0.5rem;
    color: var(--text-accent);
  }

  .timeline-content p {
    margin: 0;
  }

  /* Skills Grid Styles */
  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 1.5rem;
    margin: 2rem 0;
  }

  .skill-card {
    background: rgba(13, 25, 42, 0.5);
    border: 1px solid rgba(125, 211, 252, 0.2);
    border-radius: 8px;
    padding: 1.5rem;
    transition: all 0.3s ease;
    text-align: center;
  }

  .skill-card:hover {
    transform: translateY(-5px);
    border-color: rgba(125, 211, 252, 0.4);
  }

  .skill-icon {
    font-size: 2rem;
    margin-bottom: 1rem;
    display: block;
  }

  .skill-card h4 {
    margin-top: 0;
    margin-bottom: 0.5rem;
    color: var(--text-accent);
  }

  .skill-card p {
    margin: 0;
    font-size: 0.9rem;
  }

  /* Final Status Box */
  .final-status {
    background: linear-gradient(135deg, rgba(13, 25, 42, 0.8), rgba(26, 188, 156, 0.2));
    border-radius: 8px;
    padding: 1.5rem;
    margin-top: 2rem;
    border: 1px solid rgba(125, 211, 252, 0.2);
  }

  .final-status h4 {
    margin-top: 0;
    color: var(--text-accent);
  }

  .final-status p {
    margin-bottom: 0;
  }

  /* Responsive adjustments */
  @media (max-width: 768px) {
    .performance-metrics {
      flex-direction: column;
      align-items: center;
    }
    
    .performance-metrics .metric {
      width: 100%;
      max-width: 250px;
    }
    
    .skills-grid {
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    }
  }

  @media (max-width: 480px) {
    .skills-grid {
      grid-template-columns: 1fr;
    }
  }

  /* Sync Description Styles */
  .sync-description {
    margin: 2rem 0;
    background: rgba(13, 25, 42, 0.5);
    border-radius: 12px;
    padding: 1.5rem;
    border: 1px solid rgba(125, 211, 252, 0.15);
  }
  
  .sync-description h5 {
    color: var(--text-accent);
    margin-top: 0;
    margin-bottom: 1rem;
  }
  
  .feature-cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1rem;
    margin: 1.5rem 0;
  }
  
  .feature-card {
    background: rgba(13, 25, 42, 0.7);
    border-radius: 8px;
    padding: 1.25rem;
    border: 1px solid rgba(125, 211, 252, 0.1);
    transition: all 0.3s ease;
  }
  
  .feature-card:hover {
    transform: translateY(-3px);
    border-color: rgba(125, 211, 252, 0.3);
  }
  
  .feature-card h6 {
    color: var(--text-accent);
    margin-top: 0;
    margin-bottom: 0.75rem;
    font-size: 1.1rem;
  }
  
  .feature-card p {
    margin: 0;
    font-size: 0.95rem;
  }
  
  @media (max-width: 768px) {
    .feature-cards {
      grid-template-columns: 1fr;
    }
  }
</style> 