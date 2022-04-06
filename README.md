# Using Web 3 to deliver Web 1 websites

This demo, is to introduce some developers tools in the Arweave ecosystem, that are awesome to work with:

* arlocal - a devnet for arweave
* arkb - a deploy tool for arweave

In this simple demo, I am going to take a web 1 website and deploy it to the world using the permaweb. And the cool thing about this process is that I will only pay once and be able to host my site forever! Yes! Forever.

---

## Requirements

In order to get the most out of this tutorial you will need the latest version of NodeJS installed. https://nodejs.org

---

## Setup the devnet

First we need to setup our devnet environment:

in a terminal window run

```
npm i -g arlocal
arlocal
```

Woohoo, we have a little devnet running that we can use to test our deployment.

> You can provide options like --hidelogs --persist --dbpath

---

## Create a wallet

Lets use https://arweave.app to create a wallet or we could create one programmatically.


To create a wallet with https://arweave.app, open the url in your browser and click the plus icon in the bottom left corner. Then click create wallet, once finished generating, click the finish button, and then download your key file. The name of the key file is your public address. This is how you publically reference your wallet on the blockweave.


For every transaction in arweave, we need a wallet, in this case we will create a wallet using the `arweave-js` library.

```
yarn init -y
yarn add -D arweave
```

create-wallet script

``` js
const Arweave = require('arweave')
const fs = require('fs')

const arweave = Arweave.init({
  host: 'localhost',
  port: '1984',
  protocol: 'http'
})

arweave.wallets.generate().then(key => 
  arweave.wallets.jwkToAddress(key)
    .then(address =>  fs.writeFileSync(
      address + '.json', 
      JSON.stringify(key)
    )
  )
)
```

Either way we end up with a `[address].json` file.

---

## Deploy locally

> mint some ar for our wallet `curl localhost:/1984/mint/[address]/10000000000000

```
npm i -g arkb
arkb deploy public --wallet wallet.json --gateway http://localhost:1984
```

Success! You should see a url to check out your permaweb website! 

Yay!

> So how does this work? Arweave has a protocol to create a path-manifest json file, this file contains a json object with each asset as the key and the arweave transaction id as the value. This file informs the gateway that the manifest should be treated like a website and look for the index.html file. You can access the manifest file by using the following https://arweave.net/tx/[addr]/data.json

You can find more about the paths manifest here: https://github.com/ArweaveTeam/arweave/blob/master/doc/path-manifest-schema.md

---

## Deploy to arweave.net

Now that we have tested our website with our local arweave devnet, we can deploy it to the production https://arweave.net. First we need to send some AR to our wallet. Copy the address from the wallet file. And open a wallet you have that has some AR, and sent to your wallet address $0.50 cents to your wallet. It may take 15 minutes to transfer.

Once transferred then you are ready to push the web site to arweave.net.

```
arkb deploy public --wallet [address].json
```


