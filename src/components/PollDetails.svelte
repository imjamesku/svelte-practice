<script>
    import { createEventDispatcher } from "svelte";
    import Card from "../shared/Card.svelte";
    export let poll;
    $: totalVotes = poll.votesA + poll.votesB;
    $: percentA =
        poll.votesA === 0 ? 0 : Math.floor((100 / totalVotes) * poll.votesA);
    $: percentB =
        poll.votesB === 0 ? 0 : Math.floor((100 / totalVotes) * poll.votesB);
    const dispatch = createEventDispatcher();

    function handleVote(option, id) {
        dispatch("vote", { option, id });
    }
</script>

<style>
    h3 {
        margin: 0 auto;
        color: #555;
    }
    p {
        margin: 6px 0 30px;
        font-size: 14px;
        color: #aaa;
    }
    .answer {
        background: #fafafa;
        cursor: pointer;
        margin: 10px auto;
        position: relative;
    }
    .answer:hover {
        opacity: 0.6;
    }
    span {
        display: inline-block;
        padding: 10px 20px;
    }
    .percent {
        height: 100%;
        /* position: absolute; */
        box-sizing: border-box;
    }
    .percent-a {
        border-left: 4px solid #491b42;
        background: rgba(217, 27, 66, 0.2);
    }
    .percent-b {
        border-left: 4px solid #45c496;

        background: rgba(69, 196, 150, 0.2);
    }
</style>

<Card>
    <div class="poll">
        <h3>{poll.question}</h3>
        <p>Total votes: {totalVotes}</p>
        <div class="answer" on:click={() => handleVote('a', poll.id)}>
            <div class="percent percent-a" style="width: {percentA}%">
                <span>{poll.answerA} ({poll.votesA})</span>
            </div>
        </div>
        <div class="answer" on:click={() => handleVote('b', poll.id)}>
            <div class="percent percent-b" style="width: {percentB}%">
                <span>{poll.answerB} ({poll.votesB})</span>
            </div>
        </div>
    </div>
</Card>
