<script lang="ts">
  import { Link, Route, Router } from "svelte-navigator";
  import Home from "./pages/Home.svelte";
  import Playground from "./pages/Playground.svelte";

  let title: string = "Mario Vieilledent";
</script>

<svelte:head>
  <title>{title}</title>
</svelte:head>

<div class="all fc">
  <Router>
    <nav class="navbar f">
      <Link to=""><span class="link f">Home</span></Link>
      <Link to="playground"><span class="link f">Playground</span></Link>
    </nav>
    <div class="content fc">
      <Route path="/"><Home /></Route>
      <Route path="home"><Home /></Route>
      <Route path="playground"><Playground /></Route>
    </div>
  </Router>
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
        margin: 0px 12px;
        align-items: center;
      }
    }

    .content {
      width: 100%;
      height: calc(100% - $navbar-height);
      overflow: auto;
    }
  }
</style>
