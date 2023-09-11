# [svelte-monaco](https://leodog896.github.io/svelte-monaco)

[![npm](https://img.shields.io/npm/v/svelte-monaco)](https://npmjs.com/package/svelte-monaco)

Reactive two-way, full theme support monaco editor for Svelte(&Kit).

```sh
npm install --save-dev svelte-monaco
```

```sh
yarn add --save-dev moncao-svelte
```

```sh
pnpm install --save-dev svelte-monaco
```

## Example

```svelte
<script lang="ts">
	import Monaco from 'svelte-monaco';

	let value = 'const x = 5';
</script>

<div id="editor">
	<Monaco
		options={{ language: 'typescript', automaticLayout: true }}
		theme="monokai"
		// `event.detail` is the monaco instance
		on:ready={(event) => console.log(event.detail)}
		bind:value
	/>
</div>

<textarea bind:value />
```

## Features
- Reactive everywhere - get every theme from [monaco-themes](https://github.com/brijeshb42/monaco-themes), set the input value, or set the options without hassle.
- Full access - the moment the editor is finished loading, it is shown to the user. No more waiting for API changes.
