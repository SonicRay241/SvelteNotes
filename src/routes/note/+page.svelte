<script lang="ts">
	import '$lib/css/editor.css'
	import '$lib/css/shades-of-purple.css'
	import MarkdownIt from 'markdown-it';
	import { afterUpdate } from 'svelte';
	import hljs from 'highlight.js';
	
	const md = new MarkdownIt();
	
	let markdown = '';
	let result = '';
	let searchHeaders: string[] = []

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
			innerCode = innerCode.replaceAll("&lt;", '<')
			innerCode = innerCode.replaceAll("&gt;", '>')
			let outputCode = hljs.highlightAuto(innerCode).value.toString()
			result = result.replace(code.toString(), outputCode)
		}
		const reId = result.matchAll(new RegExp(/<h[1-6]>\[\?\s?[\S]+?\][\s\S]*?<\/h[1-6]>/g))
		for (let header of reId) {
			let innerHead = header.toString().slice(4).slice(0, -5)
			const idTag = innerHead.match(new RegExp(/\[\?\s?[\S]+?\]/g))?.toString().slice(2).slice(0, -1)
			const headerContent = innerHead.replace(new RegExp(/\[\?\s?[\S]*?\]/g), "")
			const headerLvl = header.toString()[2]

			const output = `<h${headerLvl} id="${idTag}">${headerContent}</h${headerLvl}>`
			console.log(innerHead);
			
			result = result.replace(header.toString(), output)

			// searchHeaders.push(idTag?.toString() ?? "")
		}

		result = result.replaceAll("<pre>", "<pre class='hljs'>")
		
		const aTags = result.matchAll(new RegExp(/<a href=".*?">/g))
		for (let anchor of aTags){
			let b = ""
			b += anchor.toString().slice(0, -1)
			b += ' target="_blank">'
			anchors += b
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

	<a href="#{searchHeaders[0]}">Moai</a>
</div>