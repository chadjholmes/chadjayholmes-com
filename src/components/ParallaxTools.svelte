<script lang="ts">
  import { onMount } from 'svelte';
  
  export let scrollY = 0;
  
  // Tool interface definition
  interface Tool {
    name: string;
    icon: string;
    x: number;
    y: number;
    size: number;
    speed: number;
  }
  
  // Expanded tech tools list - we'll show more tools here than in the ticker
  const tools: Tool[] = [
    { name: 'TypeScript', icon: 'devicon-typescript-plain', x: 15, y: 20, size: 2.2, speed: 0.03 },
    { name: 'JavaScript', icon: 'devicon-javascript-plain', x: 85, y: 35, size: 2.0, speed: 0.05 },
    { name: 'Ruby', icon: 'devicon-ruby-plain', x: 25, y: 65, size: 2.4, speed: 0.02 },
    { name: 'Rails', icon: 'devicon-rails-plain', x: 80, y: 15, size: 2.1, speed: 0.04 },
    { name: 'Python', icon: 'devicon-python-plain', x: 10, y: 40, size: 2.3, speed: 0.06 },
    { name: 'React', icon: 'devicon-react-original', x: 75, y: 75, size: 2.2, speed: 0.03 },
    { name: 'Svelte', icon: 'devicon-svelte-plain', x: 60, y: 30, size: 1.9, speed: 0.04 },
    { name: 'Docker', icon: 'devicon-docker-plain', x: 30, y: 80, size: 2.1, speed: 0.05 },
    { name: 'AWS', icon: 'devicon-amazonwebservices-original', x: 20, y: 25, size: 2.0, speed: 0.07 },
    { name: 'Git', icon: 'devicon-git-plain', x: 70, y: 60, size: 2.3, speed: 0.02 },
    { name: 'Node', icon: 'devicon-nodejs-plain', x: 40, y: 45, size: 2.2, speed: 0.04 },
    { name: 'PostgreSQL', icon: 'devicon-postgresql-plain', x: 65, y: 10, size: 2.0, speed: 0.06 },
    { name: 'MongoDB', icon: 'devicon-mongodb-plain', x: 5, y: 70, size: 2.1, speed: 0.03 },
    { name: 'Terraform', icon: 'devicon-terraform-plain', x: 90, y: 85, size: 2.2, speed: 0.05 },
    { name: 'GraphQL', icon: 'devicon-graphql-plain', x: 35, y: 55, size: 2.0, speed: 0.02 },
    { name: 'Redis', icon: 'devicon-redis-plain', x: 55, y: 65, size: 1.9, speed: 0.03 },
    { name: 'OpenSearch', icon: 'devicon-opensearch-original', x: 45, y: 5, size: 2.2, speed: 0.04 }
  ];
  
  let mouseX = 0;
  let mouseY = 0;
  let windowWidth: number = 0;
  let windowHeight: number = 0;
  
  function handleMouseMove(event: MouseEvent): void {
    mouseX = (event.clientX / windowWidth) - 0.5;
    mouseY = (event.clientY / windowHeight) - 0.5;
  }
  
  onMount(() => {
    windowWidth = window.innerWidth;
    windowHeight = window.innerHeight;
    
    window.addEventListener('mousemove', handleMouseMove);
    window.addEventListener('resize', () => {
      windowWidth = window.innerWidth;
      windowHeight = window.innerHeight;
    });
    
    return () => {
      window.removeEventListener('mousemove', handleMouseMove);
      window.removeEventListener('resize', () => {});
    };
  });
  
  function getParallaxStyle(tool: Tool): string {
    // Calculate position based on scrolling and mouse movement
    const scrollOffset = scrollY * tool.speed;
    const mouseOffsetX = mouseX * 20 * tool.speed;
    const mouseOffsetY = mouseY * 20 * tool.speed;
    
    // Add slight rotation based on mouse position
    const rotation = (mouseX * 5 * tool.speed) + (mouseY * 5 * tool.speed);
    
    return `
      left: ${tool.x}%;
      top: ${tool.y}%;
      transform: translate(
        calc(${mouseOffsetX}px - 50%), 
        calc(${scrollOffset}px + ${mouseOffsetY}px - 50%)
      ) rotate(${rotation}deg);
      font-size: ${tool.size}rem;
      opacity: 1; /* Full opacity for debugging */
      background-color: rgba(0, 0, 0, 0.3);
      padding: 10px;
      border-radius: 8px;
    `;
  }
</script>

<div class="parallax-container debug-mode">
  {#each tools as tool}
    <div class="tool-icon" style={getParallaxStyle(tool)}>
      <i class={tool.icon}></i>
      <span class="tool-name">{tool.name}</span>
    </div>
  {/each}
</div>

<style>
  .parallax-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    z-index: 10;
    pointer-events: none;
  }
  
  .debug-mode .tool-icon {
    outline: 2px solid red;
  }
  
  .tool-icon {
    position: absolute;
    color: #FFFFFF; /* Bright white for maximum contrast */
    filter: drop-shadow(0 0 20px rgba(255, 255, 255, 0.8));
    transition: transform 0.3s ease-out, opacity 0.5s ease;
    will-change: transform, opacity;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  
  .tool-icon i {
    opacity: inherit;
  }
  
  .tool-name {
    font-weight: bold;
    font-size: 0.7em;
  }
  
  /* Adjust visibility on smaller screens */
  @media (max-width: 768px) {
    .tool-icon {
      opacity: 0.8 !important;
    }
  }
  
  @media (max-width: 480px) {
    .parallax-container {
      display: none;
    }
  }
</style> 