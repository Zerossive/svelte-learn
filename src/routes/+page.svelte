<script lang="ts">
	import { fly } from 'svelte/transition'
	import Header from './Header.svelte'

	let formState = $state({
		answers: {},
		step: 0,
		error: ''
	})

	$inspect(formState.step)

	const QUESTIONS = [
		{
			question: 'What is your name?',
			id: 'name',
			type: 'text'
		},
		{
			question: 'What is your birthday?',
			id: 'birthday',
			type: 'date'
		},
		{
			question: 'What is your favorite color?',
			id: 'color',
			type: 'color'
		}
	]

	function nextStep(id: string) {
		if (formState.answers[id]) {
			formState.step += 1
			formState.error = ''
		} else {
			formState.error = 'Please fill out the form input'
		}
	}

	// $effect(() => {
	// 	console.log('on mounted')
	// 	return () => {
	// 		console.log('on unmounted')
	// 	}
	// })
	//
	// $effect(() => {
	// 	console.log('formState', formState.step)
	// })
</script>

<Header name={formState.answers.name} />

<main>
	{#if formState.step >= QUESTIONS.length}
		<p>Thank you!</p>
	{:else}
		<p>Step: {formState.step + 1}</p>
	{/if}

	{#each QUESTIONS as question, index (question.id)}
		{#if formState.step === index}
			<div
				in:fly={{ x: 200, duration: 200, opacity: 0, delay: 200 }}
				out:fly={{ x: -200, duration: 200, opacity: 0 }}
			>
				{@render formStep(question)}
			</div>
		{/if}
	{/each}

	{#if formState.error}
		<p class="error">{formState.error}</p>
	{/if}
</main>

{#snippet formStep({ question, id, type }: { question: string; type: string; id: string })}
	<article>
		<div>
			<label for={id}>{question}</label>
			<input {type} {id} bind:value={formState.answers[id]} />
		</div>
		<button onclick={() => nextStep(id)}>Next</button>
	</article>
{/snippet}

<style>
	.error {
		color: red;
	}
</style>
