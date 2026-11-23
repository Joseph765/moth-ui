<script>
    // Experimental "lit surface" Badge: same API as moth-ui-svelte's Badge, rendering `badge` classes.
    /**
     * @type {{
     *   variant?: "neutral" | "danger" | "warning" | "success" | "info",
     *   strong?: boolean,
     *   icon?: import('svelte').Snippet,
     *   children?: import('svelte').Snippet
     * }}
     */
    let { variant = "neutral", strong = false, icon, children } = $props();
</script>

<div class="badge is-{variant}" class:is-strong={strong}>
    {@render icon?.()}
    {@render children?.()}
</div>
