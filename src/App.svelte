<script lang="ts">
  import Countries from "./pages/Countries.svelte";
  import Playground from "./pages/Playground.svelte";
  import Home from "./pages/Home.svelte";

  const pages = ["Home", "Playground", "Countries"];
  const lsPage = "MarioVieilledentPersonalWebsitePage";
  let title: string = "Mario Vieilledent";

  let currentPage: Page = window.localStorage.getItem(lsPage) as Page;
  !currentPage ? (currentPage = "Countries") : {};

  function setPage(page: string) {
    currentPage = page as Page;
    window.localStorage.setItem(lsPage, page);
  }

  type Page = "Home" | "Playground" | "Countries";
</script>

<svelte:head>
  <title>{title}</title>
</svelte:head>

<div class="all fc">
  <div class="navbar f">
    {#each pages as page}
      <button
        class={currentPage === page ? "link curr f" : "link f"}
        on:click={() => setPage(page)}
        >{page}
      </button>
    {/each}
  </div>
  <div class="content fc">
    {#if currentPage === "Home"}
      <Home />
    {/if}
    {#if currentPage === "Playground"}
      <Playground />
    {/if}
    {#if currentPage === "Countries"}
      <Countries />
    {/if}
  </div>
</div>

<style lang="scss">
  $navbar-height: 36px;

  .all {
    width: 100%;
    height: 100%;

    .navbar {
      width: 100%;
      height: $navbar-height;
      background-color: #454545;

      .link {
        height: 100%;
        padding: 0px 12px;
        align-items: center;
        color: #eee;
        cursor: pointer;
        background-color: transparent;
        border: none;
        transition: background-color 0.2s;
      }

      .link:hover {
        background-color: #606060;
      }

      .curr {
        background-color: #565656;
      }
    }

    .content {
      width: 100%;
      height: calc(100% - $navbar-height);
      overflow: auto;
    }
  }
</style>
