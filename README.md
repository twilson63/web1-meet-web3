# Using Web 3 to deliver Web 1 websites

This demo, is to introduce some developers tools in the Arweave ecosystem, that are awesome to work with:

* arlocal - a devnet for arweave
* arkb - a deploy tool for arweave

In this simple demo, I am going to take a web 1 website and deploy it to the world using the permaweb. And the cool thing about this process is that I will only pay once and be able to host my site forever! Yes! Forever.


## Requirements

In order to get the most out of this tutorial you will need the latest version of NodeJS installed. https://nodejs.org

## Setup the devnet

First we need to setup our devnet environment:

in a terminal window run

```
npm i -g arlocal
arlocal
```

Woohoo, we have a little devnet running that we can use to test our deployment.

## Create a wallet

Lets use https://arweave.app to create a wallet or we could create one programmatically.


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
  fs.writeFileSync(
    'wallet.json', 
    JSON.stringify(key)
  )
)
```

Either way we end up with a `wallet.json` file.

## Deploy locally

> mint some ar for our wallet `curl localhost:/1984/mint/[address]/10000000000000

```
npm i -g arkb
arkb deploy public --wallet wallet.json --gateway http://localhost:1984
```

## Deploy to arweave.net

....
