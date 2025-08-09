<script lang="ts">
  let currentUrl = $state<string | null>(null);
  let error = $state<string | null>(null);

  async function loadUrl() {
    try {
      const tabs = await chrome.tabs.query({ active: true, currentWindow: true });
      const tab = tabs[0];
      currentUrl = tab?.url ?? null;
      if (!currentUrl) {
        error = 'Could not read the current tab URL. Ensure the extension has the "tabs" permission.';
      }
    } catch (e) {
      error = e instanceof Error ? e.message : String(e);
    }
  }

  $effect(() => {
    // Run once on mount
    loadUrl();
  });
</script>

<h1>Current Page</h1>
{#if currentUrl}
  <p class="url">{currentUrl}</p>
{:else if error}
  <p>{error}</p>
{:else}
  <p>Loading current tab URL…</p>
{/if}
