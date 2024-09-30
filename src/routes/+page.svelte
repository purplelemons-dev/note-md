<script lang="ts">
	import { marked } from 'marked';
	import { onMount } from 'svelte';
	import { jsPDF } from 'jspdf';

	let editorContent = '';
	let hostname = '';
	$: preview = marked(editorContent);
	$: filename = `${hostname}_${editorContent.split(' ').slice(0, 3).join('-')}`;

	const mdSave = () => {
		const blob = new Blob([editorContent], { type: 'text/markdown' });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = `${filename}.md`;
		a.click();
		URL.revokeObjectURL(url);
	};

	const exportPDF = () => {
        // may want to switch to html2pdf--pdfs look like shit right now
		const doc = new jsPDF();
		const preprocPreview = `<div style="color: #000;">${preview}</div>`;
		console.log(preprocPreview);
		doc.html(preprocPreview, {
			callback: (doc) => {
				doc.save(`${filename}.pdf`);
			},
			x: 10,
			y: 10,
			/*html2canvas: {
				scale: 0.37
			}*/
		});
	};

	onMount(() => {
		hostname = location.host.split(':')[0].replaceAll('.', '_');

		const textarea = document.querySelector('textarea');
		if (textarea) {
			textarea.focus();
			textarea.addEventListener('keydown', (e) => {
				// Ctrl+S to save
				// Ctrl+P or Ctrl+Shift+E to export PDF
				if (e.ctrlKey && e.key === 's') {
					e.preventDefault();
					mdSave();
				} else if ((e.ctrlKey && e.key === 'p') || (e.ctrlKey && e.shiftKey && e.key === 'e')) {
					e.preventDefault();
					exportPDF();
				}
			});
		}
	});
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
