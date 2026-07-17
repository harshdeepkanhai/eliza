<script lang="ts">
  type Message = { id: string; sender: "patient" | "eliza"; text: string };
  let draft: string = $state("");
  let messages: Message[] = $state([]);

  const mirror: Record<string, string> = {
    i: "you",
    me: "you",
    my: "your",
    am: "are",
    you: "i",
    your: "my",
    are: "am",
  };
  function reflect(text: string): string {
    return text
      .split(" ") // ← words
      .map((word) => mirror[word.toLowerCase()] ?? word) // ← consult table; miss = keep original
      .join(" ");
  }

  const lastMessage = $derived(
    messages[messages.length - 1] ?? "...silence...",
  );
  function speak(e: SubmitEvent) {
    e.preventDefault();
    if (draft.trim().length === 0) return;
    messages.push({ id: crypto.randomUUID(), sender: "patient", text: draft });
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
    {#each messages as message (message.id)}
      <li>{message.text}</li>
    {/each}
  </ul>
</div>
