<script>
    import Button from "../shared/Button.svelte";
    import Card from "../shared/Card.svelte";
    import PollStore from "../stores/PollStore";
    import { tweened } from "svelte/motion";
    export let poll;
    //reactive values
    $: totalVotes = poll.votesA + poll.votesB;
    $: percentA =
        poll.votesA === 0 ? 0 : Math.floor((100 / totalVotes) * poll.votesA);
    $: percentB =
        poll.votesB === 0 ? 0 : Math.floor((100 / totalVotes) * poll.votesB);

    // tweened parcentages
    const tweenedA = tweened(0);
    const tweenedB = tweened(0);
    $: tweenedA.set(percentA);
    $: tweenedB.set(percentB);
    // $: console.log($tweenedA, $tweenedB);

    function handleVote(option, id) {
        PollStore.update((currentPolls) => {
            let copiedPolls = [...currentPolls];
            let upvotedPoll = copiedPolls.find((poll) => poll.id === id);
            if (option === "a") {
                upvotedPoll.votesA++;
            }
            if (option === "b") {
                upvotedPoll.votesB++;
            }
            return copiedPolls;
        });
    }

    // deleting a poll
    const handleDelete = (id) => {
        PollStore.update((currentPolls) =>
            currentPolls.filter((poll) => poll.id != id)
        );
    };
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
    .delete {
        text-align: center;
        margin-top: 30px;
    }
</style>

<Card>
    <div class="poll">
        <h3>{poll.question}</h3>
        <p>Total votes: {totalVotes}</p>
        <div class="answer" on:click={() => handleVote('a', poll.id)}>
            <div class="percent percent-a" style="width: {$tweenedA}%">
                <span>{poll.answerA} ({poll.votesA})</span>
            </div>
        </div>
        <div class="answer" on:click={() => handleVote('b', poll.id)}>
            <div class="percent percent-b" style="width: {$tweenedB}%">
                <span>{poll.answerB} ({poll.votesB})</span>
            </div>
        </div>
        <div class="delete">
            <Button flat on:click={() => handleDelete(poll.id)}>Delete</Button>
        </div>
    </div>
</Card>
