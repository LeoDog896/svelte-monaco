<script lang="ts">
	import './style.css';
	import Monaco, { themes } from '$lib/index';

	let value = 'const x = 5';
	let readOnly = false;
	let theme = 'github-light';

	function selectTheme(themeSelection: string) {
		return () => {
			theme = themeSelection;
			window.scrollTo(0, 0);
		};
	}
</script>

<main>
	<h1>svelte-monaco</h1>
	<h2>monaco bindings for svelte(&kit)</h2>

	<div id="editor">
		<Monaco
			options={{
				language: 'typescript',
				readOnly
			}}
			{theme}
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

	<hr />

	<!-- readonly checkbox -->
	<label>
		<input type="checkbox" bind:checked={readOnly} />
		readonly
	</label>

	<br />

	<p>Theme: <input type="text" bind:value={theme} /> </p>

	<p>Available themes:</p>
	<ul>
		{#each Object.keys(themes) as theme}
			<li><button on:click={selectTheme(theme)}>{theme}</button></li>
		{/each}
	</ul>
</main>
<style>
	div#editor {
		width: calc(100% - 2rem - 1px);
		height: 80%;
		border: 1px solid black;
		max-height: 80%;
	}

	main {
		width: 100%;
		height: 100%;
	}
</style>
