<script lang="ts">
  import { onMount } from 'svelte';
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';

  gsap.registerPlugin(ScrollTrigger);

  let answered = false;
  let showAnswer = false;
  let noClickCount = 0;

  const photos = [
    'PXL_20250905_200745481.jpg',
    'PXL_20250907_183652052.jpg',
    'PXL_20251003_140956569.jpg',
    'PXL_20251006_172023273.jpg',
    'PXL_20251006_175203932.jpg',
    'PXL_20251108_224231293.jpg',
    'PXL_20251109_202956993.jpg',
    'PXL_20251122_191841870.jpg',
    'PXL_20251122_200156097.jpg',
    'PXL_20251209_195316772.jpg',
    'PXL_20251226_153836738.jpg',
    'PXL_20260101_202121532.jpg',
    'PXL_20260116_215917361.jpg',
    'PXL_20260117_173610236.jpg',
    'PXL_20260117_202818946.jpg',
    'PXL_20260131_224518963.jpg'
  ];

  onMount(() => {
    // Hero fade and rise
    gsap.from('.hero-title', {
      y: 50,
      opacity: 0,
      duration: 1.2,
      ease: 'power3.out'
    });

    gsap.from('.greeting, .subtitle', {
      y: 30,
      opacity: 0,
      duration: 1,
      delay: 0.3,
      stagger: 0.2,
      ease: 'power2.out'
    });

    // Scroll fade hero
    gsap.to('.hero-content', {
      opacity: 0,
      y: -100,
      scrollTrigger: {
        trigger: '.hero-section',
        start: 'top top',
        end: 'bottom top',
        scrub: 2
      }
    });

    // Photo cards stagger in
    gsap.from('.photo-card', {
      y: 100,
      opacity: 0,
      scale: 0.8,
      scrollTrigger: {
        trigger: '.photos-flow',
        start: 'top 80%',
        end: 'top 30%',
        scrub: 1
      },
      stagger: 0.1
    });

    // Section title
    gsap.from('.section-title', {
      y: 50,
      opacity: 0,
      scrollTrigger: {
        trigger: '.section-title',
        start: 'top 80%',
        end: 'top 60%',
        scrub: 1
      }
    });

    // Question section entrance
    gsap.from('.question-container', {
      scale: 0.9,
      opacity: 0,
      scrollTrigger: {
        trigger: '.question-section',
        start: 'top 70%',
        end: 'top 40%',
        scrub: 1
      }
    });
  });

  function handleYes() {
    answered = true;
    showAnswer = true;
    
    setTimeout(() => {
      gsap.from('.answer-container',
        { 
          scale: 0.8,
          opacity: 0,
          y: 50,
          duration: 0.8,
          ease: 'back.out(1.7)'
        }
      );

      gsap.from('.heart',
        { 
          scale: 0,
          opacity: 0,
          duration: 0.6,
          ease: 'back.out(2)',
          stagger: 0.1
        }
      );
      
      gsap.to('.heart', {
        y: [0, -10, 0],
        duration: 2,
        ease: 'power1.inOut',
        repeat: -1,
        stagger: 0.2
      });

      gsap.from('.love-note', {
        x: -30,
        opacity: 0,
        duration: 0.8,
        stagger: 0.2,
        delay: 0.5
      });
    }, 50);
  }

  function handleNo() {
    noClickCount++;
    
    gsap.to('.btn-no', {
      x: [10, -10, 8, -8, 6, -6, 0],
      rotation: [0, 5, -5, 3, -3, 0],
      duration: 0.5,
      ease: 'power2.inOut'
    });
    
    gsap.to('.btn-yes', {
      scale: 1 + (noClickCount * 0.15),
      duration: 0.3,
      ease: 'back.out(2)'
    });
    
    if (noClickCount >= 3) {
      gsap.to('.btn-no', {
        scale: 0.8,
        duration: 0.2
      });
    }
  }
</script>

<!-- HERO SECTION -->
<section class="hero-section">
  <div class="hero-content">
    <div class="greeting">To my beautiful</div>
    <h1 class="hero-title">Anano</h1>
    <div class="subtitle">A collection of moments that made my heart skip</div>
    <div class="scroll-indicator">
      <span>scroll to see our story</span>
      <div class="arrow-down">♡</div>
    </div>
  </div>
</section>

<!-- PHOTOS CONTAINER -->
<section class="photos-container">
  <div class="section-title">Our Memories Together</div>
  
  <div class="photos-flow">
    {#each photos as photo, i}
      <div 
        class="photo-card photo-card-{i}"
        style="--delay: {i * 0.1}s; --offset: {(i % 3) * 20}px;"
      >
        <div class="photo-wrapper">
          <img src="/photos/{photo}" alt="Memory {i + 1}" />
          <div class="photo-overlay">
            <span class="memory-number">Memory {i + 1}</span>
          </div>
        </div>
      </div>
    {/each}
  </div>
  
  <!-- Floating Hearts -->
  <div class="floating-hearts">
    <span class="float-heart" style="--delay: 0s; --x: 20%">♡</span>
    <span class="float-heart" style="--delay: 1s; --x: 60%">♡</span>
    <span class="float-heart" style="--delay: 2s; --x: 80%">♡</span>
    <span class="float-heart" style="--delay: 0.5s; --x: 40%">♡</span>
    <span class="float-heart" style="--delay: 1.5s; --x: 70%">♡</span>
  </div>
  
  <!-- Decorative Divider -->
  <div class="decorative-divider">
    <div class="divider-line"></div>
    <div class="divider-icon">♡</div>
    <div class="divider-line"></div>
  </div>
</section>

<!-- QUESTION SECTION -->
<section class="question-section">
  <div class="question-wrapper">
    <div class="question-container">
      <div class="heart-decoration">♡</div>
      
      <h2 class="the-question">
        Will you be my Valentine?
      </h2>
      
      <p class="question-subtitle">Choose your answer below</p>
      
      {#if !answered}
        <div class="buttons">
          <button class="btn btn-yes" on:click={handleYes}>
            <span>Yes, of course! ♡</span>
            <div class="btn-shine"></div>
          </button>
          <button class="btn btn-no" on:click={handleNo}>
            <span>Not sure...</span>
          </button>
        </div>
        <div class="hint-text">✨ Think carefully ✨</div>
      {/if}

      {#if showAnswer}
        <div class="answer-container">
          <div class="celebration">✨ 🎉 ✨</div>
          
          <h1 class="success-message">
            I knew you'd say yes!
          </h1>
          
          <div class="hearts-row">
            <span class="heart">♡</span>
            <span class="heart">♡</span>
            <span class="heart">♡</span>
          </div>
          
          <div class="special-date">
            <div class="date-label">Our Special Day</div>
            <div class="date-value">February 14, 2026</div>
          </div>
          
          <div class="love-notes">
            <p class="love-note">You make every day feel like magic</p>
            <p class="love-note">Thank you for being mine</p>
            <p class="signature">Forever yours ♡</p>
          </div>
        </div>
      {/if}
    </div>
  </div>
</section>

<style>
  :global(body) {
    overflow-x: hidden;
  }

  /* HERO SECTION */
  .hero-section {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, #FFF5F0 0%, #FFE5D9 50%, #FFD4E0 100%);
    position: relative;
    overflow: hidden;
    padding: 40px 20px;
  }

  .hero-section::before {
    content: '';
    position: absolute;
    width: 300px;
    height: 300px;
    background: radial-gradient(circle, rgba(255, 180, 200, 0.2) 0%, transparent 70%);
    top: -100px;
    right: -100px;
    animation: float 8s ease-in-out infinite;
  }

  .hero-section::after {
    content: '';
    position: absolute;
    width: 400px;
    height: 400px;
    background: radial-gradient(circle, rgba(255, 200, 220, 0.15) 0%, transparent 70%);
    bottom: -150px;
    left: -150px;
    animation: float 10s ease-in-out infinite reverse;
  }

  @keyframes float {
    0%, 100% { transform: translate(0, 0); }
    50% { transform: translate(30px, -30px); }
  }

  .hero-content {
    text-align: center;
    z-index: 10;
    max-width: 800px;
  }

  .greeting {
    font-family: 'Quicksand', sans-serif;
    font-size: 1.2rem;
    color: var(--soft-brown);
    margin-bottom: 20px;
    font-weight: 500;
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  .hero-title {
    font-family: 'Gilda Display', serif;
    font-size: clamp(5rem, 15vw, 10rem);
    font-weight: 400;
    margin: 0 0 30px 0;
    background: linear-gradient(135deg, var(--rose) 0%, var(--blush) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .subtitle {
    font-family: 'Quicksand', sans-serif;
    font-size: 1.3rem;
    color: var(--text);
    margin-bottom: 60px;
    font-weight: 400;
    line-height: 1.6;
  }

  .scroll-indicator {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 15px;
    font-size: 0.9rem;
    letter-spacing: 0.1em;
    color: var(--soft-brown);
  }

  .arrow-down {
    font-size: 2rem;
    color: var(--rose);
    animation: bounce 2s infinite;
  }

  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(10px); }
  }

  /* PHOTOS SECTION */
  .photos-container {
    min-height: 200vh;
    background: linear-gradient(180deg, #FFD4E0 0%, #FFFBF5 50%, #FFF5F0 100%);
    padding: 120px 20px;
    position: relative;
  }

  .section-title {
    font-family: 'Gilda Display', serif;
    font-size: clamp(2.5rem, 6vw, 4rem);
    text-align: center;
    color: var(--text);
    margin-bottom: 80px;
    font-weight: 400;
  }

  .photos-flow {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 40px;
    padding: 0 20px;
  }

  .photo-card {
    width: 100%;
    max-width: 500px;
    opacity: 0;
    animation: fadeInUp 0.8s ease forwards;
    animation-delay: var(--delay);
  }

  .photo-card:nth-child(odd) {
    margin-left: 0;
    margin-right: auto;
  }

  .photo-card:nth-child(even) {
    margin-left: auto;
    margin-right: 0;
  }

  .photo-card:nth-child(3n) {
    margin-left: auto;
    margin-right: auto;
  }

  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .photo-wrapper {
    position: relative;
    width: 100%;
    aspect-ratio: 4/5;
    border-radius: 30px;
    overflow: hidden;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.1);
    transition: transform 0.4s ease, box-shadow 0.4s ease;
    background: white;
  }

  .photo-wrapper:hover {
    transform: translateY(-10px) scale(1.02);
    box-shadow: 0 30px 80px rgba(255, 150, 180, 0.25);
  }

  .photo-wrapper img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .photo-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: linear-gradient(to top, rgba(0, 0, 0, 0.6) 0%, transparent 100%);
    padding: 30px 25px;
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  .photo-wrapper:hover .photo-overlay {
    opacity: 1;
  }

  .memory-number {
    color: white;
    font-size: 1rem;
    font-weight: 600;
    letter-spacing: 0.05em;
  }

  .memories-counter {
    text-align: center;
    margin-top: 100px;
    padding: 40px;
    background: rgba(255, 255, 255, 0.6);
    backdrop-filter: blur(20px);
    border-radius: 40px;
    max-width: 300px;
    margin-left: auto;
    margin-right: auto;
    box-shadow: 0 10px 40px rgba(255, 150, 180, 0.2);
  }

  .counter-number {
    display: block;
    font-family: 'Gilda Display', serif;
    font-size: 5rem;
    font-weight: 400;
    background: linear-gradient(135deg, var(--rose) 0%, var(--blush) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1;
    margin-bottom: 10px;
  }

  .counter-text {
    font-size: 1.1rem;
    color: var(--soft-brown);
    font-weight: 500;
    letter-spacing: 0.05em;
  }

  /* FLOATING HEARTS */
  .floating-hearts {
    position: relative;
    height: 200px;
    overflow: hidden;
    margin: 60px 0;
  }

  .float-heart {
    position: absolute;
    bottom: 0;
    font-size: 2rem;
    color: var(--soft-pink);
    opacity: 0.6;
    animation: floatUp 4s ease-in infinite;
    animation-delay: var(--delay);
    left: var(--x);
  }

  @keyframes floatUp {
    0% {
      transform: translateY(0) rotate(0deg);
      opacity: 0.6;
    }
    50% {
      opacity: 0.8;
    }
    100% {
      transform: translateY(-200px) rotate(15deg);
      opacity: 0;
    }
  }

  /* DECORATIVE DIVIDER */
  .decorative-divider {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    margin: 80px auto;
    max-width: 600px;
    padding: 0 40px;
  }

  .divider-line {
    flex: 1;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--soft-pink) 30%, var(--rose) 50%, var(--soft-pink) 70%, transparent);
    opacity: 0.5;
  }

  .divider-icon {
    font-size: 2.5rem;
    color: var(--rose);
    animation: heartPulse 2s ease-in-out infinite;
  }

  /* QUESTION SECTION */
  .question-section {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, #FFFBF5 0%, #FFF5F0 100%);
    padding: 100px 20px;
    position: relative;
  }

  .question-section::before {
    content: '♡';
    position: absolute;
    font-size: 20rem;
    color: rgba(255, 200, 220, 0.06);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    pointer-events: none;
  }

  .question-wrapper {
    max-width: 700px;
    width: 100%;
    z-index: 1;
  }

  .question-container {
    background: white;
    padding: 80px 60px;
    border-radius: 50px;
    box-shadow: 0 30px 80px rgba(0, 0, 0, 0.08);
    position: relative;
  }

  .heart-decoration {
    position: absolute;
    top: 30px;
    right: 30px;
    font-size: 2.5rem;
    color: var(--soft-pink);
    animation: heartPulse 2s ease-in-out infinite;
  }

  @keyframes heartPulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.1); }
  }

  .the-question {
    font-family: 'Gilda Display', serif;
    font-size: clamp(2.5rem, 5vw, 4rem);
    font-weight: 400;
    text-align: center;
    color: var(--text);
    margin: 0 0 20px 0;
    line-height: 1.3;
  }

  .question-subtitle {
    text-align: center;
    font-size: 1.1rem;
    color: var(--soft-brown);
    margin-bottom: 50px;
    font-weight: 400;
  }

  .buttons {
    display: flex;
    gap: 20px;
    justify-content: center;
    margin-top: 50px;
    flex-wrap: wrap;
  }

  .btn {
    font-family: 'Quicksand', sans-serif;
    font-size: 1.5rem;
    font-weight: 600;
    padding: 20px 50px;
    border: none;
    cursor: pointer;
    border-radius: 50px;
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
  }

  .btn span {
    position: relative;
    z-index: 1;
  }

  .btn:hover {
    transform: translateY(-3px);
  }

  .btn-yes {
    background: linear-gradient(135deg, var(--soft-pink) 0%, var(--rose) 100%);
    color: white;
    box-shadow: 0 10px 30px rgba(255, 150, 180, 0.3);
  }

  .btn-yes:hover {
    box-shadow: 0 15px 40px rgba(255, 150, 180, 0.4);
  }

  .btn-shine {
    position: absolute;
    top: 0;
    left: -100%;
    width: 50%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
    animation: shine 3s infinite;
  }

  @keyframes shine {
    0% { left: -100%; }
    50%, 100% { left: 200%; }
  }

  .btn-no {
    background: white;
    color: var(--text);
    border: 2px solid var(--gray);
  }

  .btn-no:hover {
    background: var(--gray);
  }

  .hint-text {
    text-align: center;
    margin-top: 30px;
    font-size: 1rem;
    color: var(--soft-brown);
    font-weight: 500;
  }

  /* ANSWER SECTION */
  .answer-container {
    margin-top: 60px;
    text-align: center;
  }

  .celebration {
    font-size: 3rem;
    margin-bottom: 30px;
  }

  .success-message {
    font-family: 'Gilda Display', serif;
    font-size: clamp(2.5rem, 6vw, 4.5rem);
    font-weight: 400;
    color: var(--text);
    margin: 0 0 40px 0;
  }

  .hearts-row {
    display: flex;
    justify-content: center;
    gap: 30px;
    margin: 40px 0;
  }

  .heart {
    font-size: 3rem;
    color: var(--rose);
  }

  .special-date {
    margin: 50px 0;
    padding: 30px;
    background: linear-gradient(135deg, var(--soft-pink) 0%, var(--blush) 100%);
    border-radius: 30px;
  }

  .date-label {
    font-size: 0.9rem;
    text-transform: uppercase;
    letter-spacing: 0.15em;
    color: white;
    margin-bottom: 10px;
    font-weight: 600;
  }

  .date-value {
    font-family: 'Gilda Display', serif;
    font-size: 2.5rem;
    color: white;
    font-weight: 400;
  }

  .love-notes {
    margin-top: 40px;
  }

  .love-note {
    font-size: 1.3rem;
    color: var(--text);
    margin: 20px 0;
    font-weight: 400;
  }

  .signature {
    font-family: 'Gilda Display', serif;
    font-size: 1.8rem;
    color: var(--rose);
    margin-top: 30px;
    font-style: italic;
  }

  /* RESPONSIVE */
  @media (max-width: 768px) {
    .hero-title {
      font-size: clamp(3rem, 12vw, 6rem);
    }

    .subtitle {
      font-size: 1.1rem;
    }

    .section-title {
      font-size: 2rem;
      margin-bottom: 50px;
    }

    .photo-card {
      max-width: 100%;
    }

    .photo-card:nth-child(odd),
    .photo-card:nth-child(even),
    .photo-card:nth-child(3n) {
      margin-left: auto;
      margin-right: auto;
    }

    .question-container {
      padding: 50px 30px;
      border-radius: 40px;
    }

    .the-question {
      font-size: 2rem;
    }

    .buttons {
      flex-direction: column;
      gap: 15px;
    }

    .btn {
      font-size: 1.3rem;
      padding: 18px 40px;
    }

    .counter-number {
      font-size: 4rem;
    }

    .success-message {
      font-size: 2rem;
    }

    .date-value {
      font-size: 1.8rem;
    }

    .love-note {
      font-size: 1.1rem;
    }
  }
</style>
