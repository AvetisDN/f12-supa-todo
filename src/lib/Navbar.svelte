<script>
  import supabase from "$lib/db.js";
  import { user } from "$lib/store.js";
  import { goto } from "$app/navigation";

  const signOut = async () => {
    let { error } = await supabase.auth.signOut();
    $user = null;
    goto("/auth");
  };
</script>

<div class="navbar bg-primary text-primary-content shadow-xl">
  <div class="flex-1">
    <a href="/" class="btn btn-ghost normal-case text-xl">TodoApp</a>
  </div>
  <div class="flex-none flex gap-3">
    {#if !$user}
      <ul class="menu menu-horizontal p-0">
        <li>
          <a href="/auth">Sign In</a>
        </li>
      </ul>
    {/if}
    {#if $user}
      <span class="font-semibold">{$user.email}</span>
      <button class="btn btn-error gap-2" on:click={signOut}>
        Log Out
        <i class="fa-light fa-right-from-bracket" />
      </button>
    {/if}
  </div>
</div>
