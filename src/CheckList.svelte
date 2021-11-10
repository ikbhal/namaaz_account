<script>
import {writable} from 'svelte/store';
import {onMount} from 'svelte';
import Check from './Check.svelte';
export let db;
export let user = '';
let todayDateString = new Date().toISOString().slice(0,10);
export let checkDate= todayDateString;
export let defaultCheckListValues = [];
export let collectionName ;
let checkListValues = [... defaultCheckListValues];
let checkList = writable(checkListValues);

const handleCheckClick = (label) => {
    console.log("check clicked ", label);
    let check= checkListValues.find(nz => nz.label ==label);
    check.value = !check.value;
    $checkList = [... checkListValues];
};

const handleSave = ( ) => {
    console.log("handle save for user ", user, " values: ", checkListValues);
	db.collection(collectionName).doc(checkDate).set({
        checkListValues
	})
	.then(() => {
		console.log("Document successfully written!");
	})
	.catch((error) => {
		console.error("Error writing document: ", error);
	});
};

const loadCheckList = async () => {
    let docRef = db.collection(collectionName).doc(checkDate);
    docRef.get().then((doc) => {
        if (doc.exists) {
            console.log("Document data:", doc.data());
            console.log("doc data checkListValues : ", doc.data().checkListValues);
            checkListValues = [... doc.data().checkListValues]
            $checkList = [... checkListValues];
        } else {
            console.log("No such document!");
            checkListValues = defaultCheckListValues;
            $checkList = [... checkListValues];
        } 
    }).catch((error) => {
        console.log("Error getting document:", error);
    });
};


onMount(async () => {
	// loadCheckList(); temporarly not load, some values not in the default, should be retain for display
});

</script>

<div class="checklist">
    <h3>{user} Check List</h3>
    <label>Date:<input type="date" bind:value={checkDate}/></label>
    {#each $checkList as check} 
    <Check label={check.label} value={check.value} 
        tags = {check.tags||[]}
        handleClick={handleCheckClick} />
    {/each}
    <button type="submit" on:click={handleSave}>Save</button>
</div>

<style>

</style>