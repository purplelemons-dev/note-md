<script lang="ts">
	import { marked } from 'marked';
	import { onMount } from 'svelte';

	const mdSave = () => {
		const blob = new Blob([editorContent], { type: 'text/markdown' });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = `${location.host.split(":")[0].replaceAll('.', '_')}_${editorContent.split(' ').slice(0, 3).join('-')}.md`;
		a.click();
		URL.revokeObjectURL(url);
	};

	onMount(() => {
		const textarea = document.querySelector('textarea');
		if (textarea) {
			textarea.focus();
			textarea.addEventListener('keydown', (e) => {
				if (e.ctrlKey && e.key === 's') {
					e.preventDefault();
					mdSave();
				}
			});
		}
	});

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
