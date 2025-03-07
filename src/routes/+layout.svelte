<script lang="ts">
  import { invalidate } from "$app/navigation";
  import "../app.css";

  let { data, children } = $props();
  let { supabase, session } = $state(data);

  $effect(() => {
    const {
      data: { subscription },
    } = supabase.auth.onAuthStateChange(async (event, _session) => {
      if (_session?.expires_at !== session?.expires_at) {
        invalidate("supabase:auth");
      }
    });

    return () => subscription.unsubscribe();
  });
</script>

<nav
  class="border-b-2 border-gray-300 py-2 px-4 flex items-center justify-between"
>
  <div class="flex items-center space-x-4">
    <a href="/" class="text-lg font-semibold text-gray-700 hover:text-gray-900"
      >Home</a
    >
    {#if session}
      <a
        href="/app"
        class="text-lg font-semibold text-gray-700 hover:text-gray-900">App</a
      >
      <a
        href="/self"
        class="text-lg font-semibold text-gray-700 hover:text-gray-900">Self</a
      >
    {/if}
  </div>
  {#if session}
    <div class="flex items-center space-x-4">
      <img
        class="w-8 h-8 rounded-full"
        src={session.user.user_metadata.avatar_url ??
          "https://api.dicebear.com/8.x/fun-emoji/svg"}
        alt="person_avatar"
      />
      <p class="text-sm text-gray-500">
        Session expires at: {data.session?.expires_at
          ? new Date(data.session.expires_at * 1000).toLocaleString()
          : "unknown"}
      </p>
      <form method="POST" action="auth?/signout">
        <button class="text-sm text-blue-500 hover:underline">Logout</button>
      </form>
    </div>
  {:else}
    <a
      href="/auth"
      class="text-lg font-semibold text-gray-700 hover:text-gray-900">Login</a
    >
  {/if}
</nav>

{@render children?.()}
