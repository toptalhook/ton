## 2023.06 Update
1. (disabled by default) New deflation mechanisms: partial fee burning and blackhole address
2. Storage-contract improvement

Besides the work of the core team, this update is based on the efforts of @DearJohnDoe from Tonbyte (Storage-contract improvement).

## 2023.05 Update
1. Archive manager optimization
2. A series of catchain (basic consensus protocol) security improvements
3. Update for Fift libraries and FunC: better error-handling, fixes for `catch` stack recovery
4. A series of out message queue handling optimization (already deployed during emergency upgrades between releases)
5. Improvement of binaries portability

Besides the work of the core team, this update is based on the efforts of @aleksej-paschenko (portability improvement), [Disintar team](https://github.com/disintar/) (archive manager optimization) and [sec3-service](https://github.com/sec3-service) security auditors (funC improvements).

## 2023.04 Update
1. CPU load optimization: previous DHT reconnect policy was too aggressive
2. Network throughput improvements: granular control on external message broadcast, optimize celldb GC, adjust state serialization and block downloading timings, rldp2 for states and archives 
3. Update for Fift (namespaces) and Fift libraries (list of improvements: https://github.com/ton-blockchain/ton/issues/631)
4. Better handling of incorrect inputs in funC: fix UB and prevent crashes on some inputs, improve optimizing int consts and unused variables in FunC, fix analyzing repeat loop. FunC version is increase to 0.4.3.
5. `listBlockTransactionsExt` in liteserver added
6. Tvm emulator improvements

Besides the work of the core team, this update is based on the efforts of @krigga (tvm emulator improvement), @ex3ndr (`PUSHSLICE` fift-asm improvement) and [sec3-service](https://github.com/sec3-service) security auditors (funC improvements).

## 2023.03 Update
1. Improvement of ADNL connection stability
2. Transaction emulator support and getAccountStateByTransaction method
3. Fixes of typos, undefined behavior and timer warnings
4. Handling incorrect integer literal values in funC; funC version bumped to 0.4.2
5. FunC Mathlib

## 2023.01 Update
1. Added ConfigParam 44: `SuspendedAddressList`. Upon being set this config suspends initialisation of **uninit** addresses from the list for given time.
2. FunC: `v0.4.1` added pragmas for precise control of computation order
3. FunC: fixed compiler crashes for some exotic inputs
4. FunC: added legacy tester, a collection of smart-contracts which is used to check whether compilator update change compilation result
5. Improved archive manager: proper handling of recently garbage-collected blocks

## 2022.12 Update
Node update:
1. Improvements of ton-proxy: fixed few bugs, improved stability
2. Improved collator/validator checks, added optimization of storage stat calculation, generation and validation of new blocks is made safer
3. Some previously hard-coded parameters such as split/merge timings, max sizes and depths of internal and external messages, and others now can be updated by validators through setting ConfigParams. Max contract size added to configs.
4. Tonlib: updated raw.getTransactions (now it contains InitState), fixed long bytestrings truncation
5. abseil-cpp is updated to newer versions
6. Added configs for Token Bridge
7. LiteServers: a few bug fixes, added liteServer.getAccountStatePrunned method, improved work with not yet applied blocks.
8. Improved DHT: works for some NAT configurations, optimized excessive requests, added option for DHT network segregation.
9. FunC v0.4.0: added try/catch statements, added throw_arg functions, allowed in-place modification of global variables, forbidden ambiguous modification of local variables after it's usage in the same expression.
10. TON Storage: added storage-daemon (create, download bag of Files, storage-provider staff), added storage-daemon-cli

Besides the work of the core team, this update is based on the efforts of @vtamara (help with abseil-cpp upgrade), @krigga(in-place modification of global variables) and third-party security auditors.

## 2022.10 Update
* Added extended block creation and general perfomance stats gathering
* Forbidden report data on blocks not committed to the master chain for LS
* Improved debug in TVM
* FunC 0.3.0: multi-line asms, bitwise operations for constants, duplication of identical definition for constants and asms now allowed
* New tonlib methods: sendMessageReturnHash, getTransactionsV2, getMasterchainBlockSignatures, getShardBlockProof, getLibraries.
* Fixed bugs related to invalid TVM output (c4, c5, libaries) and non-validated network data; avoided too deep recursion in libraries loading
* Fixed multiple undefined behavior issues
* Added build of FunC and Fift to WASM

Besides the work of the core team, this update is based on the efforts of @tvorogme (debug improvements), @AlexeyFSL (WASM builds)  and third-party security auditors.

## 2022.08 Update
* Blockchain state serialization now works via separate db-handler which simplfies memory clearing after serialization
* CellDB now works asynchronously which substantially increase database access throughput
* Abseil-cpp and crc32 updated: solve issues with compilation on recent OS distributives
* Fixed a series of UBs and issues for exotic endianness hosts
* Added detailed network stats for overlays (can be accessed via `validator-console`)
* Improved auto-builds for wide range of systems.
* Added extended error information for unaccepted external messages: `exit_code` and TVM trace (where applicable).
* [Improved catchain DoS resistance](https://github.com/ton-blockchain/ton/blob/master/doc/catchain-dos.md)
* A series of FunC improvements, summarized [here](https://github.com/ton-blockchain/ton/pull/378)
#### Update delay
Update coincided with persistent state serialization event which lead to block production speed deterioration (issue substantially mitigated in update itself). This phenomena was aggravated by the fact that after update some validators lost ability to participate in block creation. The last was caused by threshold based hardcoded protocol version bump, where threshold was set in such manner (based on block height with value higher than 9m), that it eluded detection in private net tests. The update was temporarily paused and resumed after persistent state serialization ended and issues with block creation were resolved.

Besides the work of the core team, this update is based on the efforts of @awesome-doge (help with abseil-cpp upgrade), @rec00rsiff (noted issues for exotic endianess and implemented network stats) and third-party security auditors.

## 2022.05 Update
* Initial synchronization improved: adjusted timeouts for state download and the way of choosing which state to download. Nodes with low network speed and/or bad connectivity will synchronize faster and consistently.
* Improved peer-to-peer network stability and DDoS resistance: now peers will only relay valid messages to the network. Large messages, which require splitting for relaying, will be retranslated as well, but only after the node gets all parts, and reassembles and checks them. Validators may sign certificates for network peers, which allow relaying large messages by parts without checks. It is used now by validators to faster relay new blocks. Sign and import certificate commands are exposed via `validator-engine-console`.
* Fixed some rare edge cases in TVM arithmetic operations related to big numbers (`2**63+`)
* Improved fixes used to combat wrong activate-destruct-activate contract behavior last November.
* Improved tonlib: support libraries (with client-side caching), getmethods completely fill c7 register, getmethods support slice arguments, improved messages listing for transactions, added extended block header params, added getConfig method.
* RocksDB updated to a newer version.
* Improved persistent state serialization: memory usage during serialization was optimized; the start of serialization on different nodes was sparsed.
* FunC update: support for string literals and constants (including precompiled constant expressions), semver, `include` expressions.
* Fixed rarely manifested bugs in `Asm.fif`.
* LiteClient supports key as cli parameter.
* Improved Liteserver DoS resistance for running getmethods.

Besides the work of the core team, this update is based on the efforts of @tvorogme (added support for slice arguments and noted bugs in Asm.fif), @akifoq (fixed bug in Asm.fif), @cryshado (noted strange behavior of LS, which, upon inspection, turned out to be a vector of DoS attack).


