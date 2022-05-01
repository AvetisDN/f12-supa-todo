<script>
  import SignIn from "$lib/auth/SignIn.svelte";
  import SignUp from "$lib/auth/SignUp.svelte";
  import { fly } from "svelte/transition";
  let newUser = true;
</script>

<div class="bg-base-100 p-4 shadow-lg w-full max-w-md">
  <div class="flex justify-center">
    <button
      class="btn btn-ghost"
      class:bg-base-200={!newUser}
      on:click={() => (newUser = !newUser)}
    >
      Sign In
    </button>
    <button
      class="btn btn-ghost"
      class:bg-base-200={newUser}
      on:click={() => (newUser = !newUser)}
    >
      Sign Up
    </button>
  </div>
</div>
<div class="relative w-full max-w-md">
  {#if !newUser}
    <div
      class="bg-base-100 p-4 shadow-lg w-full absolute"
      in:fly={{ duration: 300, x: -50 }}
      out:fly={{ duration: 300, x: -50 }}
    >
      <SignIn />
    </div>
  {:else}
    <div
      class="bg-base-100 p-4 shadow-lg w-full absolute"
      in:fly={{ duration: 300, x: 50 }}
      out:fly={{ duration: 300, x: 50 }}
    >
      <SignUp />
    </div>
  {/if}
</div>
