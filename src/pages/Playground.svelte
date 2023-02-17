<script lang="ts">
  import { useFocus } from "svelte-navigator";

  const registerFocus = useFocus();

  let textCode: string = "";
  let codeLines: any[] = ["<h1>", " test", "</h1>"];

  $: textCode = codeLines.join("");
</script>

<div class="playground f">
  <div class="left">
    {#each codeLines as line}
      <span class="code-line f" contenteditable="true">
        <pre class="code">{line}</pre>
      </span>
    {/each}
    {#each codeLines as line, index}
      <span class="before">{index}:</span>
    {/each}
  </div>
  <div class="right">{@html textCode}</div>
</div>

<style lang="scss">
  $before-width: 24px;

  .playground {
    width: 100%;
    height: calc(100vh - 48px);

    .left,
    .right {
      width: 50%;
      height: 100%;
    }

    .left {
      .before {
        font-family: "Courier New", Courier, monospace;
        font-size: 14px;
        color: #999;
        width: $before-width;
      }

      .code-line {
        .code {
          font-family: "Courier New", Courier, monospace;
          font-size: 14px;
          margin: 0;
          cursor: text;
          width: calc(100% - $before-width);
          word-wrap: break-word;
          white-space: pre-wrap;
          word-break: normal;
          outline: none;
        }

        .code:focus {
          outline: none;
        }
      }
    }
  }
</style>
