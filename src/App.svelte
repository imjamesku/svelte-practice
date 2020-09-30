<script>
	import CreatePollForm from "./components/CreatePollForm.svelte";
	import Footer from "./components/Footer.svelte";
	import Header from "./components/Header.svelte";
	import PollList from "./components/PollList.svelte";
	import Tabs from "./shared/Tabs.svelte";

	// tabs
	let items = ["Current Polls", "Add New Poll"];
	let activeItem = "Current Polls";
	function tabChange(e) {
		activeItem = e.detail;
	}
	let polls = [
		{
			id: 1,
			question: "Python or Javascript?",
			answerA: "Python",
			answerB: "Javascript",
			votesA: 9,
			votesB: 15,
		},
	];
	const handleAdd = (e) => {
		const poll = e.detail;
		polls = [poll, ...polls];
		activeItem = "Current Polls";
		console.log(polls);
	};
	function handleVote(e) {
		const { id, option } = e.detail;
		let copiedPolls = [...polls];
		let upvotedPoll = copiedPolls.find((poll) => poll.id === id);
		if (option === "a") {
			upvotedPoll.votesA++;
		}
		if (option === "b") {
			upvotedPoll.votesB++;
		}
		polls = copiedPolls;
	}
</script>

<style>
	main {
		max-width: 960px;
		margin: 40px auto;
		padding: 0 20px;
	}
</style>

<Header />
<main>
	<Tabs {activeItem} {items} on:tabChange={tabChange} />
	{#if activeItem === 'Current Polls'}
		<PollList {polls} on:vote={handleVote} />
	{:else if activeItem === 'Add New Poll'}
		<CreatePollForm on:add={handleAdd} />
	{/if}
</main>

<Footer />
