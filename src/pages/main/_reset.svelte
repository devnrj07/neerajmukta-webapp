<script>
  import { TabsTransition } from "@sveltech/routify/decorators";
  import { writable } from "svelte/store";
  import BottomNav from "./_components/BottomNav.svelte";
  import { url, isActive } from "@sveltech/routify";

  const width = writable();
  const color = writable();
  const _urls = [
    ["./fun", "Fun", "#7fc5bb"],
    ["./about", "About Me", "#0bf5cc"],
    ["./home", "Home", "#7fc5bb"],
    ["./resume", "Resume", "#88f0d0"],
    ["./hireMe", "Hire Me", "#a1fac3"],
  ];
  $: urls = _urls.map(([path, name, color]) => ({
    name,
    href: $url(path),
    color,
    active: !!$isActive(path)
  }));
  
  $: urlOrder = urls.map(({href}) => href);
</script>

<style>
  :global(body) {
    padding: 0;
  }
  * :global(.inset) {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }
  main.inset {
    bottom: 64px;
    overflow: hidden;
  }

  * :global(*) {
    text-align: center;
  }
  
</style>

<div style="height: 100%">

  <main class="inset" bind:offsetWidth={$width}>
    <slot decorator={TabsTransition} scoped={{ width, urls: urlOrder }} />
  </main>
  <BottomNav {urls} height="64px" />
</div>

<!-- routify:options bundle=true -->