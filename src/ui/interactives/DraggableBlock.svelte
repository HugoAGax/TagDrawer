<script lang="ts">
  import { onMount } from "svelte";

  export let posX: number = 100;
  export let posY: number = 100;
  export let height: number = 0;
  export let width: number = 0;

  let anchor = {
    x: 0,
    y: 0
  }


  let draggable: HTMLDivElement;
  let isDragging: boolean = false;

  export const updatePosition = (x: number, y: number) => {
    draggable.style.left = x + "px";
    draggable.style.top = y + "px";
  };

  export const getSize = () => {
    return {
      width: parseInt(draggable.style.width),
      height: parseInt(draggable.style.width)
    }
  };

  const getPos = (element: HTMLDivElement) => {
    return {
      x: parseInt(element.style.left),
      y: parseInt(element.style.top)
    }
  };

  export const updateSize = (width: number, height: number) => {
    draggable.style.width = Math.abs(width) + "px";
    draggable.style.height = Math.abs(height) + "px";
  };

  onMount(() => {
    updatePosition(posX, posY);
    updateSize(width, height);

    anchor.x = posX;
    anchor.y = posY;

    draggable.addEventListener("mousedown", (e) => {
      e.stopPropagation();
      document.addEventListener("mouseup", onMouseUp);
      function onMouseUp() {
        document.removeEventListener("mouseup", onMouseUp);
      }
    });
  });
</script>

<div style="left: {posX};"  bind:this={draggable}>
  <input value="(none)">
</div>

<style>
  div {
    border: 1px solid blue;
    background-color: rgba(0,0,255,0.1);
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  input {
    border: none;
    text-align: center;
    width: calc(100% - 10px);
    background-color: transparent;
  }
</style>
