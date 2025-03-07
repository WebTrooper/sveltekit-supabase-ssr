<script lang="ts">
  let { data, form } = $props();

  let { session } = $state(data);
  const providers = session?.user.app_metadata.providers;
  console.log(providers);
  const has_email_provider = providers
    ? providers.some((p: string) => p === "email")
    : session?.user.app_metadata.provider === "email";
</script>

<div class="min-h-screen flex flex-col items-center justify-center bg-gray-100">
  <div class="bg-white shadow-md rounded-lg p-8 max-w-md w-full">
    {#if session}
      <h2 class="text-2xl font-bold mb-4">Welcome to /self!</h2>
      <h3 class="text-lg text-gray-700 mb-4">User Information:</h3>
      <p class="mb-2">ID: {session.user.id}</p>
      <p class="mb-2">Email: {session.user.email || "not set"}</p>
      <p class="mb-2">Phone Number: {session.user.phone || "not set"}</p>
      <p class="mb-2">
        Nickname: {session.user.user_metadata.nickname || "not set"}
      </p>
      <form method="POST" action="?/delete_user" class="space-y-4">
        <label>Delete a user by ID:</label>
        <input
          name="user"
          type="text"
          class="w-full p-2 border border-gray-300 rounded"
        />
        <button
          class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
          >Delete</button
        >
      </form>
      <form method="POST" action="?/update_nickname" class="space-y-4 mt-4">
        <label>Change your nickname:</label>
        <input
          name="nickname"
          type="text"
          value={form?.data?.nickname ?? ""}
          class="w-full p-2 border border-gray-300 rounded"
        />
        <button
          class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
          >Update</button
        >
        <button
          formaction="?/delete_nickname"
          class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
          >Delete</button
        >
      </form>
      <form method="POST" action="?/update_phone" class="space-y-4 mt-4">
        <label>Change your phone number:</label>
        <input
          name="phone"
          type="text"
          class="w-full p-2 border border-gray-300 rounded"
        />
        <button
          class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
          >Update</button
        >
      </form>
      {#if has_email_provider}
        <form method="POST" action="?/update_password" class="space-y-4 mt-4">
          <label>Change your password:</label>
          <input
            name="password"
            type="password"
            class="w-full p-2 border border-gray-300 rounded"
          />
          <button
            class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
            >Change</button
          >
        </form>
      {/if}
      {#if session.user.is_anonymous}
        <form method="POST" action="?/convert_provider" class="space-y-4 mt-4">
          <label>Convert to a permanent user:</label>
          <button
            class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
            name="provider"
            value="github">Use GitHub auth</button
          >
        </form>
        <form method="POST" action="?/convert_email" class="space-y-4 mt-4">
          <label>Convert to a permanent user:</label>
          <input
            name="email"
            type="email"
            placeholder="email"
            class="w-full p-2 border border-gray-300 rounded"
          />
          <button
            class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
            >Use email auth</button
          >
        </form>
      {/if}
    {/if}

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
          placeholder={`OTP sent to ${form?.phone ?? form?.email}`}
          type="text"
          class="w-full p-2 border border-gray-300 rounded"
        />
        {#if form?.password_prompt}
          <input
            name="password"
            placeholder="Enter new password"
            type="password"
            class="w-full p-2 border border-gray-300 rounded"
          />
        {/if}
        {#if form?.phone}
          <input name="phone" type="hidden" value={form.phone} />
        {/if}
        {#if form?.email}
          <input name="email" type="hidden" value={form.email} />
        {/if}
        <button
          class="w-full bg-blue-400 text-white py-2 rounded hover:bg-blue-500"
          >Verify</button
        >
      </form>
    {/if}
  </div>
</div>
