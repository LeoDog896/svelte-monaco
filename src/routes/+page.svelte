<script lang="ts">
	import './style.css';
	import Monaco from '$lib/index';

	let value = 'const x = 5';
</script>

<div id="editor">
	<Monaco
		options={{ language: 'typescript' }}
		bind:value
		on:ready={(editor) => {
			editor.detail.addAction({
				// An unique identifier of the contributed action.
				id: 'my-unique-id',

				// A label of the action that will be presented to the user.
				label: 'Custom Action',

				contextMenuGroupId: 'navigation',

				contextMenuOrder: 1.5,

				// Method that will be executed when the action is triggered.
				// @param editor The editor instance is passed in as a convenience
				run: function (ed) {
					alert("i'm running => " + ed.getPosition());
				}
			});
		}}
	/>
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
