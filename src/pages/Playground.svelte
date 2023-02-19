<script lang="ts">
  const initialTemplate: string = `<div style="display: flex; flex-direction: column; align-items: center">
  <h1>Title</h1>
  <button onclick="alert('Javascript is working')">Click me</button>
  <input type="date">
  <img src="./flags/fr.svg" width="144" />
</div>`;

  let input: string = initialTemplate;
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

      .textarea:focus {
        outline: none;
      }
    }
  }

  @media (max-width: 700px) {
    .container {
      flex-direction: column;

      .left,
      .right {
        width: 100%;
      }
    }
  }
</style>
