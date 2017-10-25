# blockchain_voting_system
Your First Decentralized Application. A tutorial to create a decentralized voting system and deploy it to the ethereum testrpc blockchain (intended only for testing) which is free.

## Dependencies

* ethereumjs-testrpc (ethereum test blockchain network, no need to download the whole network)
* web3 (the client that will be used to access this blockchain network)
* solc (solidity is the language tha ethereum uses and solc is the compiler)

## Get started
```
$ git clone https://github.com/Temeteron/blockchain_voting_system.git
$ cd blockchain_voting_system
$ npm install
```

## Usage

1) On your terminal run the following command, which will create a localhost nodejs server to communicate with the test ethereum blockchain. We will use that to vote.
```
$ node_modules/.bin/testrpc
```

2) Open a new terminal in the same folder. (Don't close the previous terminal!)

3) First, run the ‘node’ command in your terminal to get in to the node console and initialize the solc + web3 objects and parse your code.
```
$ node
> Web3 = require('web3')
> web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"))
> code = fs.readFileSync('voting.sol').toString()
> solc = require('solc')
```
4) Compile your code.
4.1) compileddCode.contracts[‘:Voting’].bytecode: This is the bytecode you get when the source code in Voting.sol is compiled. This is the code which will be deployed to the blockchain.
4.2) compiledCode.contracts[‘:Voting’].interface: This is an interface or template of the contract (called abi) which tells the contract user what methods are available in the contract. Whenever you have to interact with the contract in the future, you will need this abi definition.
4.3) You first create a contract object (VotingContract below) which is used to deploy and initiate contracts in the blockchain. VotingContract.new below deploys the contract to the blockchain. The first argument is an array of candidates who are competing in the election which is pretty straightforward. 
The second argument: 
4.3.1) data: This is the compiled bytecode which we deploy to the blockchain, 
4.3.2) from: The blockchain has to keep track of who deployed the contract. In this case, we are just picking the first account we get back from calling web3.eth.accounts to be the owner of this contract (who will deploy it to the blockchain). Remember that web3.eth.accounts returns an array of 10 test accounts testrpc created when we started the test blockchain. In the live blockchain, you can not just use any account. You have to own that account and unlock it before transacting. You are asked for a passphrase while creating an account and that is what you use to prove your ownership of that account. Testrpc by default unlocks all the 10 accounts for convenience. 
4.3.3) gas: It costs money to interact with the blockchain. This money goes to miners who do all the work to include your code in the blockchain. You have to specify how much money you are willing to pay to get your code included in the blockchain and you do that by setting the value of ‘gas’. The ether balance in your ‘from’ account will be used to buy gas. The price of gas is set by the network.
```
> compiledCode = solc.compile(code)
> abiDefinition = JSON.parse(compiledCode.contracts[':Voting'].interface)
> VotingContract = web3.eth.contract(abiDefinition)
> byteCode = compiledCode.contracts[':Voting'].bytecode
> deployedContract = VotingContract.new(['Teo','Nick','Harry'],{data: byteCode, from: web3.eth.accounts[0], gas: 4700000})
> deployedContract.address
> contractInstance = VotingContract.at(deployedContract.address)
```
5) We have now deployed the contract and have an instance of the contract (variable contractInstance above) which we can use to interact with the contract within the node console.
6) To interact with the contract via the html page attached, edit line 5 in your index.js and replace the deployedContract.address with the hash you got from the node console at step (4).
```
contractInstance = VotingContract.at(deployedContract.address);
```
7) You are ready. Just open the index.html in the browser and vote. The voting system operates through the web3 server you started at step (1). You can view the transactions there while you are using the website to vote.
