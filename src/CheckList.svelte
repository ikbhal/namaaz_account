<script>
import {writable} from 'svelte/store';
import {onMount} from 'svelte';
import {db} from './firebase';
import Check from './Check.svelte';
export let user = '';
let todayDateString = new Date().toISOString().slice(0,10);
export let checkDate= todayDateString;
let tags = ["namaaz", "deen"];
let defaultNamaazListValues = [
    {
        label: "Subah",
        value: false,
        tags 
    },
    {
        label: "Zuhar",
        value: false,
        tags
    },
    {
        label: "Asr",
        value: false,
        tags
    },
    {
        label: "Magrib",
        value: false,
        tags
    },
    {
        label: "Isha",
        value: false,
        tags
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
    console.log(`${user}-namaaz-account ${checkDate}`)
    let namaazRef = db.collection(`${user}-namaaz-account`).doc(checkDate);
  	// let namaazDoc= await namaazRef.get();
    namaazRef.get().then((doc) => {
        if (doc.exists) {
            console.log("Document data:", doc.data());
            console.log("doc data namaazListValues : ", doc.data().namaazListValues);
            namaazListValues = [... doc.data().namaazListValues]
            $namaazList = [... namaazListValues];
        } else {
            // doc.data() will be undefined in this case
            console.log("No such document!");
            namaazListValues = defaultNamaazListValues;
            $namaazList = [... namaazListValues];
        } 
    }).catch((error) => {
        console.log("Error getting document:", error);
    });
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
        tags = {namaaz.tags||[]}
        handleClick={handleCheckClick} />
    {/each}
    <button type="submit" on:click={handleSave}>Save</button>
</div>

<style>

</style>