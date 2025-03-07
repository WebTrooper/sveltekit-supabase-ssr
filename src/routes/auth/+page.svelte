<script lang="ts">
  let { form } = $props();
</script>

<div class="min-h-screen flex flex-col items-center justify-center bg-gray-100">
  <div class="bg-white shadow-md rounded-lg p-8 max-w-md w-full">
    <form method="POST" action="?/signin_email" class="space-y-4">
      <input
        name="email"
        placeholder="email"
        type="email"
        value={form?.data?.email ?? ""}
        class="w-full p-2 border border-gray-300 rounded"
      />
      <input
        name="password"
        placeholder="password"
        type="password"
        class="w-full p-2 border border-gray-300 rounded"
      />
      <button
        class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
        >Login</button
      >
      <button
        formaction="?/signup"
        class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
        >Signup</button
      >
    </form>
    <form method="POST" action="?/oauth" class="space-y-4 mt-4">
      <button
        class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
        name="provider"
        value="github">Login with GitHub</button
      >
    </form>
    <form method="POST" action="?/magic" class="space-y-4 mt-4">
      <input
        name="email"
        placeholder="email"
        type="email"
        class="w-full p-2 border border-gray-300 rounded"
      />
      <button
        class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
        >Login with magic link</button
      >
    </form>
    <form method="POST" action="?/signin_otp" class="space-y-4 mt-4">
      <input
        name="phone"
        placeholder="phone number"
        type="text"
        class="w-full p-2 border border-gray-300 rounded"
      />
      <button
        class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
        >Login with phone OTP</button
      >
    </form>
    <form method="POST" action="?/anon" class="space-y-4 mt-4">
      <button
        class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
        >Login Anonymously</button
      >
    </form>
    <form method="POST" action="?/reset" class="space-y-4 mt-4">
      <input
        name="email"
        placeholder="email"
        type="email"
        class="w-full p-2 border border-gray-300 rounded"
      />
      <button
        class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
        >Reset Your Password</button
      >
    </form>

    {#if form?.message}
      <p class="mt-4 text-green-400">{form.message}</p>
    {/if}
    {#if form?.error}
      <p class="mt-4 text-red-400">{form.error}</p>
    {/if}
    {#if form?.verify}
      <form method="POST" action="?/verify_otp" class="space-y-4 mt-4">
        <input
          name="otp"
          placeholder={`OTP sent to ${form?.phone}`}
          type="text"
          class="w-full p-2 border border-gray-300 rounded"
        />
        <input name="phone" type="hidden" value={form?.phone} />
        <button
          class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
          >Verify</button
        >
      </form>
    {/if}
  </div>
</div>
