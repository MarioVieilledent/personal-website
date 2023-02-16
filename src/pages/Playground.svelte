<script lang="ts">
  import { useFocus } from "svelte-navigator";

  const initialTemplate: string = `<div style="background-color: #555;">
  <h1>Heya</h1>
</div>`;

  let input: string = initialTemplate;

  const registerFocus = useFocus();
</script>

<div class="container f">
  <div class="left">
    <textarea class="textarea" bind:value={input} />
  </div>
  <div class="right">{@html input}</div>
</div>

<style lang="scss">
  .container {
    width: 100%;
    height: calc(100vh - 48px);

    .left,
    .right {
      width: 50%;
      height: 100%;
    }

    .left {
      .textarea {
        box-sizing: border-box;
        border: none;
        width: 100%;
        height: 100%;
        resize: none;
      }
    }
  }
</style>
