<script lang="ts">
  import '../styles/global.css';
  import '../app.css';
  import { page } from '$app/stores';
  import { onMount } from 'svelte';
  import { navigating } from '$app/stores';
  import { fade } from 'svelte/transition';

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
      // Get navbar height to use as offset
      const navbar = document.querySelector('.navbar');
      const navbarHeight = navbar ? navbar.getBoundingClientRect().height : 0;
      
      // Calculate the element's position relative to the top of the page
      const sectionPosition = section.getBoundingClientRect().top + window.scrollY;
      
      // Add additional offset for the experience section
      let additionalOffset = 20; // Default extra padding
      if (sectionId === 'experience') {
        additionalOffset = 20; // Extra offset for experience section
      }
      
      // Scroll to element with offset for navbar
      window.scrollTo({
        top: sectionPosition - navbarHeight - additionalOffset,
        behavior: 'smooth'
      });
    }
  }

  // Function to toggle projects dropdown
  function toggleProjectsMenu(event: MouseEvent) {
    event.stopPropagation();
    projectsMenuOpen = !projectsMenuOpen;
  }
  
  // Close dropdown when clicking outside
  function handleClickOutside(event: MouseEvent) {
    if (projectsMenuOpen && !(event.target as HTMLElement).closest('.projects-dropdown')) {
      projectsMenuOpen = false;
    }
  }

  onMount(() => {
    visible = true;
    
    // Set up intersection observer to track which section is in view
    const sections = ['home', 'about', 'experience', 'projects'];
    const sectionElements = sections.map(id => document.getElementById(id));
    
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        // Store the most visible section
        if (entry.isIntersecting && entry.intersectionRatio >= 0.2) {
          activeSection = entry.target.id;
        }
      });
    }, { 
      threshold: [0.2, 0.5, 0.8],
      rootMargin: '-100px 0px -100px 0px' // Adjust the margins to better control when sections become active
    });
    
    sectionElements.forEach(el => {
      if (el) observer.observe(el);
    });
    
    // Update active section on navigation click to ensure it stays active
    const handleNavClick = (id: string) => {
      // Set the active section immediately on click
      activeSection = id;
    };
    
    // Add click listeners to nav items
    document.querySelectorAll('.nav-item').forEach(item => {
      item.addEventListener('click', () => {
        const id = item.getAttribute('data-section');
        if (id) handleNavClick(id);
      });
    });
    
    // Add event listener for click outside
    document.addEventListener('click', handleClickOutside);
    
    return () => {
      sectionElements.forEach(el => {
        if (el) observer.unobserve(el);
      });
      document.removeEventListener('click', handleClickOutside);
      
      // Remove click listeners from nav items
      document.querySelectorAll('.nav-item').forEach(item => {
        item.removeEventListener('click', () => {});
      });
    };
  });
</script>

<svelte:head>
  <!-- DevIcon font for tech stack icons -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/devicons/devicon@v2.15.1/devicon.min.css">
</svelte:head>

<!-- Navigation Bar -->
<nav class="navbar" class:visible>
  <div class="navbar-container">
    <div class="navbar-left">
      <button class="logo" on:click={() => scrollToSection('home')}>CH</button>
    </div>
    
    <!-- Mobile menu button -->
    <button class="mobile-menu-toggle" class:open={mobileMenuOpen} on:click={toggleMobileMenu} aria-label="Toggle navigation menu">
      <span class="bar"></span>
      <span class="bar"></span>
      <span class="bar"></span>
    </button>
    
    <div class="navbar-right" class:open={mobileMenuOpen}>
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
      
      <!-- Projects dropdown -->
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
    </div>
  </div>
</nav>

<main>
  <slot /> 
</main>

<style>
  /* Typography Colors */
  :root {
    --text-primary: rgb(248, 250, 252);    /* Crisp white with slight warmth for confidence */
    --text-accent: rgb(125, 211, 252);     /* Bright sky blue for impact without aggression */
    --text-secondary: rgb(186, 230, 253);  /* Lighter blue for supporting text */
    --text-glow: rgba(125, 211, 252, 0.3); /* Subtle glow for sophistication */
    --bg-dark: rgb(2, 12, 27);             /* Dark blue background */
    --bg-dark-transparent: rgba(2, 12, 27, 0.9); /* Dark blue semi-transparent */
  }

  /* Navigation Bar Styles */
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

  @media (max-width: 768px) {
    .navbar-container {
      padding: 0 1rem;
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
    padding-top: 5rem; /* Add padding to account for the fixed navbar */
    /* Removed background-color to let the body background show through */
    color: var(--text-primary);
  }
  
  /* Global styles to ensure consistency */
  :global(body) {
    background-color: var(--bg-color);
    margin: 0;
    padding: 0;
    color: var(--text-primary);
    font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    scroll-behavior: smooth;
  }

  /* Projects Dropdown Styles */
  .projects-dropdown {
    position: relative;
    display: inline-block;
  }
  
  .dropdown-arrow {
    font-size: 0.8rem;
    margin-left: 5px;
    transition: transform 0.2s ease;
  }
  
  .projects-dropdown .nav-item.active .dropdown-arrow {
    transform: rotate(180deg);
  }
  
  .dropdown-menu {
    position: absolute;
    top: 100%;
    right: 0;
    background: rgba(13, 25, 42, 0.95);
    border: 1px solid rgba(125, 211, 252, 0.15);
    backdrop-filter: blur(10px);
    border-radius: 8px;
    min-width: 200px;
    z-index: 1000;
    overflow: hidden;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  }
  
  .dropdown-item {
    display: block;
    width: 100%;
    padding: 0.75rem 1.25rem;
    text-align: left;
    color: var(--text-secondary);
    text-decoration: none;
    transition: all 0.2s ease;
    background: transparent;
    border: none;
    cursor: pointer;
    font-family: inherit;
    font-size: 1rem;
  }
  
  .dropdown-item:hover {
    background: rgba(125, 211, 252, 0.1);
    color: var(--text-accent);
  }
  
  /* Mobile dropdown adjustments */
  @media (max-width: 768px) {
    .dropdown-menu {
      position: static;
      width: 100%;
      border: none;
      border-top: 1px solid rgba(125, 211, 252, 0.15);
      border-radius: 0;
      background: rgba(13, 25, 42, 0.5);
      box-shadow: none;
    }
    
    .dropdown-item {
      padding-left: 2rem;
    }
  }
</style> 