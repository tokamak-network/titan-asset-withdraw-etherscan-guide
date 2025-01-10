# Etherscan Guide: Finalizing Your Withdrawals

## Introduction

Welcome to our step-by-step guide for finalizing your Titan Network withdrawal transactions. While the Tokamak Bridge interface is no longer available, this guide will help you complete your pending withdrawals through Etherscan.

For more information about the Titan Network sunset, please refer to our [official announcement](https://medium.com/tokamak-network/tokamak-network-to-sunset-titan-b4471019c92).

> **Important Note**: If you have multiple pending withdrawals, each transaction must be finalized separately. The estimated gas cost per finalization is approximately ... ETH.

## Steps

1. Go to the L1StandardBridge contract in etherscan and click `Contract` → `Read as Proxy`.
   ![image](/assets/finalize/image01.png)
2. Connect your account into etherscan clicking `Connect to Web3`.
   ![image](/assets/finalize/image02.png)
3. Search the address of your account where you have pending withdraw transactions in [finalWithdrawInputs.json](https://github.com/tokamak-network/legacy-titan-pending-tx-fetcher/blob/main/storage/sepolia/1734932537532/finalWithdrawalInputs.json) file.
   ![image](/assets/finalize/image03.png)
4. Check if the withdrawal has been already finalized or not:

   - Query `failedMessages` (2nd query) with `msgHash` from what you’ve searched and it should be `false`.
     ![image](/assets/finalize/image04.png)
     ![image](/assets/finalize/image10.png)
   - Query `successfulMessages` (8th query) with `msgHash` from what you’ve searched and it should be `false`.
     ![image](/assets/finalize/image05.png)

   - Both of them should be `false` or it means that withdrawal has been already done and can’t continue the process for that withdrawal. Please give it a try with another withdrawal.

5. Click `Write as Proxy` and connect your account.
   ![image](/assets/finalize/image06.png)
6. Input the parameters from step 3 into the `relayMessage` method (6th method):

   > **⚠️Note**: For the `_proof` parameter, include the entire value with double quotes (`"`)
   > Example input parameters:

   - `_target` (address):

     - From JSON: "target"
     - Input value: `0xfb2a94f731ade55f8b8bef7836ecb423bea576b1`

   - `_sender` (address):

     - From JSON: "sender"
     - Input value: `0x4200000000000000000000000000000000000010`

   - `_message` (bytes):

     - From JSON: "message"
     - Input value: `0x1532ec34000000000000000000000000fb2a94f731ade55f8b8bef7836ecb423bea576b1000000000000000000000000fb2a94f731ade55f8b8bef7836ecb423bea576b100000000000000000000000000000000000000000000000000038d7ea4c6800000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000000`

   - `_messageNonce` (uint256):

     - From JSON: "messageNonce"
     - Input value: `100081`

   - `_proof` (tuple):

     1. stateRoot (From JSON: "proof → stateRoot"):

     ```json
     "0x894eee0eee3c42c51a8d238211591c5ca162c5fa33c2f34dd421bacea5d74e9b"
     ```

     2. stateRootBatchHeader (From JSON: "proof → stateRootBatchHeader"):

     ```json
     {
       "batchIndex": "1574",
       "batchRoot": "0x4b36b294054f95ca688dea28b7c78f07335dd1555d9a7cd962670f58ce65b0de",
       "batchSize": "34",
       "prevTotalElements": "15498",
       "extraData": "0x0000000000000000000000000000000000000000000000000000000067127f7c0000000000000000000000006bcb3b9ee42bcbe3c67ad3e6d28ca5a258677e98"
     }
     ```

     3. stateRootProof (From JSON: "proof → stateRootProof"):

     ```json
     {
       "index": 3,
       "siblings": [
         "0xfb64610ba52382fd9e249c44606dba827524c8a26b6bd6da000fce1c93039196",
         "0xfaf050ece9d2c849c04a1eb4e4c2ec1ec8a1fc83a9b0d9735b7c516b36f67e05",
         "0x34f348f14c516ed35389f55d117c54ce250a7d2353467efdb4562aff4a878172",
         "0xdb15b4222eb48137d6510c0bd97f31c4ee21ab03a7f06534c7ce44823dfb58f8",
         "0x6f4fd8f04103bd3c17d4eefe219fc31abd928dfffd2393b8259513bfb4242beb",
         "0xcceda7674a82e06ce50a73ec5d14631c5c5fe6d9d8b11487c2af3500b75e38db"
       ]
     }
     ```

     4. stateTrieWitness: (From JSON: "proof → stateTrieWitness") →

     ```json
     "0xf904efb90214f90211a03fb88c69592133351a798cbfc39feec2049078ffacf6f38d775731662692a3a9a0e548c430b4027273b4a684fd6495e49e0278751fc24a3ca784557b7b00572cfca0efb33d2663bd76ac31a54b79488de113152d16c144056c5620ed8485323ef002a03673784db98dd8b71c116115ad70069387181f5e8268da9fc8d33e6b51249ea9a0320d7df0551bd8fdb4045096a2a274ea43e707cb973117736787e60b657805dfa0d8b9611717f94792a1847619f04fd82bf10b24ec121a1e3082afd10db82bd84ea0d3dfdb80b8a26eefda67508a858fb91bc0e109279e6f21a61b942e227d67dd8ca0dd88be7d2ec083175f560d512ec0286a606fcada0d44426c123b4cc86094da96a03491f882b0bc5bb2cb99d0c88a8b9ad69e5e3a09b288ec3e1f2154780a4befc0a0c304a5d6cd681207b8f4c6abab8e2255de05a5a4587c8085bbead9344a3490bfa037d45fb80ec6edd3c8b9747b0a026685e0f157e48aba4f237b69621243207d79a0c95a2d92005bfcc91e510864022e21931937ea54c87f1fa4f32f2bba972ecfa1a06dc5f46231efbde848548ff3766ac287d0d3620f132cd653a5bcd23a1179fe24a06a49cff0abffed8149c04f6e2a09e2a7030ca15b58b166cb8a64e5f407625188a0d07b201dc6505ef45e6c8ab9dc2677507e27ca3fa9d0876f3611865e760a5821a07705e13fb01037d2e99a8338160cd8816c32da5bb60d9669029468fe532a459580b901f4f901f1a0d079cfe59edf4e050e0c99493dd971dcaf9d427701d66c27c67d224357d9e736a0bc4b52e8684ee1b6831a12bcda5b69556bb816619d0316eec66ccc48f3ceb8c4a0059897f4155192a77bf0fbea95a421194e54b3cf9f44be03610f38aafe259f95a089c9510284ff81e1162afb8a08711a4c5770a60048e018adf6453e487a38205ba0a0575d75ca1231fade726e7751bad0e2d51e84c66b13a044e82e8e9ef6b697eda0674be06c6363a91570f5eb001f9857566392bfa70c7605ed218aad5452d0113ca0e17bea0041b17969b90667da6bc316238baddb50b44502d84369c28972ec965ba0eac955e1d8dd96a0c11104f960168a61edd32d9336230713dc2cb4eb5d36c95aa003a91c60b25c48b252bfe0553f0c8df0a3b3d6cb7dc5080001c0d362ab918c8aa01d5fadef7fd46702d1d6a996997d2ae8d3869cf198662a7a9c3900de8a729473a05e81795dd27cdb5c56ea2283c9eb40e8f75158360d7e1c990cedb2cc75d23a78a048497716db09bd346e6b133e6f5d121fb6856561ed51331867625321b46f5bff80a023dca619295c4fa1cd866fc7daaf2882f00811e7dd6b4b67216088ed841279eea072fefd1c622b4613d8a0f94fcfa0d664e98a36071dc8eb242c2c729fd7244e2fa0f80737536b992e8fb791c307fbcb9b84e95b6dd2f668717c7e6bb3dd4c9a733380b873f871808080808080808080a0d64fd76b3aeaee6e1136e1dabfd4fef2a486c1444080b8a6bf37ae824c2eaf0c80a0594ab083df935e5d6d23a7accd72932904f68ef5916412ebbfff1f893da06f0fa055c0f29d286e781b859f5a43a7d3b1f87e0c98ec50241ae64077c68f16b9625780808080b86af8689f3d311b46a2440f3eaf346b6c1ce588ed08712591822a258c5a1c4cf44cd0c9b846f8448080a038e6035bdf2333249dbcc37b3ea421f6321a0d679e1eeb23c4266981508a4035a0c89b4b2237442578842fd73dfe24a34f3d19a8db05719dd7938f04574b4e89e6"
     ```

     5. storageTrieWitness: From JSON: "proof → storageTrieWitness" →

     ```json
     "0xf90332b901f4f901f1a03af19a9c87bc51e84006d54f940acace6d08015e18d6e83bd91a363360906498a07c53266a9e94b3183b9477db99ac862f898912840100495e094b5fc7e51c36aca0d8ef3eff0487154c672bb02ae75e6cb11981533287ef9f2b0a9d5d5f911e239aa0f75f51b99b8b4e2ab8e03d241430c8c168a5ddee3e03588cfdd58fea8cf508fca004b4ba4d7cc79c333bf01d7cdbcc10026f5f68948711d9ae87880eef646bdcd080a02760fefa8a9e66e4d9a3595b9ac206da5f3a494bc47636067aabdd2d5e274bcaa0d81ce2fc4bc57ef7a8ca8f8d9ed7efc2c7b9af98930e40d10c619d3bfca28938a03538383f77acf570ca2c0bb00f49473be5b250b42750b00b57dbeaa31271c0b1a02eb6845177907f3b7945f6fa85081bb8d2f63fffbb4a3a8a0b2532c990903526a0b733ea42996197f7202e4776c2dc156895fb81cd4fab6728b7738e01660d6019a04a4fec6733d52a52b4a9c114ddfc54d8ec4c9c3e624682c685fc6047805bd57aa098a08e19fb95d073b942566101ec9c55ca35a906eec6201c2017df5de7ea3616a0a2986d63dde4298513fe694041c460fe7145679f3256e79fa194cc4abc66c1faa0074e9a0fc14828cd882001bb8cc046f451f86cd7b7d52a9e648b70334c69e987a0ce3d1916d06af8ee8f02b41b973e61d3c6d73103c04f842b25eb47edbc5c2d4780b90114f90111a0c3e47868c58cb3af5d1691b2c64ca432e4ecd292933f9a40c2678aef46f03c088080a00802c80d4e65929d801f1051750238ee1702b39da8c5f184a733bedda46a18788080a0d1a87df4c54519d8b1349f02b17b9beefaa24c2cac2dfcb917f7d4427511bfeaa0e623c31257c8f0bd298517eb113447185d48c06233e2e2fb142984a87ce0bf2180a0fc0309f7556876f8a66349d90f72cdde1be34ef3e689599081d790db8ec3c98e808080a022f9e0185641c8beccb0599dccb40b2d91cb1c2e566083ac02b194da2e5f630ea04b609c28019782a80f43e572a554a486d4d610bb8224be4452d27edaaa6c156ca05d2b38e90f59db76cb7b517dbd91eaef6be371f86e8c467cd64ac7f60814e98d80a3e2a020ce69ec16e3e139d304710d5fb5e907d04eb1fef6b7ad931c2b4413a959fbd601"
     ```

7. Execute the transaction clicking `Write`
   ![image](/assets/finalize/image07.png)
   ![image](/assets/finalize/image08.png)
