<script lang="ts">
  import init, * as Nimiq from "@nimiq/core/web";
  import { onMount } from "svelte";

  let client: Nimiq.Client | null = null;
  let consensus: Nimiq.ConsensusState | null = null;
  let head: Nimiq.PlainBlock | null = null;
  onMount(async () => {
    await init();

    const config = new Nimiq.ClientConfiguration();
    const client = await Nimiq.Client.create(config.build());

    client.addConsensusChangedListener((state) => {
      consensus = state;
    });
    client.addHeadChangedListener(async (hash) => {
      head = await client.getBlock(hash);
    });
  });
</script>

<h1>Welcome to SvelteKit + Nimiq</h1>
<p>
  Consensus: {consensus}
  Head: {head?.height}
</p>
