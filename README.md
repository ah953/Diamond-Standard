# Diamond-1-hardhat Implementation

This is a reference implementation for [EIP-2535 Diamonds](https://github.com/ethereum/EIPs/issues/2535). To learn about other implementations go here: https://github.com/mudgen/diamond

## Installation

1. Clone this repo:
```console
git clone git@github.com:mudgen/diamond-1-hardhat.git
```

2. Install NPM packages:
```console
cd diamond-1-hardhat
npm install
```

## Deployment

```console
npx hardhat run scripts/deploy.js
```

## Run tests:
```console
npx hardhat test
```

## Upgrade a diamond

Check the `scripts/deploy.js` and or the `test/diamondTest.js` file for examples of upgrades.

Note that upgrade functionality is optional. It is possible to deploy a diamond that can't be upgraded, which is a 'Single Cut Diamond'.  It is also possible to deploy an upgradeable diamond and at a later date remove its `diamondCut` function so it can't be upgraded any more.

Note that any number of functions from any number of facets can be added/replaced/removed on a diamond in a single transaction. In addition an initialization function can be executed in the same transaction as an upgrade to initialize any state variables required for an upgrade. This 'everything done in a single transaction' capability ensures a diamond maintains a correct and consistent state during upgrades.

## Facet Information

**Note:** In this implementation the loupe functions are NOT gas optimized. The `facets`, `facetFunctionSelectors`, `facetAddresses` loupe functions are not meant to be called on-chain and may use too much gas or run out of gas when called in on-chain transactions. In this implementation these functions should be called by off-chain software like websites and Javascript libraries etc., where gas costs do not matter.

However the `facetAddress` loupe function is gas efficient and can be called in on-chain transactions.

The `contracts/Diamond.sol` file shows an example of implementing a diamond.

The `contracts/facets/DiamondCutFacet.sol` file shows how to implement the `diamondCut` external function.

The `contracts/facets/DiamondLoupeFacet.sol` file shows how to implement the four standard loupe functions.

The `contracts/libraries/LibDiamond.sol` file shows how to implement Diamond Storage and a `diamondCut` internal function.

The `scripts/deploy.js` file shows how to deploy a diamond.

The `test/diamondTest.js` file gives tests for the `diamondCut` function and the Diamond Loupe functions.

## How to Get Started Making Your Diamond

1. Reading and understand [EIP-2535 Diamonds](https://github.com/ethereum/EIPs/issues/2535). If something is unclear let me know!

2. Use a diamond reference implementation. You are at the right place because this is the README for a diamond reference implementation.

This diamond implementation is boilerplate code that makes a diamond compliant with EIP-2535 Diamonds.

Specifically you can copy and use the [DiamondCutFacet.sol](./contracts/facets/DiamondCutFacet.sol) and [DiamondLoupeFacet.sol](./contracts/facets/DiamondLoupeFacet.sol) contracts. They implement the `diamondCut` function and the loupe functions.

The [Diamond.sol](./contracts/Diamond.sol) contract could be used as is, or it could be used as a starting point and customized. This contract is the diamond. Its deployment creates a diamond. It's address is a stable diamond address that does not change.

The [LibDiamond.sol](./contracts/libraries/LibDiamond.sol) library could be used as is. It shows how to implement Diamond Storage. This contract includes contract ownership which you might want to change if you want to implement DAO-based ownership or other form of contract ownership. Go for it. Diamonds can work with any kind of contract ownership strategy. This library contains an internal function version of `diamondCut` that can be used in the constructor of a diamond or other places.

## Calling Diamond Functions

In order to call a function that exists in a diamond you need to use the ABI information of the facet that has the function.

Here is an example that uses web3.js:

```javascript
let myUsefulFacet = new web3.eth.Contract(MyUsefulFacet.abi, diamondAddress);
```

In the code above we create a contract variable so we can call contract functions with it.

In this example we know we will use a diamond because we pass a diamond's address as the second argument. But we are using an ABI from the MyUsefulFacet facet so we can call functions that are defined in that facet. MyUsefulFacet's functions must have been added to the diamond (using diamondCut) in order for the diamond to use the function information provided by the ABI of course.

Similarly you need to use the ABI of a facet in Solidity code in order to call functions from a diamond. Here's an example of Solidity code that calls a function from a diamond:

```solidity
string result = MyUsefulFacet(diamondAddress).getResult()
```

## Get Help and Join the Community

If you need help or would like to discuss diamonds then send me a message [on twitter](https://twitter.com/mudgen), or [email me](mailto:nick@perfectabstractions.com). Or join the [Diamond Standard Discord server](https://discord.gg/kQewPw2).

## Useful Links

1. [EIP-2535 Diamonds](https://github.com/ethereum/EIPs/issues/2535)
1. [Understanding Diamonds on Ethereum](https://dev.to/mudgen/understanding-diamonds-on-ethereum-1fb)
1. [Solidity Storage Layout For Proxy Contracts and Diamonds](https://medium.com/1milliondevs/solidity-storage-layout-for-proxy-contracts-and-diamonds-c4f009b6903)
1. [New Storage Layout For Proxy Contracts and Diamonds](https://medium.com/1milliondevs/new-storage-layout-for-proxy-contracts-and-diamonds-98d01d0eadb)
1. [Upgradeable smart contracts using the Diamond Standard](https://hiddentao.com/archives/2020/05/28/upgradeable-smart-contracts-using-diamond-standard)
1. [buidler-deploy supports diamonds](https://github.com/wighawag/buidler-deploy/)

## Author

This example implementation was written by Nick Mudge.

Contact:

- https://twitter.com/mudgen
- nick@perfectabstractions.com

## License

MIT license. See the license file.
Anyone can use or modify this software for their purposes.

