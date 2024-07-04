<script>
  import Prompt from '../../lib/components/Prompt/Prompt.svelte'
  import {nsfwPrompts} from '$lib/components/Prompt/nsfwprompts'
</script>

<!-- <svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head> -->

<section class="grid grid-cols-12 px-8 justify-items-center items-center h-screen">
  <Prompt prompts={nsfwPrompts}/>
</section>

<style lang="scss"></style>
