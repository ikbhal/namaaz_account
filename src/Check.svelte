<script>
export let label = '';
export let value =false;
export let tags =[];
export let handleClick ;
</script>

<div class="check">
    <label> 
        <input type="checkbox" bind:value={value}
            on:click={() =>handleClick(label)}
        /> 
        {label}
        
        <span class="tag_wrapper"> Tags: 
        {#each tags as tag}
        <span class='tag'>{tag}</span>
        {/each}
        </span>
    </label>
        
</div>

<style>
.tag {
    border-radius: 3px;
    border: 1px solid orange;
    margin: 2px;
}
</style>