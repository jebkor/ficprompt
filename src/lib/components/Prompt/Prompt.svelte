

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

	<div class="col-span-12 lg:col-span-3 h-full">
		<input
			bind:value={personOne}
			type="text"
			name="otp1"
			placeholder="Person 1"
			id="person1"
			class="bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
		/>
	</div>

	<div class="col-span-12 lg:col-span-3 h-full">
		<input
			bind:value={personTwo}
			type="text"
			name="otp2"
			placeholder="Person 2"
			id="person2"
			class="bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
		/>
	</div>

  <div class="col-span-12 lg:col-span-3 h-full">
		<input
			bind:value={personThree}
			type="text"
			name="otp3"
			placeholder="Person 3"
			id="person3"
			class="bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
		/>
	</div>

	<div class="col-span-12 lg:col-span-3 h-full">
		<button id="genbutt" type="submit" class="bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" on:click={fluff}
			>Generate</button
		>
	</div>

  {#if promptResult.length > 0}
  <div class="col-span-12">
    <span class="pl-1" on:click={swapCharacters} aria-label="Button">Swap characters in prompt</span>
  </div>
  {/if}
  

	<div id="shitbox" class="col-span-12 p-5 bg-white text-black border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full">
		<b>Your prompt:</b> <br>
		<span id="premise">{@html promptResult}</span>
	</div>

  {#if selectedCategory.length === 0 && showError}
    <div class="col-span-12">
      <span>Please select a category from the dropdown</span>
    </div>
  {/if}
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
  let personThree = ''
	let promptResult = ''
  let promptIndex = 0
  let selectedCategory = ''
  let choice = ''
  let promptDataSet: any[] = []
  let timesClicked = 0
  let showError = false

	// Methods
  /**
   * Generate a random prompt from the dataset
   */
	const fluff = () => {
    // check if category is selected and show error if not
    if (selectedCategory.length === 0) {
      showError = true
      return
    } else {
      showError = false
    }

    // reset the counter on new prompt
    timesClicked = 0

		// Get random prompt and set the index generated
		let randomIndex = randomGenerator()
		let str: string = promptDataSet[randomIndex]
    promptIndex = randomIndex


    // Check if str includes {C} and personThree is empty, find a new str that doesn't include {C}
    while (str.includes("{C}") && personThree === "") {
        randomIndex = randomGenerator()
        str = promptDataSet[randomIndex]
        promptIndex = randomIndex
        console.log('reroll has happened')
    }


		// Replace {A} & {B} in prompt with OTP namesult
		promptResult = str.replace(/{A}/g, personOne).replace(/{B}/g, personTwo)
	}


  /**
   * Set the category of the prompt and get the correct dataset
   */
  const setCategory = () => {
    choice = selectedCategory
    getCorrectPromptData()
  }


  /**
   * Generate a random number to get a random prompt from the dataset
   */
  const randomGenerator = () => {
    return Math.floor(Math.random() * promptDataSet.length)
  }


  /**
   * Swap the characters in the prompt
   */
  const swapCharacters = () => {
    const str: string = promptDataSet[promptIndex]


    if (timesClicked % 2 === 0) {
      promptResult = str.replace(/{B}/g, personOne).replace(/{A}/g, personTwo)
    } else {
      promptResult = str.replace(/{B}/g, personTwo).replace(/{A}/g, personOne)
    }

    timesClicked++
  }


  /**
   * Get the correct dataset based on the category selected
   */
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
