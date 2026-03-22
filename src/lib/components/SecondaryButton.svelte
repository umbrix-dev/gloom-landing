<script lang="ts">
    import type { MouseEventHandler } from "svelte/elements";

    interface Props {
        class?: string;
        identifier?: string,
        text?: string,
        fontSize?: string,
        padding?: string,
        onclick?: MouseEventHandler<HTMLButtonElement>;
    }

    let { 
        class: className = "",
        identifier = "",
        text = "",
        fontSize = "",
        padding = "",
        onclick = (_ev: MouseEvent) => {}
    }: Props = $props();
</script>

<button 
    class={className} id={identifier}
    style="font-size: {fontSize}; padding: {padding}"
    {onclick}
>{text}</button>

<style>
    button {
        color: white;
        background-color: var(--bg-third);
        border: none;
        border-radius: var(--border-radius);

        font-family: var(--secondary-font);
        font-weight: bold;
    }

    button:hover {
        background-color: #202020;
        cursor: pointer;
    }

    button:active {
        background-color: var(--bg-third);
    }
</style>