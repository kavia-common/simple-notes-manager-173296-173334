<script lang="ts">
  import '../app.css';
  import '$lib/theme.css';
  let { children } = $props();
</script>

<div class="app-root">
  {@render children()}
</div>

<style>
  .app-root {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
</style>
