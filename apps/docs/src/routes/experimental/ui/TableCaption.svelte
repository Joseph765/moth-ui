<script>
    /** @type {{ children?: import('svelte').Snippet }} */
    let { children } = $props();
</script>

<caption class="table-caption">
    {@render children?.()}
</caption>
