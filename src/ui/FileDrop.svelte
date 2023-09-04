<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import DropArea from "./DropArea.svelte";

  const dispatch = createEventDispatcher();

  const _dispatchFileData = (name, data) => {
    dispatch("upload", {
      name: name,
      data: data,
    });
  };

  const onFileSelected = (e) => {
    let doc = e.target.files[0];
    readFile(doc);
  };

  const onFileDrop = (e) => readFile(e.detail[0]);

  const readFile = (doc) => {
    const fileread = new FileReader();
    fileread.onload = (evt: FileReaderEventMap) => {
      let jsonData : object;
      try {
        jsonData = JSON.parse(evt.target.result);
      } catch (e) {
        jsonData = {};
      }
      _dispatchFileData(doc["name"], jsonData);
    };
    fileread.readAsText(doc);
  };

</script>

<div class="input-container">
  <div>
    <DropArea on:filedrop={onFileDrop}>
      <img
        class="upload-icon"
        src="assets/cloud-arrow-up-solid.svg"
        alt="upload-icon"
      />
      <p>Drag and drop JSON files here or</p>
      <label for="json-input" class="custom-file-upload">
        upload from your computer
      </label>
      <input
        id="json-input"
        type="file"
        accept=".json, .txt"
        on:change={(e) => onFileSelected(e)}
      />
    </DropArea>
  </div>
</div>

<style>
  .input-container {
    background-color: #f3e8ff;
    padding: 1rem;
    border-radius: 0.75rem;
    display: flex;
    height: 20vh;
  }

  .input-container > div {
    width: 100%;
    margin: 0 12px;
  }

  .input-container > div:first-child {
    margin-left: 0;
  }

  .input-container > div:last-child {
    margin-right: 0;
  }

  .input-container input {
    display: none;
  }

  .upload-as-text {
    display: flex;
    flex-direction: column;
  }

  .custom-file-upload {
    color: #4f46e5;
    transition: 0.2s;
    cursor: pointer;
  }

  .custom-file-upload:hover {
    text-decoration: underline;
  }

  .upload-icon {
    height: 64px;
    width: auto;
    margin-bottom: 1rem;
  }
</style>
