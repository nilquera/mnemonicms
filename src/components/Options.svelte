<script>
    export let options = [];
    export let handleSelectOption = () => {
        console.log("Please, pass handleSelectOption as a prop");
    };
</script>

<div class="options">
    {#each options as option}
        <span on:click={handleSelectOption(option)}
            >{option[0]}<sup>{option[1].toString().length}</sup></span
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
