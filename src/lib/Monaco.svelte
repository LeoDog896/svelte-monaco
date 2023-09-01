<script lang="ts">
	import type Monaco from 'monaco-editor';
	import { onDestroy, onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';
	import loader from '@monaco-editor/loader';

	const themes = Object.fromEntries(
		Object.entries(import.meta.glob('/node_modules/monaco-themes/themes/*.json')).map(([k, v]) => [
			k.toLowerCase().split('/').reverse()[0].slice(0, -'.json'.length).replaceAll(" ", "-"),
			v
		])
	);

	let monaco: typeof Monaco;

	const dispatch = createEventDispatcher<{
		ready: Monaco.editor.IStandaloneCodeEditor;
	}>();

	let container: HTMLDivElement;
	export let editor: Monaco.editor.IStandaloneCodeEditor | undefined = undefined;
	export let value: string;

	export let theme: string | undefined = undefined;
	export let options: Monaco.editor.IStandaloneEditorConstructionOptions = {
		value,
		automaticLayout: true
	};

	$: if (theme && themes[theme]) {
		const themeName = theme;
		themes[theme]().then((resolvedTheme) => {
			monaco?.editor.defineTheme(themeName, resolvedTheme as any);
			monaco?.editor.setTheme(themeName);
		});
	}

	$: editor?.updateOptions(options);

	$: if (editor && editor.getValue() != value) {
		const position = editor.getPosition();
		editor.setValue(value);
		if (position) editor.setPosition(position);
	}

	onMount(async () => {
		monaco = await loader.init();
		editor = monaco.editor.create(container, options);

		dispatch('ready', editor);

		if (theme && themes[theme]) {
			const themeName = theme;
			themes[theme]().then((resolvedTheme) => {
				monaco?.editor.defineTheme(themeName, resolvedTheme as any);
				monaco?.editor.setTheme(themeName);
			});
		}

		editor.getModel()!.onDidChangeContent(() => {
			if (!editor) return;
			value = editor.getValue();
		});
	});

	onDestroy(() => editor?.dispose());
</script>

<div id="monaco-container" bind:this={container} />

<style>
	div#monaco-container {
		width: 100%;
		height: 100%;
		padding: 0;
		margin: 0;
	}
</style>
