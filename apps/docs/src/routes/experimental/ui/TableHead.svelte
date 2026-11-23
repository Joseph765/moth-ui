<script>
    /** @type {{ children?: import('svelte').Snippet }} */
    let { children } = $props();
</script>

<thead class="table-head">
    {@render children?.()}
</thead>
