<script lang="ts">
  import svelteLogo from './assets/svelte.svg'
  import Navbar from './lib/Navbar.svelte'
  import { Router, Link, Route } from "svelte-navigator";
  import Home from "./pages/Home.svelte";
  import Player from "./pages/Player.svelte";
  import Request from "./pages/Request.svelte";

</script>



<svelte:head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css">
</svelte:head>
          

    <Navbar />




<Router>
  <!-- <nav>
    <Link to="/">Home</Link>
    <Link to="about">About</Link>
    <Link to="blog">Blog</Link>
  </nav> -->
  <div>
    <Route path="/">
      <Home />
    </Route>
    <Route path="/request">
      <Request />
    </Route>
    <Route path="player/:id" let:params>
      <Player id={params.id} />
    </Route>
  </div>
</Router>
