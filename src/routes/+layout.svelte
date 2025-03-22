<script lang="ts">
  import '../styles/global.css';
  import '../app.css';
  import { page } from '$app/stores';
  import { onMount } from 'svelte';
  import { navigating } from '$app/stores';
  import { fade } from 'svelte/transition';
  import { goto } from '$app/navigation';

  let visible = false;
  let prevScrollPos = 0;
  let mobileMenuOpen = false;
  let activeSection = 'home';
  let projectsMenuOpen = false;
  
  function toggleMobileMenu() {
    mobileMenuOpen = !mobileMenuOpen;
  }

  function scrollToSection(sectionId: string) {
    mobileMenuOpen = false;
    const section = document.getElementById(sectionId);
    if (section) {
      const navbar = document.querySelector('.navbar');
      const navbarHeight = navbar ? navbar.getBoundingClientRect().height : 0;
      
      const sectionPosition = section.getBoundingClientRect().top + window.scrollY;
      
      let additionalOffset = 20;
      if (sectionId === 'experience') {
        additionalOffset = 20;
      }
      
      window.scrollTo({
        top: sectionPosition - navbarHeight - additionalOffset,
        behavior: 'smooth'
      });
    }
  }

  function goToAbout() {
    goto('/');
    setTimeout(() => {
      scrollToSection('about');
    }, 100);
  }
  
  async function goToProjects() {
    await goto('/');
    setTimeout(() => {
      scrollToSection('projects');
    }, 100);
  }

  function toggleProjectsMenu(event: MouseEvent) {
    event.stopPropagation();
    projectsMenuOpen = !projectsMenuOpen;
  }
  
  function handleClickOutside(event: MouseEvent) {
    if (projectsMenuOpen && !(event.target as HTMLElement).closest('.projects-dropdown')) {
      projectsMenuOpen = false;
    }
  }

  onMount(() => {
    visible = true;
    
    const sections = ['home', 'about', 'experience', 'projects'];
    const sectionElements = sections.map(id => document.getElementById(id));
    
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting && entry.intersectionRatio >= 0.2) {
          activeSection = entry.target.id;
        }
      });
    }, { 
      threshold: [0.2, 0.5, 0.8],
      rootMargin: '-100px 0px -100px 0px'
    });
    
    sectionElements.forEach(el => {
      if (el) observer.observe(el);
    });
    
    const handleNavClick = (id: string) => {
      activeSection = id;
    };
    
    document.querySelectorAll('.nav-item').forEach(item => {
      item.addEventListener('click', () => {
        const id = item.getAttribute('data-section');
        if (id) handleNavClick(id);
      });
    });
    
    document.addEventListener('click', handleClickOutside);
    
    return () => {
      sectionElements.forEach(el => {
        if (el) observer.unobserve(el);
      });
      document.removeEventListener('click', handleClickOutside);
      
      document.querySelectorAll('.nav-item').forEach(item => {
        item.removeEventListener('click', () => {});
      });
    };
  });
</script>

<svelte:head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/devicons/devicon@v2.15.1/devicon.min.css">
</svelte:head>

<nav class="navbar" class:visible>
  <div class="navbar-container">
    <div class="navbar-left">
      {#if $page.url.pathname.includes('/case-studies/') }
        <button class="back-button" on:click={goToProjects}>
          <span class="back-arrow">←</span> Back to Projects
        </button>
      {:else if $page.url.pathname.includes('/experience/education')}
        <button class="back-button" on:click={goToAbout}>
          <span class="back-arrow">←</span> Back to About Me
        </button>
      {:else}
        <button class="logo" on:click={() => scrollToSection('home')}>CH</button>
      {/if}
    </div>
    
    {#if $page.url.pathname.includes('/case-studies/bookends')}
      <a 
        href="https://bookends.app" 
        target="_blank" 
        rel="noopener noreferrer" 
        class="nav-app-link">
        Visit Bookends App →
      </a>
    {:else if !$page.url.pathname.includes('/case-studies/') && !$page.url.pathname.includes('/experience/education')}
      <button class="mobile-menu-toggle" class:open={mobileMenuOpen} on:click={toggleMobileMenu} aria-label="Toggle navigation menu">
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </button>
    {/if}
    
    <div class="navbar-right" class:open={mobileMenuOpen}>
      {#if !$page.url.pathname.includes('/case-studies/') && !$page.url.pathname.includes('/experience/education')}
        <button 
          class="nav-item" 
          class:active={activeSection === 'home'}
          on:click={() => scrollToSection('home')}
          data-section="home"
        >
          Home
        </button>
        <button 
          class="nav-item" 
          class:active={activeSection === 'about'}
          on:click={() => scrollToSection('about')}
          data-section="about"
        >
          About
        </button>
        <button 
          class="nav-item" 
          class:active={activeSection === 'experience'}
          on:click={() => scrollToSection('experience')}
          data-section="experience"
        >
          Experience
        </button>
        
        <div class="projects-dropdown">
          <button 
            class="nav-item" 
            class:active={activeSection === 'projects' || $page.url.pathname.includes('/case-studies')}
            on:click={() => scrollToSection('projects')}
            data-section="projects"
          >
            Projects
          </button>
        </div>
      {/if}
    </div>
  </div>
</nav>

<main>
  <slot /> 
</main>

<style>
  :root {
    --text-primary: rgb(248, 250, 252);
    --text-accent: rgb(125, 211, 252);
    --text-secondary: rgb(186, 230, 253);
    --text-glow: rgba(125, 211, 252, 0.3);
    --bg-dark: rgb(2, 12, 27);
    --bg-dark-transparent: rgba(2, 12, 27, 0.9);
  }

  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 100;
    padding: 0.75rem 0;
    transition: all 0.3s ease;
    background: transparent;
    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);
    border-bottom: 1px solid rgba(125, 211, 252, 0.1);
    opacity: 0;
    transform: translateY(-10px);
  }

  .navbar.visible {
    opacity: 1;
    transform: translateY(0);
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

  .logo {
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--text-accent);
    text-decoration: none;
    letter-spacing: -0.02em;
    text-shadow: 0 0 10px var(--text-glow);
    transition: all 0.3s ease;
    cursor: pointer;
    background: none;
    border: none;
    padding: 0;
    font-family: inherit;
  }

  .logo:hover {
    color: var(--text-primary);
    transform: scale(1.05);
  }

  .navbar-right {
    display: flex;
    gap: clamp(0.5rem, 2vw, 2rem);
    align-items: center;
  }

  .nav-item {
    color: var(--text-secondary);
    text-decoration: none;
    font-size: clamp(0.875rem, 1vw, 1rem);
    padding: 0.5rem 0;
    position: relative;
    transition: all 0.3s ease;
    background: none;
    border: none;
    font-family: inherit;
    cursor: pointer;
  }

  .nav-item:hover, .nav-item.active {
    color: var(--text-accent);
  }

  .nav-item::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 2px;
    background: var(--text-accent);
    transition: width 0.3s ease;
  }

  .nav-item:hover::after, .nav-item.active::after {
    width: 100%;
  }
  
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

  @media (max-width: 768px) {
    .navbar-container {
      padding: 0 1rem;
    }
    
    .navbar-left {
      display: flex;
      align-items: center;
      flex-wrap: wrap;
      gap: 0.5rem;
    }
    
    .mobile-menu-toggle {
      display: flex;
    }
    
    .navbar-right {
      position: fixed;
      top: 0;
      right: -100%;
      flex-direction: column;
      background: rgba(2, 12, 27, 0.95);
      backdrop-filter: blur(10px);
      height: 100vh;
      width: 70%;
      max-width: 300px;
      padding: 5rem 2rem;
      gap: 2rem;
      transition: right 0.3s ease;
      align-items: flex-start;
      z-index: 9;
    }
    
    .navbar-right.open {
      right: 0;
    }
    
    .nav-item {
      font-size: 1.25rem;
    }
  }
  
  @media (max-width: 480px) {
    .navbar-container {
      flex-direction: row;
    }
    
    .navbar {
      padding: 0.5rem 0;
    }
    
    .logo {
      font-size: 1.25rem;
    }
  }

  main {
    min-height: 100vh;
    padding-top: 5rem;
    color: var(--text-primary);
  }
  
  :global(body) {
    background-color: var(--bg-color);
    margin: 0;
    padding: 0;
    color: var(--text-primary);
    font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    scroll-behavior: smooth;
  }

  .projects-dropdown {
    position: relative;
    display: inline-block;
  }

  .back-button {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 1rem;
    color: var(--text-accent);
    background: none;
    border: none;
    padding: 0.5rem 0;
    cursor: pointer;
    transition: all 0.3s ease;
    font-family: inherit;
  }
  
  .back-button:hover {
    color: var(--text-primary);
    transform: translateX(-3px);
  }
  
  .back-arrow {
    font-size: 1.2rem;
    transition: transform 0.3s ease;
  }
  
  .back-button:hover .back-arrow {
    transform: translateX(-3px);
  }
  
  @media (max-width: 768px) {
    .back-button {
      font-size: 0.9rem;
    }
  }

  .nav-app-link {
    display: inline-block;
    background: rgba(26, 188, 156, 0.15);
    color: var(--text-accent);
    text-decoration: none;
    padding: 0.6rem 1.2rem;
    border-radius: 8px;
    font-weight: 500;
    border: 1px solid rgba(125, 211, 252, 0.3);
    transition: all 0.3s ease;
    font-size: 1rem;
    margin-left: auto;
  }
  
  .nav-app-link:hover {
    transform: translateY(-2px);
    border-color: rgba(125, 211, 252, 0.6);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    background: rgba(26, 188, 156, 0.25);
    color: var(--text-primary);
  }
  
  @media (max-width: 768px) {
    .nav-app-link {
      font-size: 0.9rem;
      padding: 0.5rem 1rem;
    }
  }
  
  @media (max-width: 480px) {
    .nav-app-link {
      font-size: 0.75rem;
      padding: 0.3rem 0.7rem;
    }
  }
</style> 