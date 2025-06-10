<script lang="ts">
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";
  import Cookies from "js-cookie";
  import axios from "axios";

  let user: any = null;
  const token = Cookies.get("token");
  const API = "http://localhost:3000";

  onMount(async () => {
    if (!token) {
      goto("/login");
      return;
    }

    try {
      const res = await axios.post(`${API}/api/auth/user`, { token });

      if (res.data.status === "OK") {
        user = res.data.data;
      } else {
        goto("/login");
      }
    } catch (e) {
      console.error("Error fetching user data:", e);
      goto("/login");
    }
  });
</script>

<main class="p-6 max-w-6xl mx-auto space-y-10">
  {#if user}
    <h1>Welcome, {user.email}!</h1>
  {:else}
    <p>Loading user data...</p>
  {/if}
</main>

<style>
  main {
    font-family: system-ui, sans-serif;
  }
</style>
