<script lang="ts">
	import "$src/app.css";
  	import type { LayoutProps } from "./$types.js";
	import Navigation from "$src/components/navigation.svelte"
	let { children }: LayoutProps = $props();
</script>



{@render children()}
