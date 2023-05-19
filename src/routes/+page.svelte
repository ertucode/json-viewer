<script lang="ts">
	import JsonViewer from '$lib/JsonViewer.svelte';

	const data = {
		bigArr: [1, 2, 3, 5, 5, 6, 7, 8],
		smallArr: [1, 2],
		obj: {
			bj: { a: 2 },
			asd: 'rld',
			hello2213: 'wo2rld',
			hello3: 'world1',
			nested: {
				he: 'world',
				hello1123123: 'world',
				hello2213: 'wo2rld',
				hello3: 'world1'
			},
			arr: ['saldjfsadf', { hello: 'o' }]
		},
		world: {
			he: 'world',
			hello1123123: 'world',
			hello2213: 'wo2rld',
			hello3: 'world1',
			nested: {
				he: 'world',
				hello1123123: 'world',
				hello2213: 'wo2rld',
				hello3: 'world1'
			},
			arr: ['saldjfsadf', { hello: 'o' }]
		},
		smallObj: { b: 123 },
		hugeArray: Array(1000)
			.fill('')
			.map((_, i) => i),
		thing: '132123132',
		thin: 23,
		thg: null,
		ting: '132123132'
	};

	let jsonViewer: JsonViewer;
	let expanded = false;
</script>

<h1>Welcome to your library project</h1>
<p>Create your package using @sveltejs/package and preview/showcase your work with SvelteKit</p>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<JsonViewer {data} bind:this={jsonViewer} />
