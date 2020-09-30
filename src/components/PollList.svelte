<script>
    import { fade, slide, scale } from "svelte/transition";
    import { flip } from "svelte/animate";
    import PollDetails from "./PollDetails.svelte";
    import PollStore from "../stores/PollStore";

    // let polls = [];
    // const unsub = PollStore.subscribe((data) => (polls = data));

    // onDestroy(() => {
    //     // unsub from store
    //     console.log("destroyed");
    //     unsub();
    // });
</script>

<style>
    .poll-list {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 20px;
    }
</style>

<div class="poll-list">
    {#each $PollStore as poll (poll.id)}
        <div in:fade out:scale|local animate:flip={{ duration: 500 }}>
            <PollDetails {poll} />
        </div>
    {/each}
</div>
