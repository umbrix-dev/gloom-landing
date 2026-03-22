<script>
    let { value = $bindable(null), label = "", type = "", gap = "10px" } = $props();
</script>

<div style="gap: {gap};">
    <p>{label}</p>
    <input bind:value={value} {type}>
</div>

<style>
    div {
        display: flex;
        flex-direction: column;
    }

    p {
        color: var(--input-label-color);
        font-weight: 500;
        font-size: 1.25rem;
    }

    input {
        color: white;
        background-color: var(--input-bg);
        border: none;
        border-radius: var(--border-radius);
        outline: none;

        font-size: 1.25rem;
        letter-spacing: 0.5px;

        padding: 15px;
    }
</style>