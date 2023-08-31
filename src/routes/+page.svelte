<script>
  import RenderCanvas from "../ui/RenderCanvas.svelte";
  import InteractionFrame from "../ui/InteractionFrame.svelte";
</script>

<main id="">
  <div class="canvas-container">
    <RenderCanvas />
    <InteractionFrame />
  </div>
</main>

<style>
  main {
    width: 100%;
    display: flex;
    justify-content: center;
  }

  .canvas-container {
    position: relative;
  }
</style>
