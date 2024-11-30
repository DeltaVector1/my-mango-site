<script>
  import Main from './Main.svelte';
  import { onMount } from 'svelte';
  // Function to check and apply the saved theme
  onMount(() => {
    const darkMode = localStorage.getItem('darkMode') === 'true';
    if (darkMode) {
      document.body.classList.add('dark-mode');
    }
  });
</script>
<Main />
<style>
  /* App level styles if any */
</style>
