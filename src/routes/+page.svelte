<script lang="ts">
	import { marked } from 'marked';
	let editorContent = '';
	$: preview = marked(editorContent);
</script>

<main>
	<div class="editor">
		<textarea bind:value={editorContent} />
	</div>
	<div class="preview">
		{@html preview}
	</div>
</main>

<style>
	main {
		width: 97vw;
		height: 97vh;
		display: flex;
		margin: 1%;
	}

	.editor,
	.preview {
		width: 50%;
		height: 100%;
		background-color: #f0f0f0;
	}

	.preview {
		padding-left: 1%;
	}

	textarea {
		width: 100%;
		height: 100%;
		border: none;
		resize: none;
	}
</style>
