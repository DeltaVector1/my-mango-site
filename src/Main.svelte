<script>
    import { fade, fly } from 'svelte/transition';
    import { Fa } from 'svelte-fa';
    import { onMount } from 'svelte';
    import {
      faDiscord,
      faTwitter,
    } from '@fortawesome/free-brands-svg-icons';
    import {
      faSun,
      faMoon
    } from '@fortawesome/free-solid-svg-icons';
    let darkMode = false;
    let showDialogue = true;
    let audio;
    let isPlaying = false;
    let volume = 0.3;
    onMount(() => {
      audio = new Audio('/music/tape.mp3'); // Put your music file in public/music/
      audio.loop = true;
      audio.volume = volume;
    });
    function toggleDarkMode() {
      darkMode = !darkMode;
      showDialogue = false;
      localStorage.setItem('darkMode', darkMode);
      document.body.classList.toggle('dark-mode', darkMode);
    }
    function toggleMusic() {
      if (!audio) return;
      if (isPlaying) {
        audio.pause();
      } else {
        audio.play().catch(e => {
          console.log('Audio playback failed:', e);
        });
      }
      isPlaying = !isPlaying;
    }
    function updateVolume(e) {
      if (audio) {
        volume = e.target.value;
        audio.volume = volume;
      }
    }
    const socials = [
      {
        name: 'Discord',
        url: 'https://discord.com/users/sweetmango78',
        icon: faDiscord,
        type: 'fa'
      },
      {
        name: 'HuggingFace',
        url: 'https://huggingface.co/Delta-Vector',
        imagePath: '/images/hf-icon.png',
        type: 'image'
      },
      {
        name: 'Twitter',
        url: 'https://x.com/MangoSweet78',
        icon: faTwitter,
        type: 'fa'
      },
      {
        name: 'CharHub',
        url: 'https://chub.ai/users/MangyMango',
        imagePath: '/images/charhub-icon.png',
        type: 'image'
      },
      {
        name: 'Personal Site',
        url: 'https://mango.sillytilly.org',
        imagePath: '/images/mango-icon.png',
        type: 'image'
      }
    ];
  </script>
  <section class="container">
    <div class="header" in:fade={{ duration: 1000 }}>
      <h1 class="title">Mangy Mango's HQ</h1>
      <p class="subtitle">Doing alot of stuff - I have too many things; Send help </p>
      <div class="mascot-container">
        <Fa
          icon={darkMode ? faMoon : faSun}
          class="theme-icon"
        />
        <img
          src="/images/mascot.png"
          alt="Mascot"
          class="mascot-image"
          on:click={toggleDarkMode}
        />
        {#if showDialogue}
          <div class="dialogue-box" in:fade={{ duration: 300 }}>
            Click me Aesir! 
          </div>
        {/if}
      </div>
    </div>
    <div class="button-container">
      <div class="button-wrapper">
        {#each socials as social (social.name)}
          <a
            href={social.url}
            class="icon-button"
            target="_blank"
            rel="noopener noreferrer"
            title={social.name}
            in:fly={{ y: 50, duration: 500, delay: 300 }}
          >
            {#if social.type === 'fa'}
              <Fa icon={social.icon} size="2x" />
            {:else if social.type === 'image'}
              <img
                src={social.imagePath}
                alt={social.name}
                class="custom-icon"
              />
            {/if}
          </a>
        {/each}
      </div>
    </div>
    <div class="audio-controls">
      <button class="music-toggle" on:click={toggleMusic}>
        {#if isPlaying}
          🔊
        {:else}
          🔇
        {/if}
      </button>
      <input
        type="range"
        min="0"
        max="1"
        step="0.1"
        value={volume}
        on:input={updateVolume}
        class="volume-slider"
      />
    </div>
  </section>
  <style>
    .container {
      text-align: center;
      padding: 50px 20px;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    .header {
      position: relative;
      margin-bottom: 2rem;
    }
    .title {
      font-size: 3em;
      margin: 0;
      margin-bottom: 0.5rem;
    }
    .subtitle {
      font-size: 1.2em;
      margin-bottom: 2rem;
      color: var(--text-color);
      opacity: 0.8;
      font-weight: 300;
    }
    .mascot-container {
      position: relative;
      width: 200px;
      margin: 2rem auto;
      cursor: pointer;
      transition: transform 0.3s ease;
    }
    .mascot-container:hover {
      transform: scale(1.05);
    }
    .mascot-image {
      width: 150px;
      height: auto;
      display: block;
      margin: 0 auto;
    }
    .theme-icon {
      position: absolute;
      top: -20px;
      left: 50%;
      transform: translateX(-50%);
      font-size: 24px;
      color: var(--text-color);
    }
    .dialogue-box {
      position: absolute;
      background: var(--card-bg);
      border: 2px solid var(--accent-color);
      border-radius: 15px;
      padding: 10px 15px;
      top: -60px;
      right: -20px;
      font-size: 0.9em;
      white-space: nowrap;
      filter: drop-shadow(0 2px 4px rgba(0,0,0,0.1));
    }
    .dialogue-box::after {
      content: '';
      position: absolute;
      bottom: -10px;
      left: 50%;
      transform: translateX(-50%);
      border-left: 10px solid transparent;
      border-right: 10px solid transparent;
      border-top: 10px solid var(--accent-color);
    }
    .button-container {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-top: 2rem;
    }
    .button-wrapper {
      background-color: var(--card-bg);
      padding: 20px;
      border-radius: 20px;
      display: flex;
      gap: 30px;
      justify-content: center;
      align-items: center;
      border: 2px solid var(--accent-color);
    }
    .icon-button {
      background-color: var(--button-bg);
      color: var(--button-text);
      width: 60px;
      height: 60px;
      border-radius: 15px;
      display: flex;
      align-items: center;
      justify-content: center;
      text-decoration: none;
      transition: transform 0.3s, background-color 0.3s;
    }
    .icon-button:hover {
      background-color: var(--button-hover-bg);
      transform: translateY(-5px);
    }
    .custom-icon {
      width: 32px;
      height: 32px;
      object-fit: contain;
    }
    .audio-controls {
      position: fixed;
      bottom: 20px;
      left: 20px;
      display: flex;
      align-items: center;
      gap: 10px;
      z-index: 1000;
      background: var(--card-bg);
      padding: 10px;
      border-radius: 20px;
      opacity: 0.7;
      transition: opacity 0.3s ease;
    }
    .audio-controls:hover {
      opacity: 1;
    }
    .volume-slider {
      width: 100px;
      height: 4px;
      -webkit-appearance: none;
      background: var(--accent-color);
      border-radius: 2px;
      outline: none;
    }
    .volume-slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 15px;
      height: 15px;
      border-radius: 50%;
      background: var(--button-bg);
      cursor: pointer;
    }
    @media (max-width: 600px) {
    .title {
      font-size: 2em;
    }
    .subtitle {
      font-size: 1em;
    }
    .icon-button {
      width: 50px;
      height: 50px;
    }
    .button-wrapper {
      gap: 20px;
      padding: 15px;
    }
    .custom-icon {
      width: 24px;
      height: 24px;
    }
    .mascot-image {
      width: 120px;
    }
  }
  .music-toggle {
    background-color: var(--button-bg);
    border: 2px solid var(--accent-color);
    border-radius: 50%;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: transform 0.3s ease;
  }
  .music-toggle:hover {
    transform: scale(1.1);
    background-color: var(--button-hover-bg);
  }
  .volume-slider::-moz-range-thumb {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background: var(--button-bg);
    cursor: pointer;
    border: none;
  }
  .volume-slider::-ms-thumb {
    width: 15px;
    height: 15px;
    border-radius: 50%;
    background: var(--button-bg);
    cursor: pointer;
  }
</style>