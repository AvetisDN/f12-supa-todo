<script>
  import { user } from "$lib/store.js";
  import supabase from "$lib/db.js";
  import { goto } from "$app/navigation";

  let email = "";
  let password = "";

  const signUp = async () => {
    let { user: supaUser, error } = await supabase.auth.signIn({
      email,
      password,
    });
    $user = supaUser;
    goto("/");
  };
</script>

<div>
  <div class="flex items-center gap-4 my-3">
    <label for="email" class="absolute left-8 text-xl opacity-50">
      <i class="fa-light fa-envelope" />
    </label>
    <input
      type="email"
      id="email"
      required
      placeholder="E-Mail"
      class="input input-bordered w-full pl-12"
      bind:value={email}
    />
  </div>
  <div class="flex items-center gap-4 my-3">
    <label for="pass" class="absolute left-8 text-xl opacity-50">
      <i class="fa-light fa-key" />
    </label>
    <input
      type="password"
      id="pass"
      placeholder="Password"
      class="input input-bordered w-full pl-12"
      bind:value={password}
    />
  </div>
  <button
    class="btn btn-secondary gap-2 w-full"
    disabled={email.length < 6 || password.length < 6}
    on:click={signUp}
  >
    Login
    <i class="fa-regular fa-right-to-bracket" />
  </button>
</div>
