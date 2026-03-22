<script lang="ts">
    interface Props {
        class?: string;
        identifier?: string;
        message?: string;
        borderColor?: string;
        borderDirection?: string;
    }

    let {
        class: className = "",
        identifier = "",
        message = "",
        borderColor = "",
        borderDirection = ""
    }: Props = $props();
</script>

<p 
    style="border-{borderDirection}: 3px solid {borderColor};"
    class={className}
    id={identifier}
>{@html message}</p>

<style>
    p {
        color: var(--muted-text-color);
        font-size: 1.25rem;
        
        padding: 10px;
        padding-left: 20px;
    }
</style>