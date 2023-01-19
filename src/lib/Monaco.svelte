<script lang="ts">
	import { editor as monacoEditor } from 'monaco-editor';
	import { onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';

	// monaco-editor worker importing
	import TypescriptURL from 'monaco-editor/esm/vs/language/typescript/ts.worker?worker&url';
	import HTMLURL from 'monaco-editor/esm/vs/language/html/html.worker?worker&url';
	import CSSURL from 'monaco-editor/esm/vs/language/css/css.worker?worker&url';
	import JSONURL from 'monaco-editor/esm/vs/language/json/json.worker?worker&url';
	import EditorURL from 'monaco-editor/esm/vs/editor/editor.worker?worker&url';

	const dispatch = createEventDispatcher<{
		ready: monacoEditor.IStandaloneCodeEditor;
	}>();

	let container: HTMLDivElement;
	export let editor: monacoEditor.IStandaloneCodeEditor | undefined = undefined;
	export let value: string;

	export let options: monacoEditor.IStandaloneEditorConstructionOptions = {
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
				const getWorkerModule = (moduleUrl: string, label: string) => {
					return new Worker(window.MonacoEnvironment!.getWorkerUrl!(moduleUrl, label), {
						name: label,
						type: 'module'
					});
				};
				switch (label) {
					case 'json':
						return getWorkerModule(JSONURL, label);
					case 'css':
					case 'scss':
					case 'less':
						return getWorkerModule(CSSURL, label);
					case 'html':
					case 'handlebars':
					case 'razor':
						return getWorkerModule(HTMLURL, label);
					case 'typescript':
					case 'javascript':
						return getWorkerModule(TypescriptURL, label);
					default:
						return getWorkerModule(EditorURL, label);
				}
			}
		};

		editor = monacoEditor.create(container, options);

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
