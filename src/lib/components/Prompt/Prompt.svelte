<script lang="ts">
	import { spring } from 'svelte/motion';
  export let prompts: string[]


	// reactive bindings
	let personOne = '';
	let personTwo = '';
	let promptResult = '';

	// Methods
	const fluff = () => {
		// Get random prompt
		const R1 = Math.floor(Math.random() * prompts.length);
		const str: string = prompts[R1];

		// Replace {A} & {B} in prompt with OTP names
		const result = str.replace(/{A}/g, personOne).replace(/{B}/g, personTwo);

		promptResult = result;
	};
</script>

<form
	class="col-span-12 prompt grid grid-cols-12 gap-y-2 gap-x-1 w-full sm:max-w-[550px] items-center h-fit"
>
	<div class="col-span-12 lg:col-span-4 h-full">
		<input
			bind:value={personOne}
			type="text"
			name="otp1"
			placeholder="Person 1"
			id="person1"
			class="flex w-full h-full py-1 rounded-2xl text-base text-center"
		/>
	</div>

	<div class="col-span-12 lg:col-span-4 h-full">
		<input
			bind:value={personTwo}
			type="text"
			name="otp2"
			placeholder="Person 2"
			id="person2"
			class="flex w-full h-full py-1 rounded-2xl text-base text-center"
		/>
	</div>

	<div class="col-span-12 lg:col-span-4 h-full">
		<button id="genbutt" type="submit" class="block w-full h-full text-center rounded-2xl text-base" on:click={fluff}
			>Generate</button
		>
	</div>

	<div id="shitbox" class="col-span-12 rounded-2xl p-5">
		<b>Your prompt:</b> <br>
		<span id="premise">{promptResult}</span>
	</div>
</form>

<style lang="scss">
	#person1 {
		background: #fefff0;
		border: 2px solid #d6d6d6;
		color: #808080;
		display: flex;
		font-family: arial;
		line-height: 14px;
	}

	#person2 {
		background: #fefff0;
		border: 2px solid #d6d6d6;
		color: #808080;
		font-family: arial;
		line-height: 14px;
	}

	#genbutt {
		background: #fff;
		border: 2px solid #d6d6d6;
	}

	#shitbox {
		background: #fff;
		border: 2px solid #d6d6d6;
	}
</style>
