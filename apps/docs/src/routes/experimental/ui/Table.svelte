<script>
    // Experimental "lit surface" Table: same API as moth-ui-svelte's Table.
    /** @type {{ striped?: boolean, interactive?: boolean, children?: import('svelte').Snippet }} */
    let { striped = false, interactive = false, children } = $props();
</script>

<div class="table-wrapper">
    <table class="table" class:is-striped={striped} class:is-interactive={interactive}>
        {@render children?.()}
    </table>
</div>
