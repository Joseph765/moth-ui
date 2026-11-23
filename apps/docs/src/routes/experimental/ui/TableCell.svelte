<script>
    /**
     * @type {{
     *   align?: 'start' | 'center' | 'end',
     *   truncate?: boolean,
     *   style?: string,
     *   colspan?: number,
     *   children?: import('svelte').Snippet
     * }}
     */
    let { align = "start", truncate = false, style = "", colspan = undefined, children } = $props();
</script>

<td class="table-cell is-align-{align}" class:is-truncated={truncate} {style} {colspan}>
    {@render children?.()}
</td>
