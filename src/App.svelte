<script>
</script>

<main>
	<h1>Free Teacher Marketing</h1>
	<p>Add customer (email, youtube chanel, subscribers), mail send or not, mail (title, body), status (sent, received, responded)</p>

	<h3>Add Customer</h3>
	<form on:submit|preventDefault={handleOnSubmit}>
		<label for="email">Email</label>
		<input type="text" name="email" class="email" bind:value={email} /><br>
		<label for="ytchannel">Youtube channel</label>
		<input type="text" name="ytchannel" class="ytchannel" bind:value={ytchannel}><br/>
		<input type="number" name="subscribers" bind:value={subscribers}/><br/>
		<label> Mail sent<input type="checkbox"  name="mail_sent" value="yes" bind:value={mailSent} /></label><br>
		<label>Status(did not send mail, sent mail, customer respond, customer interested, customer studying)</label>
		<input type="text" name="status" bind:value={status}/><br/>

		<button type="submit">Save</button>
	</form>
</main>

<style>
</style>