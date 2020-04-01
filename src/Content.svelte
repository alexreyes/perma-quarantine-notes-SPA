<script>
    import { fly } from 'svelte/transition';	
    import Popup from './Popup.svelte';
	import Box from './Box.svelte'; 
    import New from './New.svelte';
    import About from './About.svelte';
    import {getContext} from 'svelte';
	import { loggedIn, storedWalletAddress, storedWalletBalance, arweaveWallet} from './userContext.js';
  	import { and, or, equals } from 'arql-ops';

    const { open } = getContext('simple-modal');
	let posts = []; 

    const showPopup = () => {
        open(Popup);
    };
    const showNew = () => {
        open(New);
    };
    const showAbout = () => {
        open(About);
    };
    
    function logOut() {
        loggedIn.set("log in");
        storedWalletAddress.set('wallet address');
        storedWalletBalance.set('wallet balance');
        arweaveWallet.set('wallet');
    }

    function getPosts() {
		try {
			(async () => {
				const arweave = Arweave.init();

				console.log('arweave initialized'); 

				const myQuery = and(
					equals('from', $storedWalletAddress),
					equals('App-Name', 'QuarantineNotes'),
					equals('TestData', 'false'),
					equals('production', 'true'),
					or(
						equals('App-Version', '0.0.1'),
					)
				);
						
				const results = await arweave.arql(myQuery);

				if (results.length === 0) {
					alert("No posts yet! Click new post to write a new one.");
				}

				for (var count = 0; count < results.length; count++) {
													
					const blockchainTransaction = arweave.transactions.get(results[count]).then(blockchainTransaction => {

						let returnedJson = blockchainTransaction.get('data', {decode: true, string: true});

						posts = posts.concat(JSON.parse(returnedJson)); // add returned posts to the view
							
						// Get tags
						blockchainTransaction.get('tags').forEach(tag => {
							let key = tag.get('name', {decode: true, string: true});
							let value = tag.get('value', {decode: true, string: true});
						});

					});
				}
			})()
		}
		catch (exception) {
			console.log("getting posts error: ", exception); 
		}
	}

</script>
<style>
    section {
        padding: 1em;
    }
</style>
<svelte:head>
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
	<script src="https://unpkg.com/arweave/bundles/web.bundle.js"></script>
	<script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
	<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
	<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
</svelte:head>

{#if $loggedIn === "log in"}
	<button type="button" class="btn btn-primary" id ="logInButton" on:click={showPopup}>{$loggedIn}</button>
{/if}

{#if $loggedIn === "log out"}
	<button type="button" class="btn btn-primary" id ="logInButton" on:click={logOut}>{$loggedIn}</button>
    <button class="btn btn-primary" on:click={getPosts}>get posts</button> 
    <button type="button" class="btn btn-primary" id ="logInButton" on:click={showNew}>new post</button>
    <button type="button" class="btn btn-primary" id ="logInButton" on:click={showAbout}>about</button>
{/if}

<section>
	{#if $loggedIn === "log out"}
		{#if posts.length === 0}
			<p>
			You don't have any posts yet! Click new to write a new post.
			</p>
			{:else}
			{#each posts as post}
				<Box
				postTitle={post.title} 
				postDescription={post.description}
				postDate={post.currDate}
				/>
			{/each}
		{/if}
		{:else}
			<p>Please log in to view your posts. Don't have an arweave wallet? Get one <a href="https://www.arweave.org/wallet">here</a></p>
	{/if}
</section>



