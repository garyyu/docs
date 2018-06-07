# Block

Transaction data is permanently recorded in files called blocks. They can be thought of as a stock transaction ledger. Blocks are organized into a linear sequence over time (also known as the block chain). New transactions are constantly being processed by miners into new blocks which are added to the end of the chain. As blocks are buried deeper and deeper into the blockchain they become harder and harder to change or remove, this gives rise of blockchain's **Irreversible Transactions**.

## Block structure

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| header   | [Block Header](#header) | 362 bytes  |
| input_size   | 0 or positive integer  | 8 bytes  |
| output_size   | positive integer  | 8 bytes  |
| TxKernel_size   | positive integer  | 8 bytes  |
| list of [Inputs](https://github.com/mimblewimble/docs/wiki/transaction#input) | inputs | min: `154*input_size` bytes,<br> max: `1594*input_size` bytes |
| list of [Outputs](https://github.com/mimblewimble/docs/wiki/transaction#output) | outputs | `716*output_size` bytes |
| list of [TxKernel](https://github.com/mimblewimble/docs/wiki/transaction#txkernel) | transaction kernels | `114*TxKernel_size` bytes |

As you see, block structure is very simililiar to the [transaction](https://github.com/mimblewimble/docs/wiki/transaction#transaction) structure, only difference is the first field: block's 1st field is 362bytes length `block header`, but transaction's 1st field is a 32bytes `offset` (blinding factor).

### Header
General format of an **Block Header** in a MilbleWimble/Grin block:

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| version   | Version of the block | 2 bytes  |
| height   | Height of this block since the genesis block (height 0)  | 8 bytes  |
| previous   | Hash of the block previous to this in the chain | 32 bytes  |
| timestamp   | Timestamp at which the block was built | 8 bytes  |
| total_difficulty | Total accumulated difficulty since genesis block |  8 bytes |
| output_root  | Merkle root of all the commitments in the TxHashSet | 32 bytes  |
| range_proof_root | Merkle root of all range proofs in the TxHashSet | 32 bytes  |
| kernel_root | Merkle root of all transaction kernels in the TxHashSet | 32 bytes  |
| total_kernel_offset | blinding factor.<br>Total accumulated sum of kernel offsets since genesis block.<br>We can derive the kernel offset sum for *this* block from<br>the total kernel offset of the previous block header. | 32 bytes  |
| nonce | Nonce increment used to mine this block. | 8 bytes  |
| pow  | Proof of work data | 168 bytes. `42*4` |
|   |   | Total: 362 bytes  |

## Example
Here is a **Principle example** of a MimbleWimle/Grin block. This block is included in Testnet2: [97979](https://grinscan.net/block/97979).

<pre><p>
{
  "header": {
      "hash": "99870689c6498d293379f5342b63bd07b300c37ff96b4f29e668c07b33bf6309",
      "version": 1,
      "height": 97979,
      "previous": "83f297db9acae4016e2e37fb5c9540971452bdf4e5f7e44756df3f02b496928f",
      "timestamp": "2018-06-05T14:07:41Z",
      "output_root": "bfe4e000a3297316173b55212d062915b59cae9caaba77b1e0b20ba740adc11d",
      "range_proof_root": "9e1f149249f3ec65685f6f1390998165c37cf7f21b64a20138794d257bc1e12f",
      "kernel_root": "1333829a289df3709a77583170687c0a873d72a45377343a94311f069c139768",
      "nonce": 4171404068441056000,
      "total_difficulty": 133937,
      "total_kernel_offset": "eea34d535032ceccd9961ea1476fe4587180671645a12e659f8f5e9be10f151e"
  },
  "input_size": "0000000000000000",
  "output_size": "0000000000000001",
  "TxKernel_size": "0000000000000001",
  "inputs": [],
  "outputs": [
    {
      "output_type": "Coinbase",
      "commit": "09bb9acf592a787efc4c14105b646ea0543490aa1fde421c2aeaa135029a32c643",
      "commit": "<b><a href="http://127.0.0.1:13413/v1/chain/outputs/byheight?start_height=97987&end_height=97987&id=09bb9acf592a787efc4c14105b646ea0543490aa1fde421c2aeaa135029a32c643">09bb9acf592a787efc4c14105b646ea0543490aa1fde421c2aeaa135029a32c643</a></b>",
      "range_proof_len": "00000000000002a2",
      "range_proof": "318599a8f2de8c37f560d50656b6c1252c08fa221174f8b7ff7ec6c8c490d76a1e840169c8c1e3691e7ebbddaa0287592aa3cc6b5d3e512a5d5ebe242ac3fc1d
      702020d04db0b1f75be1b8a76af2c3c6d6966f7a630d9e1cf3b3522c697c1e09c8338492e5350b0293de8f7b2548070aa3ee9df57e2428b8e94cd879b4e7c3e8
      bc2cc2c22e03fd926fb127db7a89a63a08d7718cb738b8e36db223f50a0f0805a8cd190cc6aa7d116fdaa6c53869fcd447d41a301160804f4251dd0871fbfd18
      04134a4e2fd3035faaefd3aec3c6cc449b8c2620a01b68199548e9a219700445a96a258f8ae339c38c73af8dbc76bfb889ab5109cbd7fb756524806e98ecc0d7
      2dc3b92fadf26fcddc0400be88f31fe6b4863258d5f46f9f63ed704ba0a2faaa24d4091bac976bff251d4e8bdbff47d1873dfaeb4689023f3e1a36d95524fefa
      4cbfda403062b4620104d09c47df71395013c43ee8fb06bb994bb4873f0b81b84ce0ea9ac7946ff0427d6b17c455ebdbda0a6576c7801e36e28b2c2a402bd6cb
      1faf6595828cec632c7a2e442e1b981f5068d8fa4db6110dfef2364996ce54014aeff53034a2c64f8e5276097fa3882c20d1d3be7a4f0c54f7c6219905564a9b
      d66e2c1fadae6a09a4e17780549676954c77f7cb75df2f2939155e761c213342865d80d0080fb6a2afbbb520d7161e9072f6887b4ff7fa9f8bf6468a623aa76d
      f56428bfb03dec11a0a3712e36cd7f995174fe85091c65c165a3e440b4dbc8e87bee002f6dad5181ad112d4585c08afadc6615b755dd52c72471af1801e35f97
      8f2a534e2368b7afebb7b80e852f982ef0d5d638c7f480d1b2ea88d1b5cc2bc3c0eee5d7a6968ce525c0f83eb251746943d01d9362cf638340ddf6ad6fd9b4b5
      620026527af12971debeba7c2b94d91b7b98f348428937d8c499b407daff4ad6a23a",
    }
  ],
  "kernels": [
    {
      "features": "DEFAULT_KERNEL | COINBASE_KERNEL",
      "fee": 0,
      "lock_height": 97979,
      "excess": "08eb50d009ab1a2f38b50f4a85b87a950271c8fc108a5fff377aae171fe70188d4",
      "excess_sig": "7aacc634a9c2661a6b5808a1fa883b3a1d0bd0ddab9937f5672d3a4f86b5cd003d62f8d005246505f46a9c062f8fb6aa1d2f02e9cea61986ff7c97d3f3e0d570"
    }
  ]
}
</p></pre>

## Description

Each block contains a record of some or all recent transactions, and a reference to the block that came immediately before it. It also contains an answer to a difficult-to-solve mathematical puzzle - the answer to which is unique to each block. New blocks cannot be submitted to the network without the correct answer - the process of "mining" is essentially the process of competing to be the next to find the answer that "solves" the current block. The mathematical problem in each block is extremely difficult to solve, but once a valid solution is found, it is very easy for the rest of the network to confirm that the solution is correct. There are multiple valid solutions for any given block - only one of the solutions needs to be found for the block to be solved.

There is also a reward of brand new grins for solving each block, the reward is recorded into block as a transaction, with 0 input 1 output and 1 transaction kernel. This record is known as a coinbase transaction. The number of Grins generated per block is 60, at fixed rate, i.e. no reward changes.

Grin transactions are broadcast to the network by the sender, and all peers trying to solve blocks collect the transaction records and add them to the block they are working to solve. Miners get incentive to include transactions in their blocks because of attached transaction fees. [Here](https://github.com/mimblewimble/docs/wiki/transaction#transaction-fee) has the detail for the transaction fee calculation.

The difficulty of the mathematical problem is automatically adjusted by the network, such that it targets a goal of solving an average of 1 block per minute. Every 60 blocks (solved in about one hour), all Grin clients compare the actual number created with this goal and modify the target by the percentage that it varied. The network comes to a consensus and automatically increases (or decreases) the difficulty of generating blocks.

Here is the detailed difficulty adjustment algorithm code: [https://github.com/mimblewimble/grin/blob/master/core/src/consensus.rs#L174](https://github.com/mimblewimble/grin/blob/master/core/src/consensus.rs#L174).
