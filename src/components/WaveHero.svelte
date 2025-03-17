<script lang="ts">
  import { onMount } from 'svelte';
  import { createNoise2D } from 'simplex-noise';
  
  export let scrollY = 0;
  export let innerHeight = 0;
  export let fixedWave = true;
  
  let visible = false;
  let typedName = '';
  let typingComplete = false;
  let fullName = 'CHAD HOLMES';
  let currentIndex = 0;
  let typingTimer: ReturnType<typeof setInterval>;
  let path: SVGPathElement | null = null;
  const noise2D = createNoise2D();
  let animationFrame: number;
  let waveTransitionProgress = 0;

  // === Wave Animation Controls ===
  let noiseStep = 0.0005;    // Controls speed of wave movement (lower = slower)
  let time = 0;
  
  // === Typing Animation Controls ===
  const typingSpeed = 120; // ms per character
  
  interface WavePoint {
    x: number;
    y: number;
    originY: number;
    noiseOffset: number;
  }
  
  function createWavePoints(): WavePoint[] {
    const points: WavePoint[] = [];
    const segments = 60;      // Doubled segments for smoother transitions
    const width = 3600;      
    const segmentWidth = width / segments;
    
    for (let i = 0; i <= segments; i++) {
      points.push({
        x: i * segmentWidth,
        y: 120,              
        originY: 120,        
        noiseOffset: i * 0.5
      });
    }
    return points;
  }

  function updateWave(points: WavePoint[], transitionAmount: number): string {
    let pathD = `M 0 200`;
    pathD += ` L ${points[0].x} ${points[0].y}`;
    
    // Use cardinal spline approach for smooth continuous curves
    const tension = 0.4; // Controls how "tight" the curve is (0.5-0.75 for more rounded)
    
    for (let i = 0; i < points.length - 1; i++) {
      const current = points[i];
      const next = points[i + 1];
      
      // Get points before and after for tangent calculations (with bounds checking)
      const prev = i > 0 ? points[i - 1] : { x: current.x - (next.x - current.x), y: current.y };
      const afterNext = i < points.length - 2 ? points[i + 2] : { x: next.x + (next.x - current.x), y: next.y };
      
      // Calculate tangent vectors that ensure continuous derivatives
      const tangentCurrentX = (next.x - prev.x) * tension;
      const tangentCurrentY = (next.y - prev.y) * tension;
      const tangentNextX = (afterNext.x - current.x) * tension;
      const tangentNextY = (afterNext.y - current.y) * tension;
      
      // Control points based on tangent vectors
      const controlX1 = current.x + tangentCurrentX / 3;
      const controlY1 = current.y + tangentCurrentY / 3;
      const controlX2 = next.x - tangentNextX / 3;
      const controlY2 = next.y - tangentNextY / 3;
      
      pathD += ` C ${controlX1} ${controlY1}, ${controlX2} ${controlY2}, ${next.x} ${next.y}`;
    }
    
    // Mirror the wave vertically based on scroll progress
    if (transitionAmount > 0) {
      // Create a mirrored version of the wave
      // Start at the end point of the forward wave
      const lastPoint = points[points.length - 1];
      
      // Add a control point for the mirror transition
      pathD += ` L 3600 200`;
      
      // Gradually open up to reveal the content below based on transition amount
      pathD += ` L 3600 ${200 + 400 * transitionAmount}`;
      pathD += ` L 0 ${200 + 400 * transitionAmount}`;
    } else {
      // Original closing of the path
      pathD += ` L 3600 200 L 0 200`;
    }
    
    pathD += ` Z`;
    return pathD;
  }

  function animate(points: WavePoint[]) {
    points.forEach(point => {
      // Use smoother interpolation for noise values
      const noiseValue = noise2D(point.noiseOffset, time) * 15;
      
      // Apply gentle easing to transitions
      point.y = point.originY + noiseValue;
      point.noiseOffset += noiseStep;
    });

    if (path) {
      // Calculate wave transition progress based on scroll position
      // Normalize to a value between 0 and 1 for the first viewport height of scrolling
      waveTransitionProgress = Math.min(1, Math.max(0, scrollY / (innerHeight * 0.7)));
      
      path.setAttribute('d', updateWave(points, waveTransitionProgress));
      
      // Also update wave position based on scroll
      const container = document.querySelector('.gradient-container') as HTMLElement;
      if (container) {
        // Move the wave container up as you scroll, but at a slower rate
        const translateY = scrollY * 0.3;
        container.style.transform = `translateY(${translateY}px)`;
      }
    }
    
    time += noiseStep;
    animationFrame = requestAnimationFrame(() => animate(points));
  }

  function startTypingAnimation() {
    typingTimer = setInterval(() => {
      if (currentIndex < fullName.length) {
        typedName += fullName[currentIndex];
        currentIndex++;
      } else {
        clearInterval(typingTimer);
        typingComplete = true;
      }
    }, typingSpeed);
  }
  
  onMount(() => {
    visible = true;
    const points = createWavePoints();
    path = document.querySelector('.wave-base');
    if (path) {
      animate(points);
    }
    
    // Start the typing animation
    setTimeout(() => {
      startTypingAnimation();
    }, 500); // Delay before typing starts

    return () => {
      if (animationFrame) {
        cancelAnimationFrame(animationFrame);
      }
      clearInterval(typingTimer);
    };
  });
</script>

<div class="hero" class:visible>
  <div class="gradient-container" class:fixed={fixedWave}>
    <svg class="wave-svg" viewBox="0 0 3600 200" preserveAspectRatio="xMidYMax slice">
      <defs>
        <linearGradient id="waveGradient" x1="0" y1="0" x2="1" y2="1">
          <stop offset="0%" stop-color="rgba(41, 128, 185, 1.0)" />
          <stop offset="20%" stop-color="rgba(93, 63, 211, 1.0)" />
          <stop offset="40%" stop-color="rgba(52, 152, 219, 1.0)" />
          <stop offset="60%" stop-color="rgba(125, 95, 255, 1.0)" />
          <stop offset="75%" stop-color="rgba(41, 128, 185, 1.0)" />
          <stop offset="82%" stop-color="rgba(41, 128, 185, 0.3)" />
          <stop offset="88%" stop-color="rgba(41, 128, 185, 0.1)" />
          <stop offset="100%" stop-color="rgba(41, 128, 185, 0)" />
        </linearGradient>
      </defs>
      <path class="wave-base" fill="url(#waveGradient)" />
    </svg>
  </div>
  <div class="content">
    <h1 class="name">
      <span>{typedName}</span><span class="cursor" class:blinking={typingComplete}>|</span>
    </h1>
    <h2>Senior Software Engineer</h2>
    <p class="tagline"></p>
  </div>
</div>

<style>
  .hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    background-color: var(--bg-color);
    color: var(--text-color);
    padding: clamp(1rem, 5vw, 2rem);  /* Responsive padding */
    position: relative;
    overflow: visible; /* Changed from hidden to allow the wave to expand */
    padding-top: max(clamp(1rem, 5vw, 2rem), 5rem); /* Ensure enough padding for the navbar */
  }

  .gradient-container {
    position: absolute;
    inset: 0;
    overflow: visible;
    z-index: 1;
    display: flex;
    align-items: flex-end;
    will-change: transform;
  }
  
  .gradient-container.fixed {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    top: auto;
    height: 50vh; /* Control height of the fixed wave */
    pointer-events: none; /* Allow interaction with elements below */
  }

  /* === Wave Position and Size Controls === */
  .wave-svg {
    position: absolute;
    bottom: -12%;
    left: -5%;
    width: 140%;          /* Increased width to ensure coverage */
    height: 80vh;
    filter: blur(8px);
    fill: var(--gradient-deep-ocean);
    mix-blend-mode: normal;
    transform: translateZ(0);
    backface-visibility: hidden;
    perspective: 1000;
    will-change: transform;
  }

  .wave-base {
    transform-origin: bottom;
    transform: scale(1.4);
    filter: drop-shadow(0 0 8px rgba(41, 128, 185, 0.2));  /* Subtle glow effect */
    transition: d 0.2s ease-out; /* Smooth transitions for path changes */
    will-change: d; /* Optimize for path animations */
  }

  .content {
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 clamp(1rem, 3vw, 2rem);  /* Responsive padding */
    width: 100%;
    position: relative;
    z-index: 2;
  }

  .hero.visible .content {
    opacity: 1;
    transform: translateY(0);
  }

  .name {
    font-size: clamp(2.8rem, 10vw, 8rem);  /* Slightly reduced size */
    font-weight: 700;
    font-family: 'Inter', sans-serif;
    margin: 0;
    line-height: 1;  /* Tighter line height for better wrapping */
    letter-spacing: -0.02em;
    color: var(--text-primary);
    position: relative;
    text-align: left;
    text-shadow: 
      0 0 1px rgba(255, 255, 255, 0.2),
      0 0 15px var(--text-glow),
      0 0 25px var(--text-glow);
    word-break: break-word;  /* Prevent overflow on small screens */
    hyphens: auto;  /* Enable hyphenation for better wrapping */
  }

  .cursor {
    display: inline-block;
    color: var(--text-accent);
    font-weight: 400;
    animation: none;
  }

  .cursor.blinking {
    animation: blink 1s step-end infinite;
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  h2 {
    font-size: clamp(1.25rem, 3vw, 2.5rem);  /* Smaller base size for mobile */
    font-weight: 400;
    font-family: 'Inter', sans-serif;
    margin: clamp(0.5rem, 2vw, 1rem) 0;  /* Responsive margins */
    color: white;
    opacity: 0.9;
    animation: fadeIn 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    animation-delay: 1s;
    text-align: left;
    line-height: 1.2;  /* Improved readability */
  }

  .tagline {
    font-size: clamp(0.875rem, 1.5vw, 1.25rem);  /* Smaller base size for mobile */
    margin: clamp(1rem, 3vw, 1.5rem) 0;  /* Responsive margins */
    opacity: 0.85;
    animation: fadeIn 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    animation-delay: 1.2s;
    max-width: 600px;
    font-family: var(--font-mono);
    color: var(--text-secondary);
    text-align: left;
    line-height: 1.5;  /* Improved readability */
  }

  @keyframes fadeIn {
    to {
      opacity: 1;
    }
  }

  @media (max-width: 480px) {
    .hero {
      padding: 1rem;  /* Smaller padding on very small screens */
      align-items: flex-start;  /* Better vertical alignment on mobile */
      padding-top: 20vh;  /* Push content down a bit */
    }

    .content {
      padding: 0 1rem;
    }

    .name {
      font-size: clamp(2rem, 12vw, 2.5rem);  /* Even smaller on tiny screens */
      margin-bottom: 0.5rem;
    }

    h2 {
      font-size: clamp(1rem, 5vw, 1.25rem);
      margin: 0.5rem 0;
    }

    .tagline {
      font-size: clamp(0.875rem, 4vw, 1rem);
      margin: 1rem 0;
      max-width: 100%;  /* Full width on mobile */
    }
  }

  /* Add tablet breakpoint for mid-sized devices */
  @media (min-width: 481px) and (max-width: 768px) {
    .hero {
      padding: clamp(1.5rem, 4vw, 2rem);
      align-items: center;
    }

    .name {
      font-size: clamp(2.5rem, 10vw, 4rem);
    }
  }
</style> 