<script lang="ts">
	import './style.css';
	import Monaco from '$lib/index';

	let value = 'const x = 5';
</script>

<div id="editor">
	<Monaco options={{language: "typescript"}} bind:value />
</div>

<textarea bind:value />

<style>
	div#editor {
		width: calc(100% - 2rem - 1px);
		height: 80%;
		border: 1px solid black;
		max-height: 80%;
	}
</style>
