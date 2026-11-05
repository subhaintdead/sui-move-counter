# on-chain irreversible counter

this is a counter where you can add +1 to the previous number. it's irreversible, 
so the number only goes up, just like you age

live on sui testnet(not mainnet, im broke)
## features

- smart contract that adds 1 to the previous number

- the counter itself is a shared object so all can update it

- connect wallet

- hardcoded on testnet

- irreversible cuz it doesnt have a way to decrement(hardcoded)

- unspoofable(yes i ran out of words)


## how to use

just open it in your browser:

https://frontend-brown-phi-64.vercel.app/

before that, get a sui wallet, preferably [slush](https://slush.app)

after you set up slush, get your wallet address from the profile page. then in the profile page>advanced, switch the network to testnet

get some testnet sui from [my sui faucet project](https://sui-faucet-seven.vercel.app/) cuz you will need some sui to pay for gas

open https://frontend-brown-phi-64.vercel.app

connect your wallet

after you connect your wallet and sign the connection, you click "add +1" and then sign the transaction

then you see the updated count

if youre a nerd, you can click the suiscan link to see what exactly happened

## the contract

module `ctr_module` has 3 functions:

`create_counter`, which created the counter object. you call this only once after publishing

`increment` adds 1, emits "Incremented"

`count` reads the current number

## how to reproduce it locally

### what you need installed

sui cli, installed with suiup (i recommend you see the official docs for this:  https://docs.sui.io/guides/developer/getting-started/sui-install)

make sure you use the @testnet one

you gotta have nodejs installed

### 1. clone it:
```bash
git clone https://github.com/subhaintdead/sui-move-counter
cd sui-move-counter
```

### 2. point at the sui cli and get gas

```bash
sui client switch --env testnet
sui client active-address
```

copy the address, go to [here](https://sui-faucet-seven.vercel.app/) and get a tiny amount of sui(usually enough for multiple transactions), if you want more, the https://faucet.sui.io faucet takes a while to give you the testnet sui

### 3. build

in the terminal:

```bash
cd suistuff
sui move build
```

### 4. publish it

the repo has my deployment saved in `suistuff/Published.toml`. go to that file and delete the `[published.testnet]` block and everything under it otherwise cli will complain about the stuff being already published. 

then inside /suistuff:

```bash 
sui client publish
```

if it complains about gas budget, add `--gas-budget 10000000` (or more)

in the output, find **package id** and copy it

### 5. create the counter

publishing wont make the counter on its own, you gotta "call" it

inside `suistuff/`:

```bash
sui client call --package <package id from above> --module ctr_module --function create_counter
```

in the output, look in the created objects for the one which ends in `::ctr_module::Counter` (owner should say shared). copy its object id

### 6. replace with your own ids

open `frontend/src/App.tsx`, replace these two constants with yours:

```
const packageid = 'your package id';
const counterobjectid = 'your object id'
```

### 7. run the frontend

```bash
cd ../frontend
npm install
npm run dev
```
open the local host link(alternatively, you can use vercel to deploy)

connect your wallet, "add +1"




<br>
<br>
<br>
<br>
(if this feels clunky to you, it is)

learning material: mysten's demos and docs

**AI USAGE: some ai has been used(i rarely used react in my life). however, i DID NOT use ai code editors**
