<script>
    // Experimental "lit surface" Card: same API as moth-ui-svelte's Card, plus `flat` (no light, e.g. a card nested in a card).
    /**
     * @type {{
     *   space?: 'none' | 'm' | 'l',
     *   flat?: boolean,
     *   style?: string,
     *   header?: import('svelte').Snippet,
     *   headerEnd?: import('svelte').Snippet,
     *   children?: import('svelte').Snippet
     * }}
     */
    let { space = "m", flat = false, style = "", header, headerEnd, children } = $props();
</script>

<div class="card has-space-{space}" class:is-flat={flat} {style}>
    {#if header}
        <div class="card-header">
            {@render header()}
            {@render headerEnd?.()}
        </div>
    {/if}
    <div class="card-content">
        {@render children?.()}
    </div>
</div>
