<script>
    const COLORS = [
        "#000",
        "#2C3E50",
        "#E74C3C",
        "#F7DC6F",
        "#F7DC6F",
        "#F7DC6F",
        "#F7DC6F",
        "#F7DC6F",
    ];
    
    export let options = [];
    export let handleSelectOption = () => {
        console.log("Please, pass handleSelectOption as a prop");
    };

    const getColorIndex = (digitsUsed) => {
        const index = Number(digitsUsed.toString().length - 1);
        const normalizedIndex = Math.min(Math.max(index, 0), 7);
        return normalizedIndex;
    };
</script>

<div class="options">
    {#each options as [word, digitsUsed]}
        <span
            on:click={handleSelectOption({ word, digitsUsed })}
            style="color: {COLORS[getColorIndex(digitsUsed)]}"
            >{word}<sup>{digitsUsed.toString().length}</sup></span
        >
    {/each}
</div>

<style>
    .options {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
    }
    .options span {
        margin: 0px 5px;
        cursor: pointer;
    }
</style>
