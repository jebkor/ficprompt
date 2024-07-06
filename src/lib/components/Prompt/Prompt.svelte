

<form
	class="col-span-12 prompt grid grid-cols-12 gap-y-2 gap-x-1 w-full sm:max-w-[550px] items-center h-fit"
>
  <div class="col-span-12">
    <select bind:value={selectedCategory} on:change={() => setCategory()} class="bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5">
      <option selected value="" disabled>Choose a cetegory</option>
      <option value="fluff">Fluff</option>
      <option value="kisses">Kisses</option>
      <option value="dates">Dates</option>
      <option value="nsfw">NSFW</option>
    </select>
  </div>

	<div class="col-span-12 lg:col-span-4 h-full">
		<input
			bind:value={personOne}
			type="text"
			name="otp1"
			placeholder="Person 1"
			id="person1"
			class="bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
		/>
	</div>

	<div class="col-span-12 lg:col-span-4 h-full">
		<input
			bind:value={personTwo}
			type="text"
			name="otp2"
			placeholder="Person 2"
			id="person2"
			class="bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
		/>
	</div>

	<div class="col-span-12 lg:col-span-4 h-full">
		<button id="genbutt" type="submit" class="bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" on:click={fluff}
			>Generate</button
		>
	</div>

	<div id="shitbox" class="col-span-12 p-5 bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full">
		<b>Your prompt:</b> <br>
		<span id="premise">{promptResult}</span>
	</div>
</form>



<script lang="ts">
	import { spring } from 'svelte/motion';
  import prompts from './prompts'
  import dateIdeas from './dateIdeas'
  import kissPrompts from './kissprompts'
  import nsfwPrompts from './nsfwprompts'


	// reactive bindings
	let personOne = ''
	let personTwo = ''
	let promptResult = ''
  let selectedCategory = ''
  let choice = ''
  let promptDataSet: any[] = []

	// Methods
	const fluff = () => {
		// Get random prompt
		const R1 = Math.floor(Math.random() * promptDataSet.length)
		const str: string = promptDataSet[R1]

		// Replace {A} & {B} in prompt with OTP names
		const result = str.replace(/{A}/g, personOne).replace(/{B}/g, personTwo)

		promptResult = result
	}


  const setCategory = () => {
    choice = selectedCategory
    
    getCorrectPromptData()
  }


  const getCorrectPromptData = () => {
    if (choice === 'fluff') {
      return promptDataSet = prompts
    } 
    
    if (choice === 'dates') {
      return promptDataSet = dateIdeas
    } 
    
    if (choice === 'nsfw') {
      return promptDataSet = nsfwPrompts
    } 
    
    if (choice === 'kisses') {
      return promptDataSet = kissPrompts
    }
  
    // default dataset
    return promptDataSet = prompts
  }


</script>

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
