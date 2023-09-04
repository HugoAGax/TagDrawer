<script lang="ts">
  import type { LoadEvent } from "@sveltejs/kit";
  import { createEventDispatcher } from "svelte";
  import type { FormEventHandler } from "svelte/elements";
  let state: boolean = false;

  const dispatch = createEventDispatcher();
  const _dispatchFileData = (e: Event) => {
    dispatch("filedrop", e);
  };

  const handleDragOver = (e: Event) => {
    e.preventDefault();
    e.stopPropagation();
    state = true;
  };

  const handleDragExit = (e: Event) => {
    e.preventDefault();
    e.stopPropagation();
    state = false;
  };

  const handleDragEnter = (e: Event) => {
    e.preventDefault();
    e.stopPropagation();
    state = true;
  };

  const handleDrop = (e: Event) => {
    e.preventDefault();
    e.stopPropagation();
    _dispatchFileData(e.dataTransfer.files);
    state = false;
  };
</script>

<div
  id="dropContainer"
  class="drop-container {state === true ? 'active' : ''}"
  on:dragenter={handleDragEnter}
  on:dragleave={handleDragExit}
  on:dragover={handleDragOver}
  on:drop={handleDrop}
>
  <slot />
</div>

<style>
  .drop-container {
    height: calc(100% - 2rem);
    border: 1px dashed #c084fc;
    border-radius: 0.5rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 1rem;
    color: #c084fc;
    line-height: 1.3;
    transition: 0.3s;
  }

  .drop-container.active {
    background-color: #a855f7;
  }
</style>
