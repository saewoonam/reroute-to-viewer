<script>
	import { tick } from 'svelte';
	import File from './File.svelte';
  import { render_list } from './stores.js';
	import { target } from './target.js'
	export let expanded = false;
	export let name;
	export let children;
  export let path;
	function toggle(event) {
		expanded = !expanded;
		if(expanded) {
			let new_list = children.filter(e=>!('children' in e))
			if(new_list.length) {
				// console.log(new_list)
				render_list.set(new_list)
				console.log('expanded event', event.target, $target)
				if (true) {
					console.log('old target', $target)
					if ($target.length) { // this doesn't work to close other folders
						let elt = document.getElementById($target)
						console.log(elt, elt.classList)
						elt.classList.remove('expanded')
						tick();
						console.log('removed', elt)
					}
					target.set(path)
					console.log('new target', $target)
				}
				// console.log('render_list', $render_list)
			}
		}
	}
	//console.log('name', name);
	//console.log('children', children)
</script>

<style>
	span {
		padding: 0 0 0 1.5em;
		background: url("https://raw.githubusercontent.com/tailwindlabs/heroicons/master/src/outline/folder.svg") 0 0.1em no-repeat;
		background-size: 1em 1em;
		font-weight: bold;
		cursor: pointer;
	}

	.expanded {
		background-image: url("https://raw.githubusercontent.com/tailwindlabs/heroicons/master/src/outline/folder-open.svg");
	}

	ul {
		padding: 0.2em 0 0 0.5em;
		margin: 0 0 0 0.5em;
		list-style: none;
		border-left: 1px solid #eee;
	}

	li {
		padding: 0.2em 0;
	}
</style>

<span id={path} class:expanded on:click={toggle}>{name}</span>

{#if expanded}
	<ul id={'ul'+path}>
		{#each children as file}
			<li>
				{#if 'children' in file}
					<svelte:self {...file}/>
				{:else}
					<File {...file}/>
				{/if}
			</li>
		{/each}
	</ul>
{/if}