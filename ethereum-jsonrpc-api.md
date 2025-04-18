This is a comprehensive list based on common implementations and specifications found in sources like the official Ethereum documentation, client documentation (like Geth), and developer resources. However, specific client implementations might have slight variations or additional proprietary methods (e.g., `debug_`, `trace_`, `parity_`, `personal_`). Also, the API evolves, so new methods might be added, or existing ones deprecated over time.

**Data Types:**

*   `QUANTITY`: An integer encoded as a hexadecimal string with a `0x` prefix. May have leading zeros (e.g., `0x0`, `0x4a`).
*   `DATA`: A byte array encoded as a hexadecimal string with a `0x` prefix. Must have an even number of hex digits.
*   `ADDRESS`: `DATA`, 20 bytes long.
*   `BLOCK HASH`: `DATA`, 32 bytes long.
*   `TRANSACTION HASH`: `DATA`, 32 bytes long.
*   `Default Block Parameter`: A `QUANTITY` block number, or one of the strings `"earliest"` (genesis block), `"latest"` (latest mined block), or `"pending"` (pending state/transactions). Many read methods accept this as the last parameter.

---

## **Web3 Namespace (`web3_`)**

### `web3_clientVersion`

*   **Description**: Returns the current client version.
*   **Parameters**: None
*   **Returns**: `String` - The current client version (e.g., `"Geth/v1.10.26-stable-e5eb32ac/linux-amd64/go1.18.5"`).

### `web3_sha3`

*   **Description**: Returns the Keccak-256 hash (not the standardized SHA3-256) of the given data.
*   **Parameters**:
    1.  `DATA` - The data to hash.
*   **Returns**: `DATA` - The Keccak-256 hash of the input data.

---

## **Net Namespace (`net_`)**

### `net_version`

*   **Description**: Returns the current network ID.
*   **Parameters**: None
*   **Returns**: `String` - The current network ID (e.g., `"1"` for Mainnet, `"3"` for Ropsten - deprecated, `"5"` for Goerli - deprecated, `"11155111"` for Sepolia).

### `net_listening`

*   **Description**: Returns `true` if the client is actively listening for network connections.
*   **Parameters**: None
*   **Returns**: `Boolean` - `true` when listening, otherwise `false`.

### `net_peerCount`

*   **Description**: Returns the number of peers currently connected to the client.
*   **Parameters**: None
*   **Returns**: `QUANTITY` - Integer of the number of connected peers.

---

## **Eth Namespace (`eth_`)**

### `eth_protocolVersion`

*   **Description**: Returns the current Ethereum protocol version.
*   **Parameters**: None
*   **Returns**: `String` - The Ethereum protocol version (often `"1"` or a hex value like `"0x41"`).

### `eth_syncing`

*   **Description**: Returns an object with data about the sync status or `false` if not syncing.
*   **Parameters**: None
*   **Returns**: `Object | Boolean` - An object with sync status data (`startingBlock`, `currentBlock`, `highestBlock`) or `false`.

### `eth_coinbase`

*   **Description**: Returns the client's coinbase address (the address that receives mining rewards). Note: May not be available or relevant post-Merge depending on client configuration.
*   **Parameters**: None
*   **Returns**: `ADDRESS` - The 20-byte coinbase address.

### `eth_mining`

*   **Description**: Returns `true` if the client is actively mining new blocks. Always `false` post-Merge for standard clients.
*   **Parameters**: None
*   **Returns**: `Boolean` - `true` if mining, otherwise `false`.

### `eth_hashrate`

*   **Description**: Returns the number of hashes per second that the node is mining with. Always `0x0` post-Merge for standard clients.
*   **Parameters**: None
*   **Returns**: `QUANTITY` - Number of hashes per second.

### `eth_gasPrice`

*   **Description**: Returns the current price per gas in wei. Useful for legacy transactions (Type 0). See `eth_maxPriorityFeePerGas` for EIP-1559 transactions.
*   **Parameters**: None
*   **Returns**: `QUANTITY` - Integer of the current gas price in wei.

### `eth_maxPriorityFeePerGas`

*   **Description**: Returns the current EIP-1559 priority fee per gas in wei needed to be included in a block.
*   **Parameters**: None
*   **Returns**: `QUANTITY` - Integer of the current max priority fee per gas in wei.

### `eth_feeHistory`

*   **Description**: Returns historical gas information, enabling users to estimate gas prices.
*   **Parameters**:
    1.  `QUANTITY` - Number of blocks in the requested range. Between 1 and 1024 blocks can be requested in a single query. Less than requested may be returned.
    2.  `QUANTITY | TAG` - Highest block number in the range, or "latest".
    3.  `Array<Float>` - A monotonically increasing list of percentile values between 0 and 100. For each block in the requested range, the transactions will be sorted in ascending order by effective tip per gas and the corresponding effective tip for the percentile will be computed.
*   **Returns**: `Object` - Fee history object containing:
    *   `oldestBlock`: `QUANTITY` - Lowest block number in the range.
    *   `baseFeePerGas`: `Array<QUANTITY>` - Array of block base fees per gas. Includes the next block after the newest of the returned range, because this value can be derived from the newest block. Zeroes are returned for pre-EIP-1559 blocks.
    *   `gasUsedRatio`: `Array<Float>` - Array of block gas used ratios. These are calculated as `gasUsed / gasLimit`.
    *   `reward`: `Array<Array<QUANTITY>>` (Optional) - Array of effective priority fee per gas data points from a single block. All zeroes are returned if the block is empty.

### `eth_accounts`

*   **Description**: Returns a list of addresses owned by the client.
*   **Parameters**: None
*   **Returns**: `Array<ADDRESS>` - Array of 20-byte addresses owned by the client.

### `eth_blockNumber`

*   **Description**: Returns the number of the most recent block.
*   **Parameters**: None
*   **Returns**: `QUANTITY` - Integer of the current block number the client is on.

### `eth_getBalance`

*   **Description**: Returns the balance of the account of a given address.
*   **Parameters**:
    1.  `ADDRESS` - Address to check for balance.
    2.  `QUANTITY | TAG` - Integer block number, or the string `"latest"`, `"earliest"` or `"pending"` (Default: `"latest"`).
*   **Returns**: `QUANTITY` - Integer of the current balance in wei.

### `eth_getStorageAt`

*   **Description**: Returns the value from a storage position at a given address.
*   **Parameters**:
    1.  `ADDRESS` - Address of the storage.
    2.  `QUANTITY` - Integer of the position in the storage (hex encoded).
    3.  `QUANTITY | TAG` - Integer block number, or the string `"latest"`, `"earliest"` or `"pending"` (Default: `"latest"`).
*   **Returns**: `DATA` - The value at this storage position.

### `eth_getTransactionCount`

*   **Description**: Returns the number of transactions sent from an address (nonce). *Note: Post-Pectra fork (EIP-7702), this may not strictly equal the number of sent transactions.*
*   **Parameters**:
    1.  `ADDRESS` - Address.
    2.  `QUANTITY | TAG` - Integer block number, or the string `"latest"`, `"earliest"` or `"pending"` (Default: `"latest"`).
*   **Returns**: `QUANTITY` - Integer of the number of transactions sent from this address (nonce).

### `eth_getBlockTransactionCountByHash`

*   **Description**: Returns the number of transactions in a block matching the given block hash.
*   **Parameters**:
    1.  `BLOCK HASH` - Hash of a block.
*   **Returns**: `QUANTITY` - Integer of the number of transactions in this block.

### `eth_getBlockTransactionCountByNumber`

*   **Description**: Returns the number of transactions in a block matching the given block number.
*   **Parameters**:
    1.  `QUANTITY | TAG` - Integer of a block number, or the string `"latest"`, `"earliest"` or `"pending"`.
*   **Returns**: `QUANTITY` - Integer of the number of transactions in this block.

### `eth_getUncleCountByBlockHash`

*   **Description**: Returns the number of uncles in a block from a block matching the given block hash.
*   **Parameters**:
    1.  `BLOCK HASH` - Hash of a block.
*   **Returns**: `QUANTITY` - Integer of the number of uncles in this block.

### `eth_getUncleCountByBlockNumber`

*   **Description**: Returns the number of uncles in a block matching the given block number.
*   **Parameters**:
    1.  `QUANTITY | TAG` - Integer of a block number, or the string `"latest"`, `"earliest"` or `"pending"`.
*   **Returns**: `QUANTITY` - Integer of the number of uncles in this block.

### `eth_getCode`

*   **Description**: Returns code at a given address.
*   **Parameters**:
    1.  `ADDRESS` - Address.
    2.  `QUANTITY | TAG` - Integer block number, or the string `"latest"`, `"earliest"` or `"pending"` (Default: `"latest"`).
*   **Returns**: `DATA` - The code from the given address. `"0x"` if the account doesn't exist or is not a contract.

### `eth_sign`

*   **Description**: Calculates an Ethereum-specific signature (EIP-191) with: `sign(keccak256("\x19Ethereum Signed Message:\n" + len(message) + message))`. Requires the account to be unlocked. Use `personal_sign` for a more user-friendly signing method.
*   **Parameters**:
    1.  `ADDRESS` - Address to sign with.
    2.  `DATA` - Data to sign.
*   **Returns**: `DATA` - Signature.

### `eth_signTransaction`

*   **Description**: Signs a transaction. Requires the account to be unlocked. The transaction is not sent.
*   **Parameters**:
    1.  `Object` - The transaction object:
        *   `from`: `ADDRESS`
        *   `to`: `ADDRESS` (optional for contract creation)
        *   `gas`: `QUANTITY` (optional, default: 90000)
        *   `gasPrice`: `QUANTITY` (optional, legacy, use `maxFeePerGas` / `maxPriorityFeePerGas` for EIP-1559)
        *   `maxFeePerGas`: `QUANTITY` (optional, EIP-1559)
        *   `maxPriorityFeePerGas`: `QUANTITY` (optional, EIP-1559)
        *   `value`: `QUANTITY` (optional)
        *   `data`: `DATA` (optional)
        *   `nonce`: `QUANTITY` (optional)
        *   `chainId`: `QUANTITY` (optional, EIP-155)
        *   `accessList`: `Array` (optional, EIP-2930)
*   **Returns**: `Object` - Signed transaction object including:
    *   `raw`: `DATA` - RLP encoded signed transaction.
    *   `tx`: `Object` - The original transaction object plus `v`, `r`, `s` signature components.

### `eth_sendTransaction`

*   **Description**: Creates new message call transaction or a contract creation, if the `data` field contains code. Requires the account to be unlocked.
*   **Parameters**:
    1.  `Object` - The transaction object (same fields as `eth_signTransaction`).
*   **Returns**: `TRANSACTION HASH` - The transaction hash, or the zero hash if the transaction is not yet available.

### `eth_sendRawTransaction`

*   **Description**: Submits a pre-signed transaction to the network.
*   **Parameters**:
    1.  `DATA` - The signed transaction data (RLP encoded). For EIP-4844 blob transactions, this must be the network form including blobs, commitments, and proofs.
*   **Returns**: `TRANSACTION HASH` - The transaction hash.

### `eth_call`

*   **Description**: Executes a new message call immediately without creating a transaction on the blockchain. Useful for reading contract data or simulating transactions.
*   **Parameters**:
    1.  `Object` - The transaction object (similar fields to `eth_sendTransaction`, `from` is optional).
    2.  `QUANTITY | TAG` - Integer block number, or the string `"latest"`, `"earliest"` or `"pending"` (Default: `"latest"`).
*   **Returns**: `DATA` - The return value of the executed contract call.

### `eth_estimateGas`

*   **Description**: Generates and returns an estimate of how much gas is necessary to allow the transaction to complete. The estimate may be significantly more than the amount of gas actually used by the transaction.
*   **Parameters**:
    1.  `Object` - The transaction object (similar fields to `eth_call`, `gas` is optional and ignored). `from` is optional but recommended.
    2.  `QUANTITY | TAG` - (Optional) Integer block number, or the string `"latest"`, `"earliest"` or `"pending"` (Default: `"latest"`).
*   **Returns**: `QUANTITY` - The amount of gas estimated.

### `eth_getBlockByHash`

*   **Description**: Returns information about a block by hash.
*   **Parameters**:
    1.  `BLOCK HASH` - Hash of a block.
    2.  `Boolean` - If `true`, it returns the full transaction objects; if `false`, only the hashes of the transactions.
*   **Returns**: `Object | null` - A block object, or `null` when no block was found:
    *   `number`: `QUANTITY | null` - Block number. `null` when pending.
    *   `hash`: `BLOCK HASH | null` - Block hash. `null` when pending.
    *   `parentHash`: `BLOCK HASH`
    *   `nonce`: `DATA | null` - Hash of the generated proof-of-work. `null` when pending. (Legacy PoW field)
    *   `sha3Uncles`: `DATA` - SHA3 of the uncles data in the block.
    *   `logsBloom`: `DATA | null` - The bloom filter for the logs of the block. `null` when pending.
    *   `transactionsRoot`: `DATA` - The root of the transaction trie of the block.
    *   `stateRoot`: `DATA` - The root of the final state trie of the block.
    *   `receiptsRoot`: `DATA` - The root of the receipts trie of the block.
    *   `miner`: `ADDRESS` - The address of the beneficiary to whom the mining rewards were given.
    *   `difficulty`: `QUANTITY` - Integer of the difficulty for this block.
    *   `totalDifficulty`: `QUANTITY` - Integer of the total difficulty of the chain until this block.
    *   `extraData`: `DATA`
    *   `size`: `QUANTITY` - The size of this block in bytes.
    *   `gasLimit`: `QUANTITY` - The maximum gas allowed in this block.
    *   `gasUsed`: `QUANTITY` - The total used gas by all transactions in this block.
    *   `timestamp`: `QUANTITY` - The unix timestamp for when the block was collated.
    *   `transactions`: `Array<TRANSACTION HASH | Object>` - Array of transaction objects or hashes.
    *   `uncles`: `Array<BLOCK HASH>` - Array of uncle hashes.
    *   `baseFeePerGas`: `QUANTITY` (Optional, EIP-1559) - Base fee per gas.
    *   `mixHash`: `DATA` (Optional, PoW related)
    *   `withdrawalsRoot`: `DATA` (Optional, EIP-4895) - Root of the withdrawals trie of the block.
    *   `withdrawals`: `Array<Object>` (Optional, EIP-4895) - Array of withdrawal objects.
    *   `blobGasUsed`: `QUANTITY` (Optional, EIP-4844)
    *   `excessBlobGas`: `QUANTITY` (Optional, EIP-4844)
    *   `parentBeaconBlockRoot`: `DATA` (Optional, EIP-4788)

### `eth_getBlockByNumber`

*   **Description**: Returns information about a block by block number.
*   **Parameters**:
    1.  `QUANTITY | TAG` - Integer of a block number, or the string `"latest"`, `"earliest"` or `"pending"`.
    2.  `Boolean` - If `true`, it returns the full transaction objects; if `false`, only the hashes of the transactions.
*   **Returns**: `Object | null` - A block object (same structure as `eth_getBlockByHash`), or `null` if the block wasn't found.

### `eth_getTransactionByHash`

*   **Description**: Returns the information about a transaction requested by transaction hash.
*   **Parameters**:
    1.  `TRANSACTION HASH` - Hash of a transaction.
*   **Returns**: `Object | null` - A transaction object, or `null` when no transaction was found:
    *   `blockHash`: `BLOCK HASH | null` - Hash of the block where this transaction was in. `null` when pending.
    *   `blockNumber`: `QUANTITY | null` - Block number where this transaction was in. `null` when pending.
    *   `from`: `ADDRESS` - Address of the sender.
    *   `gas`: `QUANTITY` - Gas provided by the sender.
    *   `gasPrice`: `QUANTITY` - Gas price provided by the sender in wei (legacy).
    *   `maxFeePerGas`: `QUANTITY` (Optional, EIP-1559)
    *   `maxPriorityFeePerGas`: `QUANTITY` (Optional, EIP-1559)
    *   `hash`: `TRANSACTION HASH` - Hash of the transaction.
    *   `input`: `DATA` - The data sent along with the transaction.
    *   `nonce`: `QUANTITY` - The number of transactions made by the sender prior to this one.
    *   `to`: `ADDRESS | null` - Address of the receiver. `null` when it's a contract creation transaction.
    *   `transactionIndex`: `QUANTITY | null` - Integer of the transaction's index position in the block. `null` when pending.
    *   `value`: `QUANTITY` - Value transferred in wei.
    *   `type`: `QUANTITY` - Transaction type. `0x0` for legacy, `0x1` for EIP-2930, `0x2` for EIP-1559, `0x3` for EIP-4844.
    *   `accessList`: `Array` (Optional, EIP-2930/EIP-1559)
    *   `chainId`: `QUANTITY` (Optional, EIP-155)
    *   `v`, `r`, `s`: `QUANTITY` - ECDSA signature components.
    *   `yParity`: `QUANTITY` (Optional, EIP-1559) - Same as `v`.
    *   `blobVersionedHashes`: `Array<DATA>` (Optional, EIP-4844)
    *   `maxFeePerBlobGas`: `QUANTITY` (Optional, EIP-4844)

### `eth_getTransactionByBlockHashAndIndex`

*   **Description**: Returns information about a transaction by block hash and transaction index position.
*   **Parameters**:
    1.  `BLOCK HASH` - Hash of a block.
    2.  `QUANTITY` - Integer of the transaction index position.
*   **Returns**: `Object | null` - A transaction object (same structure as `eth_getTransactionByHash`), or `null`.

### `eth_getTransactionByBlockNumberAndIndex`

*   **Description**: Returns information about a transaction by block number and transaction index position.
*   **Parameters**:
    1.  `QUANTITY | TAG` - A block number, or the string `"latest"`, `"earliest"` or `"pending"`.
    2.  `QUANTITY` - The transaction index position.
*   **Returns**: `Object | null` - A transaction object (same structure as `eth_getTransactionByHash`), or `null`.

### `eth_getTransactionReceipt`

*   **Description**: Returns the receipt of a transaction by transaction hash. Note: The receipt is not available for pending transactions.
*   **Parameters**:
    1.  `TRANSACTION HASH` - Hash of a transaction.
*   **Returns**: `Object | null` - A transaction receipt object, or `null` if no receipt was found:
    *   `transactionHash`: `TRANSACTION HASH` - Hash of the transaction.
    *   `transactionIndex`: `QUANTITY` - Integer of the transaction's index position in the block.
    *   `blockHash`: `BLOCK HASH` - Hash of the block where this transaction was in.
    *   `blockNumber`: `QUANTITY` - Block number where this transaction was in.
    *   `from`: `ADDRESS` - Address of the sender.
    *   `to`: `ADDRESS | null` - Address of the receiver. `null` when it's a contract creation transaction.
    *   `cumulativeGasUsed`: `QUANTITY` - Total amount of gas used when this transaction was executed in the block.
    *   `effectiveGasPrice`: `QUANTITY` - The actual gas price paid for this transaction in wei.
    *   `gasUsed`: `QUANTITY` - The amount of gas used by this specific transaction alone.
    *   `contractAddress`: `ADDRESS | null` - The contract address created, if the transaction was a contract creation, otherwise `null`.
    *   `logs`: `Array<Object>` - Array of log objects, which this transaction generated.
        *   `address`: `ADDRESS` - Address from which this log originated.
        *   `topics`: `Array<DATA>` - Array of 0 to 4 32-byte DATA of indexed log arguments.
        *   `data`: `DATA` - Contains the non-indexed arguments of the log.
        *   `blockNumber`: `QUANTITY`
        *   `transactionHash`: `TRANSACTION HASH`
        *   `transactionIndex`: `QUANTITY`
        *   `blockHash`: `BLOCK HASH`
        *   `logIndex`: `QUANTITY`
        *   `removed`: `Boolean` - `true` when the log was removed due to a chain reorganization. `false` if it's a valid log.
    *   `logsBloom`: `DATA` - Bloom filter for light clients to quickly retrieve related logs.
    *   `type`: `QUANTITY` - Transaction type.
    *   `status`: `QUANTITY` - Either `0x1` (success) or `0x0` (failure).
    *   `root`: `DATA` (Optional, pre-Byzantium) - Post-transaction state root.
    *   `blobGasUsed`: `QUANTITY` (Optional, EIP-4844)
    *   `blobGasPrice`: `QUANTITY` (Optional, EIP-4844)

### `eth_getUncleByBlockHashAndIndex`

*   **Description**: Returns information about an uncle of a block by hash and uncle index position.
*   **Parameters**:
    1.  `BLOCK HASH` - Hash of a block.
    2.  `QUANTITY` - The uncle's index position.
*   **Returns**: `Object | null` - A block object (same structure as `eth_getBlockByHash`, uncle blocks don't have `transactions` or `uncles` fields), or `null`.

### `eth_getUncleByBlockNumberAndIndex`

*   **Description**: Returns information about an uncle of a block by number and uncle index position.
*   **Parameters**:
    1.  `QUANTITY | TAG` - A block number, or the string `"latest"`, `"earliest"` or `"pending"`.
    2.  `QUANTITY` - The uncle's index position.
*   **Returns**: `Object | null` - A block object (same structure as `eth_getBlockByHash`, uncle blocks don't have `transactions` or `uncles` fields), or `null`.

### `eth_newFilter`

*   **Description**: Creates a filter object, based on filter options, to notify when the state changes (logs). To check if the state has changed, call `eth_getFilterChanges`.
*   **Parameters**:
    1.  `Object` - The filter options:
        *   `fromBlock`: `QUANTITY | TAG` (optional, default: `"latest"`) - Integer block number, or `"latest"`, `"earliest"`, `"pending"`.
        *   `toBlock`: `QUANTITY | TAG` (optional, default: `"latest"`) - Integer block number, or `"latest"`, `"earliest"`, `"pending"`.
        *   `address`: `ADDRESS | Array<ADDRESS>` (optional) - Contract address or a list of addresses from which logs should originate.
        *   `topics`: `Array<DATA | Array<DATA> | null>` (optional) - Array of DATA topics. Topics are order-dependent. Each topic can also be an array of DATA with "or" options. `null` matches any topic in that position.
*   **Returns**: `QUANTITY` - A filter ID.

### `eth_newBlockFilter`

*   **Description**: Creates a filter in the node, to notify when a new block arrives. To check if the state has changed, call `eth_getFilterChanges`.
*   **Parameters**: None
*   **Returns**: `QUANTITY` - A filter ID.

### `eth_newPendingTransactionFilter`

*   **Description**: Creates a filter in the node, to notify when new pending transactions arrive. To check if the state has changed, call `eth_getFilterChanges`.
*   **Parameters**: None
*   **Returns**: `QUANTITY` - A filter ID.

### `eth_uninstallFilter`

*   **Description**: Uninstalls a filter with the given ID. Should always be called when watch is no longer needed. Additionally, filters timeout when they aren't requested with `eth_getFilterChanges` for a period of time.
*   **Parameters**:
    1.  `QUANTITY` - The filter ID.
*   **Returns**: `Boolean` - `true` if the filter was successfully uninstalled, otherwise `false`.

### `eth_getFilterChanges`

*   **Description**: Polling method for a filter, which returns an array of logs which occurred since the last poll.
*   **Parameters**:
    1.  `QUANTITY` - The filter ID.
*   **Returns**: `Array` - Array of log objects (for `eth_newFilter`), block hashes (`DATA`, for `eth_newBlockFilter`), or transaction hashes (`TRANSACTION HASH`, for `eth_newPendingTransactionFilter`) that have occurred since the last poll.

### `eth_getFilterLogs`

*   **Description**: Returns an array of all logs matching filter with the given ID.
*   **Parameters**:
    1.  `QUANTITY` - The filter ID.
*   **Returns**: `Array<Object>` - Array of log objects matching the filter.

### `eth_getLogs`

*   **Description**: Returns an array of all logs matching a given filter object.
*   **Parameters**:
    1.  `Object` - The filter options (same as `eth_newFilter`).
*   **Returns**: `Array<Object>` - Array of log objects matching the filter.

### `eth_getWork`

*   **Description**: Returns the hash of the current block, the seedHash, and the boundary condition to be met ("target"). Relevant for Proof-of-Work; likely deprecated/unused post-Merge.
*   **Parameters**: None
*   **Returns**: `Array<DATA>` - Array with:
    1.  `DATA` - Current block header pow-hash.
    2.  `DATA` - Seed hash used for the DAG.
    3.  `DATA` - Boundary condition ("target").

### `eth_submitWork`

*   **Description**: Used for submitting a proof-of-work solution. Relevant for Proof-of-Work; likely deprecated/unused post-Merge.
*   **Parameters**:
    1.  `DATA` - The nonce found (8 bytes).
    2.  `DATA` - The header's pow-hash (32 bytes).
    3.  `DATA` - The mix digest (32 bytes).
*   **Returns**: `Boolean` - `true` if the provided solution is valid, otherwise `false`.

### `eth_submitHashrate`

*   **Description**: Used for submitting mining hashrate. Relevant for Proof-of-Work; likely deprecated/unused post-Merge.
*   **Parameters**:
    1.  `QUANTITY` - Hashrate, a hexadecimal number.
    2.  `DATA` - ID, an arbitrary identifier for the miner.
*   **Returns**: `Boolean` - `true` if submitting went through, otherwise `false`.

### `eth_createAccessList`

*   **Description**: Creates an EIP-2930 access list for a transaction. This method is useful for estimating the access list required for a transaction execution.
*   **Parameters**:
    1.  `Object` - The transaction object (similar fields to `eth_call`).
    2.  `QUANTITY | TAG` - (Optional) Integer block number, or the string `"latest"`, `"earliest"` or `"pending"` (Default: `"latest"`).
*   **Returns**: `Object` - Access list object:
    *   `accessList`: `Array<Object>` - The access list.
        *   `address`: `ADDRESS`
        *   `storageKeys`: `Array<DATA>`
    *   `gasUsed`: `QUANTITY` - Gas used by the estimation call.
    *   `error`: `String` (Optional) - Error message if estimation failed.

### `eth_getProof`

*   **Description**: Returns the Merkle proof for a given account and optionally some storage keys at a specific block state.
*   **Parameters**:
    1.  `ADDRESS` - Address of the account.
    2.  `Array<DATA>` - Array of storage keys to prove.
    3.  `QUANTITY | TAG` - Integer block number, or the string `"latest"`, `"earliest"` or `"pending"` (Default: `"latest"`).
*   **Returns**: `Object` - Account proof object:
    *   `balance`: `QUANTITY`
    *   `codeHash`: `DATA`
    *   `nonce`: `QUANTITY`
    *   `storageHash`: `DATA`
    *   `accountProof`: `Array<DATA>` - Array of RLP-serialized Merkle Tree nodes which constitute the proof.
    *   `storageProof`: `Array<Object>` - Array of storage proofs.
        *   `key`: `DATA`
        *   `value`: `QUANTITY`
        *   `proof`: `Array<DATA>` - Array of RLP-serialized Merkle Tree nodes.

### `eth_blobBaseFee`

*   **Description**: Returns the base fee per blob gas in wei. (EIP-4844)
*   **Parameters**: None
*   **Returns**: `QUANTITY` - The base fee per blob gas in wei.

### `eth_getBlockReceipts`

*   **Description**: Returns the receipts for all transactions in a specified block.
*   **Parameters**:
    1.  `QUANTITY | TAG` - Integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.
*   **Returns**: `Array<Object> | null` - An array of transaction receipt objects (same structure as `eth_getTransactionReceipt`), or `null` if the block is not found.

---

**Important Considerations:**

*   **Completeness:** This list aims to be comprehensive for the standard `eth_`, `net_`, and `web3_` namespaces based on common specifications. However, specific Ethereum clients (Geth, Nethermind, Besu, Erigon) might implement slightly different sets or include additional namespaces (like `debug_`, `trace_`, `txpool_`, `miner_`, `admin_`, `personal_`). Always refer to the documentation of the specific client you are interacting with for the most accurate information.
*   **Engine API:** There's also an "Engine API" used for communication between consensus and execution clients post-Merge, which has methods like `engine_newPayloadV1`, `engine_forkchoiceUpdatedV1`, etc. These are typically not exposed for general dApp interaction.
*   **Deprecated Methods:** Some methods related to PoW (like `eth_getWork`, `eth_submitWork`, `eth_hashrate`, `eth_mining`) are effectively obsolete on the mainnet and PoS testnets.
*   **EIPs:** New EIPs constantly update the protocol and may add or modify RPC methods (e.g., EIP-1559 added `eth_maxPriorityFeePerGas` and `eth_feeHistory`, EIP-4844 added blob-related fields and `eth_blobBaseFee`).
