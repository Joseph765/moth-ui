<script>
    /** @type {{ children?: import('svelte').Snippet }} */
    let { children } = $props();
</script>

<tbody class="table-body">
    {@render children?.()}
</tbody>
