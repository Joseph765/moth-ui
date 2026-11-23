<script>
    // Hidden test bench for the experimental "lit surfaces" style (moth-ui-lit-surfaces.md).
    // Not in the nav or sitemap; noindex and disallowed in robots.txt.
    import "./experimental.css";

    /** @type {{ children: import('svelte').Snippet }} */
    let { children } = $props();
</script>

<!-- Every experimental rule is scoped under this class -->
<div class="experimental">
    {@render children()}
</div>
