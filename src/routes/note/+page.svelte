<script>
	import '$lib/css/editor.css'
	import '$lib/css/shades-of-purple.css'
	import MarkdownIt from 'markdown-it';
	import { afterUpdate } from 'svelte';
	import hljs from 'highlight.js';
	
	const md = new MarkdownIt();
	
	let markdown = '';
	let result = '';

	let anchors = ""
	
	afterUpdate(() => {
		// result = sanitizeHtml(md.render(markdown), {
		// 	allowedTags: sanitizeHtml.defaults.allowedTags.concat([ 'h1', 'h2', 'img' ])
		// });

		result = md.render(markdown)
		const re = new RegExp(/<code>[\s\S]*?<\/code>/g)
		const codes = result.matchAll(re)
		for (let code of codes){
			let innerCode = code.toString().slice(6).slice(0, -7)
			if (innerCode[-1] === " " || "\n") {
				innerCode.slice(0,-1)
			}
			innerCode = innerCode.replaceAll("&quot;", '"')
			let outputCode = hljs.highlightAuto(innerCode).value.toString()
			result = result.replace(code.toString(), outputCode)
		}

		result = result.replaceAll("<pre>", "<pre class='hljs'>")
		
		const aTags = result.matchAll(new RegExp(/<a href=".*?">/g))
		for (let anchor of aTags){
			let b = ""
			b += anchor.toString().slice(0, -1)
			b += ' target="_blank">'
			anchors += b
			console.log(1);
			result = result.replace(anchor.toString(), b)
		}
	});
	
</script>

<div class="markdown-editor">
	<div class="markdown-editor__panel">
		<span class="markdown-editor__panel__label">Markdown</span>
		<textarea class="markdown-editor__textarea" bind:value={markdown} />
	</div>
	<div class="markdown-editor__panel">
		<span class="markdown-editor__panel__label">Output</span>
		<div class="markdown-editor__result-html">
			{@html result}
		</div>
	</div>
</div>