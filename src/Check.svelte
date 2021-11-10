<script>
export let label = '';
export let value =false;
export let handleClick ;
</script>

<div class="check">
    <label> 
        <input type="checkbox" bind:value={value}
            on:click={() =>handleClick(label)}
        /> 
        {label} 
    </label>
        
</div>