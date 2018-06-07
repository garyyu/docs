# Grin Transaction
> Posted on **2018-06-06 11:34:20**    Original author: **Gary Yu**

A transaction is a transfer of MimbleWimle/Grin value that is broadcast to the network and collected into blocks. A transaction typically references previous transaction outputs as new transaction inputs and dedicates all input MimbleWimle/Grin values to new outputs.

## Transaction
General format of a MimbleWimle/Grin **Transaction** (inside a block):

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| offset   | Blinding Factor. The kernel "offset" k2. <br>"excess" of TxKernel is k1G, <br>after splitting the key k = k1 + k2   | 32 bytes  |
| input_size   | 0 or positive integer  | 8 bytes  |
| output_size   | positive integer  | 8 bytes  |
| TxKernel_size   | positive integer  | 8 bytes  |
| list of [Inputs](#input) | inputs | min: `154*input_size` bytes,<br> max: `1594*input_size` bytes |
| list of [Outputs](#output) | outputs | `716*output_size` bytes |
| list of [TxKernel](#txkernel) | transaction kernels | `114*TxKernel_size` bytes |

### Input
General format of an **Input** in a MilbleWimble/Grin transaction:

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| output_type   | 0 - means transaction output, <br>1 - means coinbase output  | 1 byte  |
| commit   | pederson committment  | 33 bytes  |
| block_hash   | if input comes from coinbase output, <br>hash of the block which contains that coinbase output   | 0 or 32 bytes  |
| [merkle_proof](#merkle-proof)   | if input comes from coinbase output, <br>merkle proof of this input  | 0 or 88~xxx bytes  |

#### Merkle Proof
General format of an **Merkle Proof** in a Input:

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| root   | merkle root hash  | 32 bytes  |
| node   | merkle node hash  | 32 bytes  |
| mmr_size   | mmr total size (leaves + nodes + peaks)  | 8 bytes  |
| peaks_len   | mmr peaks size  | 8 bytes  |
| path_len   | merkle path size  | 8 bytes  |
| peaks   | list of mmr peak hashs  | 0 or `32*peaks_len`  |
| [path](#merkle-path)  |  list of merkle path | 0 or `40*path_len`  |

##### Merkle Path
General format of a **Merkle Path** in an Merkle Proof:

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| hash   | hash of a merkle node/leaf/peak  | 32 bytes  |
| position   | mmr position of this merkle node/leaf/peak  | 8 bytes  |

### Output
General format of an **Output** in a MilbleWimble/Grin transaction

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| output_type   | 0 - Transaction output, <br>1 - Coinbase output  | 1 byte  |
| commit   | pederson committment  | 33 bytes  |
| range_proof_len  |   |  8 bytes |
| proof   | range proof   | 674 bytes for bullet proof; <br>5134 bytes for range proof  |

### TxKernel
General format of a **TxKernel** in a MilbleWimble/Grin transaction:

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| output_type   | 0 - Transaction output, <br>1 - Coinbase output  | 1 byte  |
| fee   | transaction fee  | 8 bytes  |
| lock_height   | trsaction lock block height   | 8 bytes  |
| excess  | pederson committment  | 33 bytes  |
| excess_sig   | committment signature  | 64 bytes  |
|   |   | Total: 114 bytes |

## Example
Here is a **Principle example** of a MimbleWimle/Grin transaction with 1 input, 2 output and 1 TxKernel only. This transaction is included in Testnet2 block [97987](https://grinscan.net/block/97987).

<pre><p>
"offset": "540b8d5d85a80712d115548eae665e3222ce953c552bead6c0851ff3024cde1f",
"input_size": "0000000000000001",
"output_size": "0000000000000002",
"TxKernel_size": "0000000000000001",
"inputs": [
  "<b><a href="http://127.0.0.1:13413/v1/chain/outputs/byheight?start_height=97863&end_height=97863&id=09f27ad11cd6cb6611949cd4a3795ef8871fe9a54df11f163a631b04a00af3d3d5">09f27ad11cd6cb6611949cd4a3795ef8871fe9a54df11f163a631b04a00af3d3d5</a></b>"
],
"outputs": [
    {
      "output_type": "Transaction",
      "commit": "<b><a href="http://127.0.0.1:13413/v1/chain/outputs/byheight?start_height=97987&end_height=97987&id=0802d40a4d869dda6443f9679403ae8d42b0ed2579f7e7734974ee5f81000156ec">0802d40a4d869dda6443f9679403ae8d42b0ed2579f7e7734974ee5f81000156ec</a></b>",
      "range_proof_len": "00000000000002a2",
      "range_proof": "54c9e7cf305b48c514870c23ee21593be5958d1ce611c0569462d971aeb7ca8b154d28bbd68bb73a32491ecf4d48354166e90af684cae7caa3e560b846d87770
      51e606477e9ace8369c301e7eff3939ea5516f4477cd8f50f5202c2b29a97eb4455713181a02216f7518c7eb9a42dd08d4ecf673645992f6f63dd6f126495ee7
      ae339acad7123bee693a37cea5f8a32cd365624f59beb71ef9cdbd1649d625071bab4164c8f9ed53ea8e6621023e4d606b9d4a007ea8c1ed1418b1a52ca991cd
      4f29a1b842281bafb302ebfc1b0e44189325fcbc79dfe7a52ac488c843bd82d109a9e39db93c129ff4bbbb9de61f2292e89bfca9e87085e46689a9f37136d632
      e525dad2a34cea459628d788785b8793a07e133793340bf3109787178827982ee5c9ee3d13f257cafe8b47fdc53609db3aa9754304e959a5a7dead4b7aad9819
      e26a5b6f089f4d7d9ea78432baf0257a4595b4b53d07ae944b8e85864df5f226fc255cc8064433b727377229476b8f433cdbdaac8ca15aad4a1b36da7fd01900
      527c79e56e943c37ba475e290c7f6d220b0aead456c81951a7e3a51cd5b8b1923035adc7c30db0a8a8d42fe76e71a24af49e58e3972ee7b494d54dec9fdbab41
      063a6e89e63e10e8f215efb73b3407081680216976f6c29b03211a30fe9b2eefb97c7ca6193a388e3c76f3c3fcdebdbb90c8381db51a6c63bcc5ea6c8c298e70
      3ce0ba252f5ba2936c38f37a92943ae0ad5dc6196f31d666fc2fc747f3a7a6f869b8f31300e1080c75ac7664c1d3d533fd54ee1a6d11b626863aa860a6f5ba89
      1d4b7dbb3220fbf6c3282f1c440b9c0bdf9e2daa3c3beae03fdc452b6905b874e7cbf35f5ce0fd6b7364c843c37b329920fbdecd02b9d17df398a5356c18e861
      f51afb613b9460a0e18e9e99e6442aaea33ffcd0a22744a056fed6b13783d82db0d2"
    },
    {
      "output_type": "Transaction",
      "commit": "<b><a href="http://127.0.0.1:13413/v1/chain/outputs/byheight?start_height=97987&end_height=97987&id=09dc7856df58b3b1acd6d27f1f3246abe504421de2e249d2c27274eea6968cad93">09dc7856df58b3b1acd6d27f1f3246abe504421de2e249d2c27274eea6968cad93</a></b>",
      "range_proof_len": "00000000000002a2",
      "range_proof": "ca5c70c6b94eed69255910cde8048937cb6da8a782d51c18b23142774f4a4f3000c252f43421b51c520ec61befe09c37b0c10711755b46b362bd9dc4177c831e
      9e9da33aa1b344aa5aa47116ee0e5949b9b47be3dc5d797502f0e01f3f0cd44e8974472c3fd27d56b3ac1f4e16bd8beb96e885e52089a4d4370d7af4001ccd2b
      46813cf47e2a67e9c9c820e6483dfd125569847f8badbab046f6c9086dd86cfb0c527e27b59840a6a5eb19613b764a2dfa199a6e59985df9b8d26a318329d38f
      d7edac848154851d565af1eabc0bfd91813be3748ef480d1e0df84e054ea927d19d1942c00da406fc1f71d6150d411db7c8a3179f9d23214a6a373470589741c
      a6c5bcf240497ae02ea0df21741f1dc690fc4e5e753dbe996a041971b5258b92743b9483b12bd3446f331753a209a45e0da8c3bba993bc729ab5d8d2c0991903
      0303be2c432bfeef2fcdcc39d32c57a4d856667810c24632fd269db841f9cf81da07ace94b7f8e77b7958594e24ea4fbbeaf2844bc4a9f6c5b99e2681c338b90
      1141ccc9251534389b84989e5f0389450e0bbcb444a7485a66d2de50d1e2df433c27b0461a9e303cdd2b75fe6287025693b216692d9ea6298d27b75207baa3ed
      4f5f09d359839fbeaf2f2dc944e08da4e3fb15281bd37a5ca4feeafd9feedaa9804a7bd32c588cbc8ee410879b55a85a5ac4e228b9db122d9e3f64cb671ba067
      f8988f23a33cc3dfd3d59525b849df98f83dee3bc6d003d69be93f1d2abf10aa02e32845b91f146c17e362b43036f53ec6b440931aae722aa88ba68bded7d206
      b17b0c16c12d0d564b92f299f16e72982d179c6d885dae7ca0be4ec6f7039097b5fb4ed5d7b2518057719c59837fe2d8fa946b344b8c5d61b534f5cb8acdbe2e
      76223839e0002e16558d2693a686b27aebfb179c5bd76d93d2a08e214f611c8b8bf4",
    },
],
"kernels": [
  {
    "type": "Transaction",
    "fee": 8000000,
    "lock_height": 97984,
    "excess": "097f282d29b6edcc8b83360e961cc734e65d70c223b7cb062d0a755502cfb720ab",
    "excess_sig": "09704f94c0f23b9e95d11b971d047cfced94e9c318a05c2b9d7a64348c6359bc28a1967d702a6b182c7068cc306f37837da03ce4321271acd3d8d8c07b6a2989"
  }
]
</p></pre>

### Explanation

In this transaction example, Alice sent 40 Grins to Bob, Alice also paid the transaction fee 0.008 Grin, so eventually Bob got 40 Grins and Alice spent 40.008 Grins.

Grin transaction fee unit is `10^-9` Grin coin, for example in above example, transaction fee is `8*10^6` fee unit, that's `8*10^-3` Grin.

The total size of the above transaction example is 1636 bytes. Compared to [Bitcoin transaction size](https://charts.bitcoin.com/chart/transaction-size), which is average about 500 bytes; and [a typical bitcoin transaction example here](https://blockchain.info/tx/7e46a5ea9d9c19cd4d0c3d0a287419d7c1ae13049ac7ab8860b6ee0cec4ead17) with 1 input and 2 outputs has the size of 225 bytes long. So, Grin transaction size is much bigger than Bitcoin transaction, it's about 7 times in this example.

### Transaction Fee

The fee is the `transaction weight * Milli-Grin`.
- `Milli-Grin = 10^-3 Grin`
- `transaction weight = MAX(-1 * input_size + 4 * output_size + 1, 1)`
In above example, the transaction contains 1 input and 2 outputs, so, the `transaction weight = 8`, and `fee = 8 * 10^-3 Grin`.
