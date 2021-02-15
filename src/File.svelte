<script>
	export let name;
	export let path;
	$: type = name.slice(name.lastIndexOf('.') + 1);
</script>

<style>
	span {
		padding: 0 0 0 1.5em;
		background: 0 0.1em no-repeat;
		background-size: 1em 1em;
		background-image: url(https://raw.githubusercontent.com/tailwindlabs/heroicons/master/src/outline/document.svg)
	}
</style>

<span id={path} >{name}</span>