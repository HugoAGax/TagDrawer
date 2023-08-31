<script lang="ts">
  import { onMount } from "svelte";

  export let posX: number = 100;
  export let posY: number = 100;
  export let height: number = 0;
  export let width: number = 0;


  let draggable: HTMLDivElement;
  let isDragging: boolean = false;

  const updatePosition = (element: HTMLDivElement, x: number, y: number) => {
    element.style.left = x + "px";
    element.style.top = y + "px";
  };

  const getSize = (element: HTMLDivElement) => {
    return {
      width: parseInt(element.style.width),
      height: parseInt(element.style.width)
    }
  };

  const getPos = (element: HTMLDivElement) => {
    return {
      x: parseInt(element.style.left),
      y: parseInt(element.style.top)
    }
  };

  export const updateSize = (width: number, height: number) => {
    let initialSize = getSize(draggable);
    let initialPos = getPos(draggable);
    let xComp = 0;
    let yComp = 0;

    if (width < 0) {
      xComp = initialSize.width - width;
      console.log('X COMP', xComp);
    }
    
    if (height < 0) {
      yComp = initialSize.height - height;
      console.log('Y COMP', yComp);
    }
    draggable.style.width = Math.abs(width) + "px";
    draggable.style.height = Math.abs(height) + "px";
    updatePosition(draggable, initialPos.x - xComp, initialPos.y - yComp)
  };

  onMount(() => {
    updatePosition(draggable, posX, posY);
    updateSize(width, height);

    draggable.addEventListener("mousedown", (e) => {
      e.stopPropagation();
      // e.preventDefault();
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
    height: calc(100% - 10px);
    background-color: transparent;
  }
</style>
