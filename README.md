# Bleeps and Bleeps DAO

## What are Bleeps ?

The very first sounds of the EVM

Bleeps use the same pioneer concept introduced in Mandalas.eth, where the metadata is fully generated from the contract, zero externalities, no backend, no ipfs, nor any client code required. While mandalas generated bitmaps, Bleeps generate sounds!

## How to get a Bleep ?

Bleeps are initially mintable through a prelimary sale at 0.1 ETH each Bleep. 75% of the proceeds go to the Bleeps DAO. There is a hard-coded max supply of 1024 sounds but only 576 sounds are currently available made of 9 instrument each composed of 64 notes (frequencies). 128 of them are currently reserved by the Bleeps' creator (wighawag) and might be distributed differently. The rest of the Bleeps (448) do not yet exist but can potentially be created later at the discretion of the DAO.

## What are the benefits of owning a Bleep ?

Owning a Bleep transforms a mere user into a Bleeper. Bleepers are given vote in the Bleeps DAO and earn a passive revenue from Melodies created from Bleeps they own. They also get to vote on proposal about the DAO treasury which they manage. One Bleep, One Vote.

## Wait... Melodies ?

Melodies are the next steps for Bleeps. A Melody is a sequence of 32 Bleeps fine-tuned by their creator to output the most awesome tracks. Melodies are put on auction as they are minted, and the revenue from the auction is split between the Creator (90%), the Bleepers (5%) and the DAO (5%). You can test a demo version here.

## What about the Bleeps DAO ?

Like Nouns DAO, Bleeps DAO utilizes a fork of Compound Governance and its main governing body. The Bleep DAO treasury receives a share (5%) of the ETH proceeds from Melodies auctions. Each Bleep is an irrevocable member of the Bleep DAO and entitled to one vote in all governance matters. The DAO has full power over the future of Bleeps, new instruments (up to 16), evolution of Melodies and economic models.

![Bleeps Ecosystem](./web/build/images/bleeps/Core_2.svg)

## App Setup

### requirements :

This app requires [node.js](https://nodejs.org/) (tested on v12+)

#### pnpm

This repo use `pnpm` for package management : https://pnpm.js.org

```bash
npx pnpm add -g pnpm
```

`pnpm` is used because it has the best mono-repo support which this project relies on.
You might be able to switch to `yarn` but will most likely have to configure it to fix hoisting issues.
If you decide to use `yarn` you ll have to remove the script "preinstall" that by default force the use of `pnpm`

#### docker and docker-compose

`docker` and `docker-compose` are used to setup the external services (an ethereum node, an ipfs node and a [subgraph](https://thegraph.com) node)

If you prefer (or do not have access to docker/docker-compose) you can run them independently.

### intall dependencies :

```bash
pnpm boot
```

This will set the app name (and change the files to reflect that) and then call `pnpm install`

You can also manually set the name yourself :

```bash
pnpm set-name [<new name>] && pnpm install
```

## Development

The following command will start everything up.

```bash
pnpm start
```

This will run each processes in their own terminal window/tab. Note that you might need configuration based on your system.

On linux it uses `xterm` by default (so you need that installed).

On windows it use `cmd.exe` by default.

If you need some other terminal to execute the separate processes, you can configure it in `.newsh.json`.

This command will bring 5 shells up

1. docker-compose: running the ethereum node, ipfs node and subgraph node.
2. common-lib: watching for changes and recompiling to js.
3. web app: watching for changes. Hot Module Replacement enabled. (will reload on common-lib changes)
4. contracts: watching for changes. For every code changes, contract are redeployed, with proxies keeping their addresses.
5. subgraph: watch for code or template changes and redeploy.

Once docker-compose is running, you can stop the other shells and restart them if needed via

```bash
pnpm dev
```

Alternatively you can call the following first : this will setup the external services only (ipfs, ethereum and graph nodes)

```bash
pnpm externals
```

and then run `pnpm dev` to bring up the rest in watch mode.

You can also always run them individually

## full list of commands

Here is the list of npm scripts you can execute:

Some of them relies on [./\_scripts.js](./_scripts.js) to allow parameterizing it via command line argument (have a look inside if you need modifications)
<br/><br/>

`pnpm prepare`

As a standard lifecycle npm script, it is executed automatically upon install. It generate various config file for you, including vscode files.
<br/><br/>

`pnpm setup`

this will update name of the project (by default "bleeps") to be the name of the folder (See `set-name` command) and install the dependencies (`pnpm install`)
<br/><br/>

`pnpm set-name [<new name>]`

This will replace every instance of `bleeps` (or whatever name was set) to `new name` (if specified, otherwise it use the folder name)
If your name is not unique and conflict with variable name, etc... this will not be safe to execute.
<br/><br/>

`pnpm common:dev`

This will compile the common-library and watch for changes.
<br/><br/>

`pnpm common:build`

This will compile the common library and terminate
<br/><br/>

`pnpm contracts:dev`

This will deploy the contract on localhost and watch for changes and recompile/redeploy when so.
<br/><br/>

`pnpm contracts:deploy [<network>]`

This will deploy the contract on the network specified.

If network is a live network, a mnemonic and url will be required. the following env need to be set:

- `MNEMONIC_<network name>`
- `ETH_NODE_URI_<network name>`
  <br/><br/>

`pnpm seed [<network>]`

This will execute the contracts/scripts/seed.ts on the network specified
<br/><br/>

`pnpm subgraph:dev`

This will setup and deploy the subgraph on localhost and watch for changes.
<br/><br/>

`pnpm subgraph:deploy [<network>]`

This will deploy subgraph on the network specified. If network is a live network, the following env beed to be set:

- `THEGRAPH_TOKEN` token giving you write access to thegraph.com service
  <br/><br/>

`pnpm web:dev [<network>]`

This will spawn a vite dev server for the webapp, connected to the specified network
<br/><br/>

`pnpm web:build [<network>]`

This will build a static version of the web app for the specified network.
<br/><br/>

`pnpm web:serve`

This will serve the static file as if on an ipfs gateway.
<br/><br/>

`pnpm web:build:serve [<network>]`

this both build and serve the web app.
<br/><br/>

`pnpm web:deploy <network>`

This build and deploy the web app on ipfs for the network specified.

You ll need the following env variables setup :

- `IPFS_DEPLOY_PINATA__API_KEY` │
- `IPFS_DEPLOY_PINATA__SECRET_API_KEY`

<br/><br/>

`pnpm deploy [<network>]`

This will deploy all (contracts, subgraph and web app). See below for more details.

If no network are specified it will fetch from the env variable `NETWORK_NAME`.
<br/><br/>

`pnpm stop`

This stop the docker services running
<br/><br/>

`pnpm externals`
This spawn docker services: an ethereum node, an IPFS node and a subgraph node
<br/><br/>

`pnpm dev`
This assume external service run. It will spawn a web server, watch/build the common library, the web app, the contracts and the subgraph. It will also seed the contracts with some data.
<br/><br/>

`pnpm start`
It will spawn everything needed to get started, external services, a web server, watch/build the common library, the web app, the contracts and the subgraph. It will also seed the contracts with some data.
<br/><br/>

## env variables required for full deployment

You need to gather the following environment variables :

- `THEGRAPH_TOKEN=<graph token used to deploy the subgraph on thegraph.com>`
- `INFURA_TOKEN=<infura token to talk to a network>`
- `IPFS_DEPLOY_PINATA__API_KEY=<pinata api key>`
- `IPFS_DEPLOY_PINATA__SECRET_API_KEY=<pinata secret key>`
- `MNEMONIC=<mnemonic of the account that will deploy the contract>` (you can also use `MNEMONIC_<network name>`)

Note that pinata is currently the default ipfs provider setup but ipfs-deploy, the tool used to deploy to ipfs support other providers, see : https://github.com/ipfs-shipyard/ipfs-deploy

For production and staging, you would need to set MENMONIC too in the respective `.env.production` and `.env.staging` files.

You can remove the env if you want to use the same as the one in `.env`

You'll also need to update the following for staging and production :

- `SUBGRAPH_NAME=<thegraph account name>/<subgraph name>`
- `VITE_CHAIN_ID=<id of the chain where contracts lives>`
- `VITE_THE_GRAPH_HTTP=https://api.thegraph.com/subgraphs/name/<thegraph account name>/<subgraph name>`

you then need to ensure you have a subgraph already created on thegraph.com with that name: https://thegraph.com/explorer/dashboard

Furthermore, you need to ensure the values in [web/application.json](web/application.json) are to your liking. Similar for the the web/public/preview.png image that is used for open graph metadata. The application.json is also where you setup the ens name if any.

## fleek github integration

For `web:build` you can also use [fleek](https://fleek.co) so that building and ipfs deployment is done automatically. The repo provide a `.fleek.json` file already setup for staging.

The only thing needed is setting up the environment variables (VITE_THE_GRAPH_HTTP, VITE_CHAIN_ID). You can either set them in fleek dashboard or set them in `.fleek.json`
