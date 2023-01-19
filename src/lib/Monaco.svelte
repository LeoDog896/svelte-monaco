<script lang="ts">
	import * as monaco from 'monaco-editor';
	import { onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher<{
		ready: monaco.editor.IStandaloneCodeEditor;
	}>();

	let container: HTMLDivElement;
	export let editor: monaco.editor.IStandaloneCodeEditor | undefined = undefined;
	export let value: string;

	export let options: monaco.editor.IStandaloneEditorConstructionOptions = {
		value,
		automaticLayout: true
	};

	$: editor?.updateOptions(options);

	$: if (editor && editor.getValue() != value) {
		const position = editor.getPosition();
		editor.setValue(value);
		if (position) editor.setPosition(position);
	}

	onMount(() => {
		window.MonacoEnvironment = {
			getWorker: function (_, label) {
				const getWorkerModule = (moduleUrl: string, label: any) => {
					return new Worker(window.MonacoEnvironment!.getWorkerUrl!(moduleUrl, label), {
						name: label,
						type: 'module'
					});
				};
				switch (label) {
					case 'json':
						return getWorkerModule('/monaco-editor/esm/vs/language/json/json.worker?worker', label);
					case 'css':
					case 'scss':
					case 'less':
						return getWorkerModule('/monaco-editor/esm/vs/language/css/css.worker?worker', label);
					case 'html':
					case 'handlebars':
					case 'razor':
						return getWorkerModule('/monaco-editor/esm/vs/language/html/html.worker?worker', label);
					case 'typescript':
					case 'javascript':
						return getWorkerModule(
							'/monaco-editor/esm/vs/language/typescript/ts.worker?worker',
							label
						);
					default:
						return getWorkerModule('/monaco-editor/esm/vs/editor/editor.worker?worker', label);
				}
			}
		};

		editor = monaco.editor.create(container, options);

		dispatch('ready', editor);

		editor.getModel()!.onDidChangeContent(() => {
			if (!editor) return;
			value = editor.getValue();
		});

		return () => {
			editor?.dispose();
		};
	});
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
