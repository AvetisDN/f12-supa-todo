<script>
  import "../app.css";
  import Navbar from "$lib/Navbar.svelte";
</script>

<div
  class="bg-base-300 w-full min-h-screen p-4 flex flex-col items-center gap-4"
>
  <Navbar />
  <slot />
</div>
