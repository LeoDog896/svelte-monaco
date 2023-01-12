# monaco-svelte

reactive two-way monaco editor for svelte.

```html
<script lang="ts">
	import Monaco from 'monaco-svelte';

	let value = 'const x = 5';
</script>

<div id="editor">
	<Monaco options={{language: "typescript"}} bind:value />
</div>

<textarea bind:value />
```