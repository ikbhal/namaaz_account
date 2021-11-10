<script>
import CheckList from "./CheckList.svelte"; 
import {db} from './firebase';

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

const getCollectionName = (user) => {
    return `${user}-namaaz-account`;
};
</script>

<main>
	<h1>Namaaz Account</h1>
	<p>Track Namaaz Tracking for family</p>
    <CheckList user='Ikbhal' 
		db = {db}
		collectionName={getCollectionName('Ikbhal')}	
		defaultCheckListValues={defaultNamaazListValues} />
    <CheckList user='Sumiya' 
		db = {db}
		collectionName={getCollectionName('Sumiya')}	
		defaultCheckListValues={defaultNamaazListValues} />
    <CheckList user='Towheed' 
		db = {db}
		collectionName={getCollectionName('Towheed')}	
		defaultCheckListValues={defaultNamaazListValues}   />
</main>

<style>
</style>