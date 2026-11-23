<script>
    /** @type {{ children?: import('svelte').Snippet }} */
    let { children } = $props();
</script>

<tr class="table-row">
    {@render children?.()}
</tr>
