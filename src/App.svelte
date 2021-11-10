<script>
import CheckList from "./CheckList.svelte"; 
import {db} from './firebase';
import defaultCheckListValues from './DefaultCheckListValues';
// let tags = ["namaaz", "deen"];
// let zikrTags = ["namaaz", "zikr", "deen", "easy", "anytime"];
// let defaultNamaazListValues = [
// 	{
// 		label: "Zikr",
// 		value: false, 
// 		tags: zikrTags
// 	},
//     {
//         label: "Subah",
//         value: false,
//         tags 
//     },
//     {
//         label: "Zuhar",
//         value: false,
//         tags
//     },
//     {
//         label: "Asr",
//         value: false,
//         tags
//     },
//     {
//         label: "Magrib",
//         value: false,
//         tags
//     },
//     {
//         label: "Isha",
//         value: false,
//         tags
//     },
// 	{
// 		label: "Quran",
// 		value: false,
// 		tags: ["quran", "deen", "ilm"]
// 	},
// 	{
// 		label: "duwa",
// 		value: false,
// 		tags: ["duwa", "deen", "problem solver"]
// 	},
// 	{
// 		label: "sadkah",
// 		value: false,
// 		tags:["sadkah", "deen", "nafil", "problem solver"]
// 	}, 
// 	{
// 		label: "toilet",
// 		value: false,
// 		tags: ["clean", "deen"]
// 	}, 
// 	{
// 		label: "sleep early",
// 		value: false,
// 		tags: ["deen", "sunnath"]
// 	}, 
// 	{

// 		label: "cooking", // eat home food, allah wale , avoid eating in hindu hotels, wrong way eating..
// 		value: false,
// 		tags: ["deen", "nafil"]
// 	}
// ];

const getCollectionName = (user) => {
    return `${user}-namaaz-account`;
};
</script>

<main>
	<h1>Namaaz Account</h1>
	<p>Track Namaaz Tracking for family</p>
    <CheckList user='Ikbhal' 
		db = {db}
		collectionName={getCollectionName('ikbhal')}	
		defaultCheckListValues={defaultCheckListValues} />
    <CheckList user='Sumiya' 
		db = {db}
		collectionName={getCollectionName('sumiya')}	
		defaultCheckListValues={defaultCheckListValues} />
    <CheckList user='Towheed' 
		db = {db}
		collectionName={getCollectionName('towheed')}	
		defaultCheckListValues={defaultCheckListValues}   />
</main>

<style>
</style>