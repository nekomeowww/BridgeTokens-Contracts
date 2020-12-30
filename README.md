<h1 align="center">
  BridgeToken Contracts
</h1>

<p align="center">
  Solidity Contracts for BridgeTokens
</p>


## Deployment
There are two different set of contracts that are needed to deploy: __release__ and __mint__. And there are two different set of contracts that working in different ways: __multi signed__ and __native__.

The source blockchain side (such as transferring assets from source network __Ethereum__), you will need to deploy [StorageProxy.sol](https://github.com/nekomeowww/BridgeTokens-Contracts/blob/main/contracts/StorageProxy.sol) with [HomeAMBNativeToErc20.sol](https://github.com/nekomeowww/BridgeTokens-Contracts/blob/main/contracts/HomeAMBNativeToErc20.sol) in order to transfer your token. Next step, go to your destination blockchain side (such as recieveing assets to destination network __Binance__), you will need to deploy [ForeignAMBNativeToERC20.sol](https://github.com/nekomeowww/BridgeTokens-Contracts/blob/main/contracts/PermittableToken.sol) to recieve and __mint__ you token from the other side.

Vice-versa. It means you will need to deploy [StorageProxy.sol](https://github.com/nekomeowww/BridgeTokens-Contracts/blob/main/contracts/StorageProxy.sol) with [HomeAMBNativeToERC20.sol](https://github.com/nekomeowww/BridgeTokens-Contracts/blob/main/contracts/HomeAMBNativeToERC20.sol) in __destination blockchain__, deploy [ForeignAMBNativeToERC20.sol](https://github.com/nekomeowww/BridgeTokens-Contracts/blob/main/contracts/PermittableToken.sol) in __source blockchain__ in order to reverse your transfer direction. In reverse direction, your mode will be __release__.

## Set up in App

Inside the src/bridges/ETH_HECO/utils/contracts.ts, you can fill in and setup the contracts for each network with different mode.

#### Ethereum

```
'Ethereum': {
    release: '0x address',  // Foreign, Binance, your destination StorageProxy.sol
    mint: '0x address',  // Home, Ethereum, your source StorageProxy.sol
}
```

#### Binance

```
'Binance': {
    release: '0x address',  // Foreign, Ethereum, your destination StorageProxy.sol
    mint: '0x address',  // Home, Binance, your source StorageProxy.sol
}
```

## ABI

Because of all the call to the HomeAMBNativeToERC20.sol/ForeignAMBNativeToERC20.sol will all be redirected by StorageProxy.sol, so the ABI would be combined at the first time.

You can pick up the following:

#### AMB_NATIVE_ERC20_ABI

```
[
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "fixFailedMessage",
    "inputs": [{ "type": "bytes32", "name": "_messageId" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setBridgeContract",
    "inputs": [{ "type": "address", "name": "_bridgeContract" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "totalSpentPerDay",
    "inputs": [{ "type": "uint256", "name": "_day" }],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "bool", "name": "" }],
    "name": "isInitialized",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setExecutionDailyLimit",
    "inputs": [{ "type": "uint256", "name": "_dailyLimit" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "getCurrentDay",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "pure",
    "payable": false,
    "outputs": [{ "type": "bytes4", "name": "_data" }],
    "name": "getBridgeMode",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "executionDailyLimit",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "mediatorBalance",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "totalExecutedPerDay",
    "inputs": [{ "type": "uint256", "name": "_day" }],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [{ "type": "bool", "name": "" }],
    "name": "initialize",
    "inputs": [
      { "type": "address", "name": "_bridgeContract" },
      { "type": "address", "name": "_mediatorContract" },
      { "type": "uint256[3]", "name": "_dailyLimitMaxPerTxMinPerTxArray" },
      {
        "type": "uint256[2]",
        "name": "_executionDailyLimitExecutionMaxPerTxArray"
      },
      { "type": "uint256", "name": "_requestGasLimit" },
      { "type": "int256", "name": "_decimalShift" },
      { "type": "address", "name": "_owner" },
      { "type": "address", "name": "_feeManager" }
    ],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "fixMediatorBalance",
    "inputs": [{ "type": "address", "name": "_receiver" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "bool", "name": "" }],
    "name": "messageFixed",
    "inputs": [{ "type": "bytes32", "name": "_messageId" }],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "payable",
    "payable": true,
    "outputs": [],
    "name": "relayTokens",
    "inputs": [{ "type": "address", "name": "_receiver" }],
    "constant": false
  },
  {
    "constant": false,
    "inputs": [
      {
        "name": "_contract",
        "type": "address"
      },
      {
        "name": "_receiver",
        "type": "address"
      },
      {
        "name": "",
        "type": "uint256"
      }
    ],
    "name": "relayTokens",
    "outputs": [],
    "payable": false,
    "stateMutability": "payable",
    "type": "function"
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setFeeManagerContract",
    "inputs": [{ "type": "address", "name": "_feeManager" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "dailyLimit",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "claimTokens",
    "inputs": [
      { "type": "address", "name": "_token" },
      { "type": "address", "name": "_to" }
    ],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setMediatorContractOnOtherSide",
    "inputs": [{ "type": "address", "name": "_mediatorContract" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "address", "name": "" }],
    "name": "mediatorContractOnOtherSide",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "bool", "name": "" }],
    "name": "withinExecutionLimit",
    "inputs": [{ "type": "uint256", "name": "_amount" }],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "executionMaxPerTx",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "handleBridgedTokens",
    "inputs": [
      { "type": "address", "name": "_recipient" },
      { "type": "uint256", "name": "_value" }
    ],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "address", "name": "" }],
    "name": "owner",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "maxAvailablePerTx",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "requestFailedMessageFix",
    "inputs": [{ "type": "bytes32", "name": "_messageId" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "pure",
    "payable": false,
    "outputs": [
      { "type": "uint64", "name": "major" },
      { "type": "uint64", "name": "minor" },
      { "type": "uint64", "name": "patch" }
    ],
    "name": "getBridgeInterfacesVersion",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setMinPerTx",
    "inputs": [{ "type": "uint256", "name": "_minPerTx" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setDailyLimit",
    "inputs": [{ "type": "uint256", "name": "_dailyLimit" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "requestGasLimit",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setMaxPerTx",
    "inputs": [{ "type": "uint256", "name": "_maxPerTx" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "address", "name": "" }],
    "name": "bridgeContract",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "int256", "name": "" }],
    "name": "decimalShift",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "address", "name": "" }],
    "name": "feeManagerContract",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "minPerTx",
    "inputs": [],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "bool", "name": "" }],
    "name": "withinLimit",
    "inputs": [{ "type": "uint256", "name": "_amount" }],
    "constant": true
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setExecutionMaxPerTx",
    "inputs": [{ "type": "uint256", "name": "_maxPerTx" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "transferOwnership",
    "inputs": [{ "type": "address", "name": "newOwner" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "nonpayable",
    "payable": false,
    "outputs": [],
    "name": "setRequestGasLimit",
    "inputs": [{ "type": "uint256", "name": "_requestGasLimit" }],
    "constant": false
  },
  {
    "type": "function",
    "stateMutability": "view",
    "payable": false,
    "outputs": [{ "type": "uint256", "name": "" }],
    "name": "maxPerTx",
    "inputs": [],
    "constant": true
  },
  { "type": "fallback", "stateMutability": "payable", "payable": true },
  {
    "type": "event",
    "name": "FeeDistributed",
    "inputs": [
      { "type": "uint256", "name": "feeAmount", "indexed": false },
      { "type": "bytes32", "name": "messageId", "indexed": true }
    ],
    "anonymous": false
  },
  {
    "type": "event",
    "name": "FailedMessageFixed",
    "inputs": [
      { "type": "bytes32", "name": "messageId", "indexed": true },
      { "type": "address", "name": "recipient", "indexed": false },
      { "type": "uint256", "name": "value", "indexed": false }
    ],
    "anonymous": false
  },
  {
    "type": "event",
    "name": "TokensBridged",
    "inputs": [
      { "type": "address", "name": "recipient", "indexed": true },
      { "type": "uint256", "name": "value", "indexed": false },
      { "type": "bytes32", "name": "messageId", "indexed": true }
    ],
    "anonymous": false
  },
  {
    "type": "event",
    "name": "DailyLimitChanged",
    "inputs": [{ "type": "uint256", "name": "newLimit", "indexed": false }],
    "anonymous": false
  },
  {
    "type": "event",
    "name": "ExecutionDailyLimitChanged",
    "inputs": [{ "type": "uint256", "name": "newLimit", "indexed": false }],
    "anonymous": false
  },
  {
    "type": "event",
    "name": "OwnershipTransferred",
    "inputs": [
      { "type": "address", "name": "previousOwner", "indexed": false },
      { "type": "address", "name": "newOwner", "indexed": false }
    ],
    "anonymous": false
  }
]
```

#### MULTI_AMB_ERC_ERC_ABI

```
[
  {
    "constant": false,
    "inputs": [
      { "name": "token", "type": "address" },
      { "name": "_value", "type": "uint256" }
    ],
    "name": "relayTokens",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_maxPerTx", "type": "uint256" }
    ],
    "name": "setExecutionMaxPerTx",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "maxPerTx",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [{ "name": "_messageId", "type": "bytes32" }],
    "name": "fixFailedMessage",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [{ "name": "_bridgeContract", "type": "address" }],
    "name": "setBridgeContract",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_amount", "type": "uint256" }
    ],
    "name": "withinLimit",
    "outputs": [{ "name": "", "type": "bool" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_recipient", "type": "address" },
      { "name": "_value", "type": "uint256" }
    ],
    "name": "handleBridgedTokens",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "executionMaxPerTx",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "mediatorBalance",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "isTokenRegistered",
    "outputs": [{ "name": "", "type": "bool" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_dailyLimit", "type": "uint256" }
    ],
    "name": "setDailyLimit",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_bridgeContract", "type": "address" },
      { "name": "_mediatorContract", "type": "address" },
      { "name": "_dailyLimitMaxPerTxMinPerTxArray", "type": "uint256[3]" },
      {
        "name": "_executionDailyLimitExecutionMaxPerTxArray",
        "type": "uint256[2]"
      },
      { "name": "_requestGasLimit", "type": "uint256" },
      { "name": "_owner", "type": "address" }
    ],
    "name": "initialize",
    "outputs": [{ "name": "", "type": "bool" }],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "isInitialized",
    "outputs": [{ "name": "", "type": "bool" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_amount", "type": "uint256" }
    ],
    "name": "withinExecutionLimit",
    "outputs": [{ "name": "", "type": "bool" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "getCurrentDay",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "executionDailyLimit",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "getBridgeMode",
    "outputs": [{ "name": "_data", "type": "bytes4" }],
    "payable": false,
    "stateMutability": "pure",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_messageId", "type": "bytes32" }],
    "name": "messageFixed",
    "outputs": [{ "name": "", "type": "bool" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_to", "type": "address" }
    ],
    "name": "claimTokens",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [{ "name": "_mediatorContract", "type": "address" }],
    "name": "setMediatorContractOnOtherSide",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "maxAvailablePerTx",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_dailyLimit", "type": "uint256" }
    ],
    "name": "setExecutionDailyLimit",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "tokenRegistrationMessageId",
    "outputs": [{ "name": "", "type": "bytes32" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "mediatorContractOnOtherSide",
    "outputs": [{ "name": "", "type": "address" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "owner",
    "outputs": [{ "name": "", "type": "address" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [{ "name": "_messageId", "type": "bytes32" }],
    "name": "requestFailedMessageFix",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "getBridgeInterfacesVersion",
    "outputs": [
      { "name": "major", "type": "uint64" },
      { "name": "minor", "type": "uint64" },
      { "name": "patch", "type": "uint64" }
    ],
    "payable": false,
    "stateMutability": "pure",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "minPerTx",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_from", "type": "address" },
      { "name": "_value", "type": "uint256" },
      { "name": "_data", "type": "bytes" }
    ],
    "name": "onTokenTransfer",
    "outputs": [{ "name": "", "type": "bool" }],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_day", "type": "uint256" }
    ],
    "name": "totalSpentPerDay",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "token", "type": "address" },
      { "name": "_receiver", "type": "address" },
      { "name": "_value", "type": "uint256" }
    ],
    "name": "relayTokens",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "requestGasLimit",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "bridgeContract",
    "outputs": [{ "name": "", "type": "address" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_receiver", "type": "address" }
    ],
    "name": "fixMediatorBalance",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_maxPerTx", "type": "uint256" }
    ],
    "name": "setMaxPerTx",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_minPerTx", "type": "uint256" }
    ],
    "name": "setMinPerTx",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [
      { "name": "_token", "type": "address" },
      { "name": "_day", "type": "uint256" }
    ],
    "name": "totalExecutedPerDay",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [{ "name": "newOwner", "type": "address" }],
    "name": "transferOwnership",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [{ "name": "_requestGasLimit", "type": "uint256" }],
    "name": "setRequestGasLimit",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [{ "name": "_token", "type": "address" }],
    "name": "dailyLimit",
    "outputs": [{ "name": "", "type": "uint256" }],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "anonymous": false,
    "inputs": [
      { "indexed": true, "name": "messageId", "type": "bytes32" },
      { "indexed": false, "name": "token", "type": "address" },
      { "indexed": false, "name": "recipient", "type": "address" },
      { "indexed": false, "name": "value", "type": "uint256" }
    ],
    "name": "FailedMessageFixed",
    "type": "event"
  },
  {
    "anonymous": false,
    "inputs": [
      { "indexed": true, "name": "token", "type": "address" },
      { "indexed": true, "name": "recipient", "type": "address" },
      { "indexed": false, "name": "value", "type": "uint256" },
      { "indexed": true, "name": "messageId", "type": "bytes32" }
    ],
    "name": "TokensBridged",
    "type": "event"
  },
  {
    "anonymous": false,
    "inputs": [
      { "indexed": true, "name": "token", "type": "address" },
      { "indexed": false, "name": "newLimit", "type": "uint256" }
    ],
    "name": "DailyLimitChanged",
    "type": "event"
  },
  {
    "anonymous": false,
    "inputs": [
      { "indexed": true, "name": "token", "type": "address" },
      { "indexed": false, "name": "newLimit", "type": "uint256" }
    ],
    "name": "ExecutionDailyLimitChanged",
    "type": "event"
  },
  {
    "anonymous": false,
    "inputs": [
      { "indexed": false, "name": "previousOwner", "type": "address" },
      { "indexed": false, "name": "newOwner", "type": "address" }
    ],
    "name": "OwnershipTransferred",
    "type": "event"
  }
]
```

