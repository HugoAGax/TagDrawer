<script lang="ts">
  import DraggableBlock from "./interactives/DraggableBlock.svelte";
  import { onMount } from "svelte";
  let interactiveElementInstance;
  let interactionCanvas: HTMLDivElement;

  interface Coordinate {
    x: number;
    y: number;
  }

  interface Bounds {
    yStart: number;
    yEnd: number;
    xStart: number;
    xEnd: number;
  }

  interface Size {
    width: number;
    height: number;
  }

  interface Diff {
    dX: number,
    dY: number
  }

  const _offsetForFrame = (event: MouseEvent, frame: HTMLDivElement) => {
    let origin = frame.getBoundingClientRect();

    let output: Coordinate = {
      x: event.x - origin.left,
      y: event.y - origin.top,
    };

    return output;
  };

  const _calcCoordDiff = (initial: Coordinate, final: Coordinate) => {
    let output : Diff = {
      dX: final.x - initial.x,
      dY: final.y - initial.y,
    };

    return output;
  };

  const _createNewBlock = (coords: Coordinate, size: Size, bounds: Bounds) => {
    return new DraggableBlock({
      target: interactionCanvas,
      props: {
        posX: coords.x,
        posY: coords.y,
      },
    });
  };

  const _getFrameBounds = (frame: HTMLDivElement) => {
    let origin = frame.getBoundingClientRect();
    let bounds: Bounds = {
      yStart: origin.top,
      yEnd: origin.bottom,
      xStart: origin.left,
      xEnd: origin.right,
    };
    return bounds;
  };

  const _calcBlockSizing = (frame: HTMLDivElement, ) => {

  }

  const _calcBlockPosition = () => {

  }

  onMount(() => {
    interactionCanvas.addEventListener("mousedown", (e) => {
      let initialClick = _offsetForFrame(e, interactionCanvas);
      let diff: Diff;
      let sizing = {
        width: 0,
        height: 0,
      };

      let newBlock = _createNewBlock(
        initialClick,
        sizing,
        _getFrameBounds(interactionCanvas)
      );

      document.addEventListener("mouseup", onMouseUp);
      document.addEventListener("mousemove", onMouseMove);

      function onMouseMove(evt: MouseEvent) {
        diff = _calcCoordDiff(initialClick, _offsetForFrame(evt, interactionCanvas));
        let xComp = initialClick.x;
        let yComp = initialClick.y;
        
        newBlock.updateSize(diff.dX, diff.dY);

        if (diff.dX < 0) {
          xComp = initialClick.x + diff.dX;
        }

        if (diff.dY < 0) {
          yComp = initialClick.y + diff.dY;
        }
        newBlock.updatePosition(xComp, yComp);
      }

      function onMouseUp() {
        if (Math.abs(diff.dY) < 11 || Math.abs(diff.dX) < 11) {
          newBlock.$destroy();
        }
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
