# svelte-monaco

reactive two-way monaco editor for svelte.

```sh
npm install --save-dev svelte-monaco
```

```sh
pnpm install --save-dev svelte-monaco
```

```sh
yarn add --save-dev moncao-svelte
```

```svelte
<script lang="ts">
	import Monaco from 'svelte-monaco';

	let value = 'const x = 5';
</script>

<div id="editor">
	<Monaco
		options={{ language: 'typescript' }}
		on:ready={(event) => console.log(event.detail)}
		bind:value
	/>
</div>

<textarea bind:value />
```
