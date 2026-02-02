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

  // Assign parallax depths (1 = slow, 2 = medium, 3 = fast)
  const photoDepths = photos.map((_, i) => (i % 3) + 1);

  const floatingTexts = [
    'EVERY MOMENT',
    'WITH YOU',
    'IS SPECIAL',
    'YOU MAKE ME',
    'SO HAPPY'
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
    
    gsap.fromTo('.hint-text',
      { scale: 1, color: 'var(--red)' },
      { 
        scale: [1.2, 1],
        color: ['#000000', 'var(--red)'],
        duration: 0.3,
        ease: 'power2.out'
      }
    );
    
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
  
  <div class="memories-counter">
    <span class="counter-number">{photos.length}</span>
    <span class="counter-text">beautiful moments</span>
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
  .hero-section {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, #FFF5F0 0%, #FFE5D9 50%, #FFD4E0 100%);
    position: relative;
    overflow: hidden;
    will-change: background-position;
  }

  .hero-section::before {
    content: '♡';
    position: absolute;
    font-size: 15rem;
    color: rgba(255, 200, 220, 0.1);
    top: 10%;
    left: 10%;
    animation: float 6s ease-in-out infinite;
  }

  .hero-section::after {
    content: '♡';
    position: absolute;
    font-size: 12rem;
    color: rgba(255, 180, 200, 0.1);
    bottom: 15%;
    right: 15%;
    animation: float 8s ease-in-out infinite;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0) rotate(0deg); }
    50% { transform: translateY(-20px) rotate(5deg); }
  }

  .hero-content {
    text-align: center;
    z-index: 10;
    position: relative;
  }

  .hero-title {
    font-family: 'Gilda Display', serif;
    font-size: clamp(4rem, 15vw, 10rem);
    font-weight: 400;
    letter-spacing: 0.02em;
    margin: 0;
    text-transform: none;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .hero-word {
    display: inline-block;
    padding: 20px 50px;
    background: rgba(255, 255, 255, 0.8);
    backdrop-filter: blur(10px);
    border-radius: 30px;
    box-shadow: 0 10px 40px rgba(255, 180, 200, 0.3);
    will-change: transform;
    transition: transform 0.3s ease;
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

  /* QUESTION SECTION */
    transform: translateY(-5px);
    box-shadow: 0 20px 60px rgba(255, 150, 180, 0.2);
  }

  .photo-frame img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    filter: saturate(1.1) brightness(1.05);
    transition: transform 0.3s ease;
  }

  .photo-frame:hover img {
    transform: scale(1.05);
  }

  .photo-border {
    position: absolute;
    inset: 15px;
    border: 2px solid rgba(255, 255, 255, 0.5);
    border-radius: 12px;
    pointer-events: none;
  }

  .photo-number {
    position: absolute;
    bottom: 15px;
    right: 15px;
    font-size: 1.2rem;
    font-weight: 600;
    background: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(10px);
    color: var(--rose);
    padding: 8px 16px;
    border-radius: 20px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  }

  /* FLOATING TEXT */
  .floating-text {
    position: absolute;
    font-family: 'Gilda Display', serif;
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 400;
    color: rgba(255, 180, 200, 0.15);
    text-transform: lowercase;
    letter-spacing: 0.05em;
    pointer-events: none;
    white-space: nowrap;
    will-change: transform;
  }

  .floating-text-0 {
    top: 20%;
    left: -10%;
    transform: rotate(-15deg);
  }

  .floating-text-1 {
    top: 40%;
    right: -10%;
    transform: rotate(10deg);
  }

  .floating-text-2 {
    top: 60%;
    left: -5%;
    transform: rotate(-8deg);
  }

  .floating-text-3 {
    top: 80%;
    right: -8%;
    transform: rotate(12deg);
  }

  .floating-text-4 {
    top: 95%;
    left: 5%;
    transform: rotate(-10deg);
  }

  .memory-count {
    position: sticky;
    top: 20px;
    left: 20px;
    z-index: 100;
    display: flex;
    justify-content: flex-start;
    padding: 20px;
  }

  .count-box {
    display: flex;
    flex-direction: column;
    gap: 10px;
    background: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(10px);
    color: var(--text);
    padding: 20px 30px;
    border-radius: 25px;
    box-shadow: 0 10px 30px rgba(255, 150, 180, 0.2);
  }

  .count-label {
    font-size: 0.9rem;
    letter-spacing: 0.15em;
    color: var(--soft-brown);
  }

  .count-number {
    font-size: 3rem;
    font-weight: 700;
    color: var(--rose);
  }

  /* QUESTION SECTION */
  .question-section {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, #FFFBF5 0%, #FFF5F0 100%);
    padding: 100px 20px;
  }

  .question-wrapper {
    max-width: 1000px;
    width: 100%;
  }

  .question-container {
    background: white;
    padding: 80px;
    border-radius: 40px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.08);
    position: relative;
  }

  .question-container::before {
    content: '♡';
    position: absolute;
    top: 30px;
    right: 30px;
    font-size: 3rem;
    color: rgba(255, 180, 200, 0.2);
  }

  .question-header {
    display: flex;
    align-items: center;
    gap: 20px;
    margin-bottom: 50px;
    justify-content: center;
  }

  .header-line {
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--soft-pink), transparent);
    max-width: 150px;
  }

  .header-text {
    font-size: 0.9rem;
    font-weight: 600;
    letter-spacing: 0.2em;
    color: var(--soft-brown);
    text-transform: uppercase;
  }

  .big-question {
    font-family: 'Gilda Display', serif;
    font-size: clamp(2rem, 5vw, 4.5rem);
    font-weight: 400;
    margin-bottom: 40px;
    line-height: 1.4;
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
    justify-content: center;
    color: var(--text);
  }

  .question-word {
    display: inline-block;
  }

  .question-word.highlight {
    color: var(--rose);
    position: relative;
  }

  .question-word.highlight::after {
    content: '';
    position: absolute;
    bottom: 5px;
    left: 0;
    right: 0;
    height: 3px;
    background: linear-gradient(90deg, var(--soft-pink), var(--rose));
    border-radius: 2px;
  }

  .hint-text {
    margin-top: 30px;
    font-size: 1.2rem;
    font-weight: 500;
    letter-spacing: 0.1em;
    color: var(--soft-brown);
  }

  .buttons {
    display: flex;
    gap: 40px;
    justify-content: center;
    margin-top: 60px;
  }

  .btn {
    position: relative;
    font-family: 'Quicksand', sans-serif;
    font-size: 2rem;
    font-weight: 600;
    padding: 25px 60px;
    border: none;
    background: white;
    cursor: pointer;
    text-transform: none;
    border-radius: 50px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    transition: all 0.3s ease;
  }

  .btn span {
    display: block;
    position: relative;
    z-index: 1;
  }

  .btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 15px 40px rgba(255, 150, 180, 0.25);
  }

  .btn:active {
    transform: translateY(-1px);
  }

  .btn-yes {
    background: linear-gradient(135deg, var(--soft-pink) 0%, var(--rose) 100%);
    color: white;
  }

  .btn-yes:hover {
    background: linear-gradient(135deg, var(--rose) 0%, var(--blush) 100%);
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
    0% {
      left: -100%;
    }
    50%, 100% {
      left: 200%;
    }
  }

  .btn-no {
    background: white;
    color: var(--text);
    border: 2px solid var(--gray);
  }

  .btn-no:hover {
    background: var(--gray);
  }

  .btn-border {
    display: none;
  }

  /* ANSWER SECTION */
  .answer-container {
    margin-top: 60px;
    position: relative;
  }

  .answer-stamp {
    position: absolute;
    top: -40px;
    right: -40px;
    width: 180px;
    height: 180px;
    border: 4px solid var(--rose);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    transform: rotate(15deg);
    background: white;
    box-shadow: 0 10px 30px rgba(255, 150, 180, 0.2);
  }

  .answer-stamp::before {
    content: '';
    position: absolute;
    inset: 12px;
    border: 2px dashed var(--soft-pink);
    border-radius: 50%;
  }

  .stamp-text {
    font-size: 1.5rem;
    font-weight: 600;
    color: var(--rose);
    letter-spacing: 0.2em;
    transform: rotate(-15deg);
  }

  .answer-box {
    margin: 40px 0;
    position: relative;
  }

  .answer-border-top,
  .answer-border-bottom {
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--soft-pink), transparent);
    margin: 30px 0;
  }

  .success-text {
    font-family: 'Gilda Display', serif;
    font-size: clamp(3rem, 8vw, 6rem);
    color: var(--text);
    margin: 0;
    text-align: center;
    display: flex;
    flex-direction: column;
    gap: 15px;
    font-weight: 400;
  }

  .success-line {
    display: block;
  }

  .success-line.highlight {
    color: var(--rose);
    font-size: 1.1em;
  }

  .hearts-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    margin: 50px 0;
    justify-items: center;
  }

  .heart {
    font-size: 3rem;
    color: var(--rose);
    background: rgba(255, 212, 224, 0.2);
    width: 80px;
    height: 80px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    animation: heartBeat 2s infinite;
  }

  @keyframes heartBeat {
    0%, 100% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.1);
    }
  }

  .date-container {
    text-align: center;
    margin: 50px 0;
  }

  .date-label {
    font-size: 0.9rem;
    font-weight: 600;
    letter-spacing: 0.2em;
    margin-bottom: 15px;
    color: var(--soft-brown);
    text-transform: uppercase;
  }

  .date-box {
    font-family: 'Gilda Display', serif;
    font-size: 3rem;
    font-weight: 400;
    padding: 20px 40px;
    background: linear-gradient(135deg, var(--soft-pink) 0%, var(--blush) 100%);
    color: white;
    display: inline-block;
    border-radius: 25px;
    box-shadow: 0 10px 30px rgba(255, 150, 180, 0.3);
    letter-spacing: 0.1em;
  }

  .final-messages {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-top: 50px;
  }

  .message-box {
    padding: 30px;
    background: rgba(255, 245, 240, 0.5);
    border-radius: 20px;
    border: 1px solid rgba(255, 180, 200, 0.2);
    position: relative;
  }

  .message-box::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 4px;
    height: 100%;
    background: linear-gradient(180deg, var(--soft-pink), var(--rose));
    border-radius: 20px 0 0 20px;
  }

  .message-number {
    font-size: 0.8rem;
    font-weight: 600;
    color: var(--soft-brown);
    display: inline-block;
    margin-bottom: 10px;
    letter-spacing: 0.1em;
  }

  .message-box p {
    font-size: 1.5rem;
    font-weight: 500;
    margin: 0;
    letter-spacing: 0.02em;
    color: var(--text);
  }

  .message-box.signature {
    background: linear-gradient(135deg, var(--soft-pink) 0%, var(--blush) 100%);
    color: white;
    text-align: right;
    border: none;
  }

  .message-box.signature::before {
    display: none;
  }

  .message-box.signature p {
    color: white;
    font-family: 'Gilda Display', serif;
    font-size: 1.8rem;
  }

  /* RESPONSIVE */
  @media (max-width: 768px) {
    .hero-word {
      padding: 15px 35px;
      border-radius: 20px;
    }

    .photos-grid {
      grid-template-columns: 1fr;
      gap: 60px 20px;
      padding: 20px;
    }

    .photo-frame {
      border-radius: 15px;
    }

    .photo-number {
      font-size: 1rem;
      padding: 6px 14px;
    }

    .floating-text {
      font-size: 1.5rem;
    }

    .count-box {
      padding: 15px 20px;
    }

    .count-number {
      font-size: 2rem;
    }

    .question-container {
      padding: 40px 20px;
      border-radius: 30px;
    }

    .question-header {
      gap: 10px;
      margin-bottom: 30px;
    }

    .header-line {
      max-width: 40px;
    }

    .header-text {
      font-size: 0.7rem;
    }

    .big-question {
      font-size: 1.8rem;
      gap: 10px;
      margin-bottom: 30px;
    }

    .hint-text {
      font-size: 1rem;
      margin-top: 20px;
    }

    .buttons {
      flex-direction: column;
      gap: 15px;
      margin-top: 40px;
    }

    .btn {
      font-size: 1.5rem;
      padding: 20px 40px;
    }

    .answer-stamp {
      width: 120px;
      height: 120px;
      border: 3px solid var(--rose);
      top: -30px;
      right: -20px;
    }

    .stamp-text {
      font-size: 1rem;
    }

    .success-text {
      font-size: 2rem;
      gap: 10px;
    }

    .hearts-grid {
      gap: 10px;
      margin: 30px 0;
    }

    .heart {
      font-size: 2rem;
      width: 60px;
      height: 60px;
    }

    .date-label {
      font-size: 0.8rem;
      padding: 8px;
    }

    .date-box {
      font-size: 1.8rem;
      padding: 15px 25px;
      border-radius: 20px;
    }

    .message-box {
      padding: 20px;
      border-radius: 15px;
    }

    .message-box p {
      font-size: 1.1rem;
    }
  }
</style>
