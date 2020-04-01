<script>
    import { loggedIn, storedWalletAddress, storedWalletBalance, arweaveWallet } from './userContext.js';

    export let fileName;
    export let arBalance; 
    export let walletAddress; 
    
    fileName = "";
	arBalance = "";
    walletAddress ="";
    
    let files;

    function login() {
        if (files.length <= 0) {
            return false;
        }
        var fr = new FileReader();

        fr.onload = function(e) { 
            try {
                const arweave = Arweave.init();

                let wallet = JSON.parse(e.target.result);
                arweaveWallet.set(wallet);

                arweave.wallets.jwkToAddress(wallet).then((address) => {
                    walletAddress = address;

                    console.log("arweave walletAddress: ", walletAddress);

                    loggedIn.set("log out"); 
                    storedWalletAddress.set(walletAddress);

                    arweave.wallets.getBalance(address).then((balance) => {
                        let winston = balance;
                        arBalance = arweave.ar.winstonToAr(balance);

                        storedWalletBalance.set(arBalance);

                        console.log("arweave balance: ", arBalance);

                    });
                });
            } catch (err) {
            console.log('Error logging in: ', err);
            }
        }
        
        fr.readAsText(files.item(0));
	};
</script>

<svelte:head>
	<script src="https://unpkg.com/arweave/bundles/web.bundle.js"></script>
</svelte:head>

<input type="file" bind:files accept=".json" on:change={login}>

{#if files && files[0]}
	<p>
		{files[0].name}
	</p>
{/if}