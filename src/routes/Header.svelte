<script lang="ts">
	import { createState, DannyState } from './state.svelte'

	let {
		name
	}: {
		name: string
	} = $props()

	const myState = createState()
	const myState2 = new DannyState()
</script>

<div>
	<h1>{name ? name : 'User'}'s Form</h1>
	<button onclick={() => (myState.value = 10)}>{myState.value} to ten</button>
	<button onclick={myState.up}>{myState.value} by one</button>
	<button onclick={() => (myState2.value = 10)}>{myState2.value} to ten differently</button>
	<button onclick={() => myState2.up()}>{myState2.value} by one differently</button>
</div>
