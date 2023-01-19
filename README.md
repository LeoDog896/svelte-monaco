# monaco-svelte (WIP & unpublished)

reactive two-way monaco editor for svelte.

```sh
npm install --save-dev monaco-svelte
```

```sh
pnpm install --save-dev monaco-svelte
```

```sh
yarn add --save-dev moncao-svelte
```

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