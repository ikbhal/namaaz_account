<script>
import {writable} from 'svelte/store';
import {onMount} from 'svelte';
import {db} from './firebase';
import Check from './Check.svelte';
export let user = '';
let todayDateString = new Date().toISOString().slice(0,10);
export let checkDate= todayDateString;
// export let handleSave ;
let subahNamaaz = false;
let zuhrNamaaz = false;
let defaultNamaazListValues = [
    {
        label: "Subah",
        value: false
    },
    {
        label: "Zuhar",
        value: false
    },
    {
        label: "Asr",
        value: false
    },
    {
        label: "Magrib",
        value: false
    },
    {
        label: "Isha",
        value: false
    },
];
let namaazListValues = [... defaultNamaazListValues];
let namaazList = writable(namaazListValues);

const handleCheckClick = (label) => {
    console.log("check clicked ", label);
    let namaaz = namaazListValues.find(nz => nz.label ==label);
    console.log("namaaz object", namaaz); 
    namaaz.value = !namaaz.value;
    $namaazList = [... namaazListValues];
};

const handleSave = ( ) => {
    console.log("handle save for user ", user, " values: ", namaazListValues);
	db.collection(`${user}-namaaz-account`).doc(checkDate).set({
        namaazListValues
	})
	.then(() => {
		console.log("Document successfully written!");
	})
	.catch((error) => {
		console.error("Error writing document: ", error);
	});
};

const loadNamaazList = async () => {
    let namaazRef = db.collection(`{user}-namaaz-account`).doc(checkDate);
  	let namaazDoc= await namaazRef.get();
    if (namaazDoc && namaazDoc.exists){
        console.log("namaazDoc.data ", namaazDoc.data());
        namaazListValues = [... namaazDoc.data()]
    }else {
        namaazListValues = defaultNamaazListValues;
    }
};
onMount(async () => {
	loadNamaazList();
});

</script>

<div class="checklist">
    <h3>{user} Check List</h3>
    <label>Date:<input type="date" bind:value={checkDate}/></label>
    {#each $namaazList as namaaz} 
    <Check label={namaaz.label} value={namaaz.value} 
        handleClick={handleCheckClick} />
    {/each}
    <button type="submit" on:click={handleSave}>Save</button>
</div>