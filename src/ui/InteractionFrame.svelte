<script lang="ts">
  import DraggableBlock from "./interactives/DraggableBlock.svelte";
  import { onMount } from "svelte";
  let interactiveElementInstance;
  let interactionCanvas: HTMLDivElement;

  interface Coordinate {
    x: number;
    y: number;
  }

  interface Size {
    width: number;
    height: number;
  }

  const _offsetForFrame = (event: MouseEvent, frame: HTMLDivElement) => {
    let origin = frame.getBoundingClientRect();

    let output: Coordinate = {
      x: event.x - origin.left,
      y: event.y - origin.top,
    };

    return output;
  };

  const _calculateCoordDiff = (initial: Coordinate, final: Coordinate) => {
    return {
      diffX: final.x - initial.x,
      diffY: final.y - initial.y,
    };
  };

  const _createNewBlock = (coords: Coordinate, size: Size) => {
    return new DraggableBlock({
      target: interactionCanvas,
      props: {
        posX: coords.x,
        posY: coords.y,
        
      },
    });
  };

  onMount(() => {
    interactionCanvas.addEventListener("mousedown", (e) => {
      let initialClick = _offsetForFrame(e, interactionCanvas);
      let finalClick;
      let sizing = {
        width: 0,
        height: 0
      };

      let newBlock = _createNewBlock(initialClick, sizing);

      document.addEventListener("mouseup", onMouseUp);
      document.addEventListener("mousemove", onMouseMove);

      function onMouseMove(evt: MouseEvent) {
        finalClick = _offsetForFrame(evt, interactionCanvas);
        let diff = _calculateCoordDiff(initialClick, finalClick);
        sizing.width = diff.diffX;
        sizing.height = diff.diffY;
        newBlock.updateSize(sizing.width, sizing.height)
      }

      function onMouseUp() {
        newBlock.updateSize(sizing.width, sizing.height)
        document.removeEventListener("mouseup", onMouseUp);
        document.removeEventListener("mousemove", onMouseMove);
      }
    });
  });
</script>

<div class="" bind:this={interactionCanvas} />

<style>
  div {
    position: absolute;
    width: 800px;
    height: 600px;
    top: 0;
    left: 0;
  }
</style>
