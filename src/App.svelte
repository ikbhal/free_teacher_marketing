<script>

import {onMount} from 'svelte';
//https://svelte.dev/tutorial/writable-stores
import {writable} from 'svelte/store';

import {db} from './firebase';

let email = '';
let ytchannel = '';
let subscribers = 0;
let mailSent = "No";
let status = "";
let mailSubject = "";
let mailBody = "";

export let customerList=writable([]);

async function loadCustomerList(){
	let customersRef = db.collection('free-teacher-marketing-customer');
  	let allCustomers= await customersRef.get();
	let customerListCopy = [];
	for(const doc of allCustomers.docs){
		let docData = doc.data();
		let customerCopy = {
			 email: docData.email,
		ytchannel: docData.ytchannel,
		subscribers: docData.subscribers,
		mailSentL: docData.mailSent,
		status: docData.status,
		mailSubject: dodData.mailSubject,
		mailBody: docData.mailBody,
		};
		customerListCopy.push(customerCopy);
   		console.log(doc.id, '=>', doc.data());
  	}
	customerList.set(customerListCopy);
}

onMount(async () => {
	loadCustomerList();
});

function handleOnSubmit() {
	console.log("form submitted");
	db.collection("free-teacher-marketing-customer").Add({
	    email,
		ytchannel,
		subscribers,
		mailSent,
		status,
		mailSubject,
		mailBody,
	
	})
	.then(() => {
		console.log("Document successfully written!");
		loadBathList();
	})
	.catch((error) => {
		console.error("Error writing document: ", error);
	});
}


function deleteCustomer(id){
	console.log("remove customer id:", id);
	db.collection("free-teacher-marketing-customer").doc(id).delete().then(() => {
		console.log("Document successfully deleted!");
		loadBathList();
	}).catch((error) => {
		console.error("Error removing document: ", error);
	});
}
</script>

<main>
	<h1>Free Teacher Marketing</h1>
	<p>Add customer (email, youtube chanel, subscribers), mail send or not, mail (title, body), status (sent, received, responded)</p>

	<h3>Add Customer</h3>
	<form on:submit|preventDefault={handleOnSubmit}>
		<label for="email">Email</label>
		<input type="text" name="email" class="email" bind:value={email} /><br>
		<label for="ytchannel">Youtube channel</label>
		<input type="text" name="ytchannel" class="ytchannel" bind:value={ytchannel}/><br/>
		<input type="number" name="subscribers" bind:value={subscribers}/><br/>
		

		<label for="mailSent"> Mail sent</label>
		<input type="text"  name="mailSent"  bind:value={mailSent} /><br>

		<label for="mailSubject">Mail Subject</label>
		<input type="text" name="mailSubject"  bind:value={mailSubject} /><br>
		<label for="mailBody">Mail Body</label>
		<textarea name="mailBody" bind:value={mailBody}/><br/>

	
		<label for="status">Status(did not send mail, sent mail, customer respond, customer interested, customer studying)</label>
		<input type="text" name="status" bind:value={status}/><br/>

		<button type="submit">Save</button>
	</form>


	<h3>Customer List</h3>
	<div class="bathList">
	{#each $customerList as customer}
		<div class="customer">
			Email : {customer.eamil}}  <br/>
			Youtube channel: {customer.ytchannel} <br/>
			Subscribers : {customer.subscribers} <br/>
			Mail Sent: {customer.mailSent}  <br/>
			Status : {customer.status}  <br/>
			Mail Subject: {customer.mailSubject}  <br/>
			Mail Body : {customer.mailBody}  <br/>
			<button on:click={deleteCustomer(customer.id)}>Delete</button>
		</div>
	{/each}
	</div>
</main>

<style>

</style>