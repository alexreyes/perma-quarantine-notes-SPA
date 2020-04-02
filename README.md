# Permanent Quarantine notes

Quarantine notes is a web app for people to permanently store notes from a historic time onto arweave's permanent blockchain! With arweave, permanent really means *forever*. 

This is the single page version of quarantine notes. This web app was built for the [Open Web Hackathon](https://gitcoin.co/issue/ArweaveTeam/Bounties/1/2929) hosted by arweave. Quarantine notes uses Svelte as its front end framework.

If you'd like to see it in action, This site is permanently hosted on the blockchain at this web address: https://u34gimxxj3t4.arweave.net/RBbGS88Zo5VFjD4uYI3fm3b7ezLvNHgZ67Dh2wpTpBg/index.html


# Installation

This installation assumed you have npm installed. 

`git clone https://github.com/alexreyes/perma-quarantine-notes-SPA`

`cd perma-quarantine-notes-SPA`

`npm install`

`npm run build`

`npm run start`

That's it! 

# Deployment onto arweave

This part requires that arweave-deploy is installed! If you don't have [arweave-deploy](https://github.com/ArweaveTeam/arweave-deploy) run: 
`npm install -g arweave-deploy`

From the project folder, run: 

`npm run build`

`cd public`

Next, move favicon.png, global.css, and index.html to the build folder. You will need to update the paths in index.html. After doing so, your index.html file should look like [this](https://gist.github.com/alexreyes/9817160b96df5b4f88c7037b0ef363cd).

`arweave arweave deploy-dir build`

After these steps, the project should be deployed to the permaweb!
