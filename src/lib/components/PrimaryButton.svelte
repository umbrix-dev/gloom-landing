<script lang="ts">
	import type { MouseEventHandler } from "svelte/elements";

    interface Props {
        class?: string;
        identifier?: string;
        text?: string;
        fontSize?: string;
        padding?: string;
        style?: string;
        onclick?: MouseEventHandler<HTMLButtonElement>;
    }

    let {
        class: className = "",
        identifier = "",
        text = "",
        fontSize = "",
        padding = "",
        style = "",
        onclick = (_ev: MouseEvent) => {}
    }: Props = $props();
</script>

<button 
    class={className} id={identifier}
    style="font-size: {fontSize}; padding: {padding}; {style}"
    {onclick}
>{text}</button>

<style>
    button {
        background: linear-gradient(to bottom, var(--accent-color) 0%, var(--accent-color-dark) 100%);
        border: none;
        border-radius: var(--border-radius);

        font-family: var(--secondary-font);
        font-weight: bold;
    }

    button:hover {
        cursor: pointer;
        background: linear-gradient(to bottom, #5dd18a 0%, #45A469 100%);
    }

    button:active {
        background: linear-gradient(to bottom, var(--accent-color) 0%, var(--accent-color-dark) 100%);
    }
</style>