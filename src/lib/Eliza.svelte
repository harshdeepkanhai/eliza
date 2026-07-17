<script lang="ts">
  let messages: string[] = $state([]);
  let draft: string = $state("");

  const lastMessage = $derived(
    messages[messages.length - 1] ?? "...silence...",
  );
  function speak(e: SubmitEvent) {
    e.preventDefault();
    if (draft.trim().length === 0) return;
    messages.push(draft);
    draft = "";
  }
</script>

<div>
  <form onsubmit={speak}>
    <input
      bind:value={draft}
      placeholder="Tell me your troubles..."
    />
    <button type="submit">Speak</button>
  </form>
  <ul>
    {#each messages as message}
      <li>{message}</li>
    {/each}
  </ul>
</div>
