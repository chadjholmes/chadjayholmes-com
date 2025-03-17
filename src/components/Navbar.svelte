<script lang="ts">
  export let techStack: { name: string; icon: string }[];
  export let sections: { id: string; label: string }[] = [];
  export let onNavClick: (id: string) => void;
  export let activeSection = 'home';
  
  let mobileMenuOpen = false;
  
  function handleNavClick(id: string) {
    mobileMenuOpen = false;
    onNavClick(id);
  }
  
  function toggleMobileMenu() {
    mobileMenuOpen = !mobileMenuOpen;
  }
</script>

<nav class="navbar-component">
  <div class="navbar-container">
    <div class="navbar-left">
      <div class="nav-logo" on:click={() => handleNavClick('home')}>CH</div>
    </div>
    
    <!-- Mobile menu button -->
    <button class="mobile-menu-toggle" class:open={mobileMenuOpen} on:click={toggleMobileMenu} aria-label="Toggle navigation menu">
      <span class="bar"></span>
      <span class="bar"></span>
      <span class="bar"></span>
    </button>
    
    <div class="navbar-right" class:open={mobileMenuOpen}>
      {#each sections as section}
        <button 
          class="component-nav-item" 
          class:active={activeSection === section.id}
          on:click={() => handleNavClick(section.id)}
        >
          {section.label}
        </button>
      {/each}
    </div>
  </div>
</nav>

<style>
  /* Navbar Styles - Matching layout */
  .navbar-component {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 100;
    padding: 0.75rem 0;
    transition: all 0.3s ease;
    background: transparent;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border-bottom: 1px solid rgba(125, 211, 252, 0.1);
  }
  
  .navbar-container {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 clamp(1rem, 3vw, 2rem);
  }
  
  .navbar-left {
    display: flex;
    align-items: center;
    gap: 1.5rem;
  }
  
  .navbar-right {
    display: flex;
    align-items: center;
    gap: clamp(0.5rem, 2vw, 2rem);
  }
  
  .nav-logo {
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--text-accent);
    text-shadow: 0 0 10px var(--text-glow);
    transition: color 0.2s ease;
    cursor: pointer;
  }
  
  .nav-logo:hover {
    color: var(--text-primary);
  }
  
  .component-nav-item {
    color: var(--text-secondary);
    text-decoration: none;
    font-size: clamp(0.875rem, 1vw, 1rem);
    padding: 0.5rem 0;
    position: relative;
    transition: all 0.3s ease;
    background: transparent;
    border: none;
    cursor: pointer;
    font-family: inherit;
  }
  
  .component-nav-item:hover,
  .component-nav-item.active {
    color: var(--text-accent);
  }
  
  .component-nav-item::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 2px;
    background: var(--text-accent);
    transition: width 0.3s ease;
  }
  
  .component-nav-item:hover::after,
  .component-nav-item.active::after {
    width: 100%;
  }
  
  /* Mobile menu styles */
  .mobile-menu-toggle {
    display: none;
    flex-direction: column;
    justify-content: space-between;
    width: 30px;
    height: 21px;
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0;
    z-index: 10;
  }
  
  .mobile-menu-toggle .bar {
    height: 3px;
    width: 100%;
    background-color: var(--text-accent);
    border-radius: 10px;
    transition: all 0.3s ease;
  }
  
  .mobile-menu-toggle.open .bar:nth-child(1) {
    transform: translateY(9px) rotate(45deg);
  }
  
  .mobile-menu-toggle.open .bar:nth-child(2) {
    opacity: 0;
  }
  
  .mobile-menu-toggle.open .bar:nth-child(3) {
    transform: translateY(-9px) rotate(-45deg);
  }
  
  /* Add responsive breakpoints for fine-tuning */
  @media (max-width: 1024px) {
    .ticker-item {
      padding: 0 0.5rem;
    }
    
    .ticker-item i {
      font-size: 1.1rem;
    }
    
    .ticker-item .tech-name {
      font-size: 0.75rem;
    }
  }
  
  @media (max-width: 768px) {
    .navbar-right {
      position: fixed;
      top: 4rem;
      right: -100%;
      width: 100%;
      height: auto;
      flex-direction: column;
      align-items: flex-end;
      padding: 1rem;
      background: rgba(2, 12, 27, 0.8);
      backdrop-filter: blur(10px);
      transition: right 0.3s ease;
      border-bottom: 1px solid rgba(125, 211, 252, 0.1);
    }
    
    .navbar-right.open {
      right: 0;
    }
    
    .tech-ticker-container {
      display: none;
    }
    
    .mobile-menu-toggle {
      display: flex;
    }
  }

  @media (max-width: 480px) {
    .navbar-component {
      padding: 0.5rem 0;
    }
    
    .nav-logo {
      font-size: 1.25rem;
    }
  }
</style> 