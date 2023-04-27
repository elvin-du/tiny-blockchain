# tiny-blockchain
一个学习blockchain的玩具，同时学习一下rust。

# 功能
1. 共识
2. mpool
3. wasm虚拟机，利用wasmtime
4. 存储，利用sqlite
5. p2p,利用rust-libp2p
6. rpc，利用jsonrpc
7. cli，利用github.com/urfave/cli

## 共识
1. index = sha256（blockheight，block-height-round，parent-blockhash）/ size（validators in parent height）
2. producer = validators[index]
3. 每个height的块，初始等待5秒钟，5秒内收到区块，则本地高度+1，然后定时器重置，重新计算并倒计时5秒钟。
4. 如果5秒内没有收到区块，则height不变，round+1，同时接受block的窗口时间从5秒，变成5sec的两倍。因为round变化了。所以出块人也会变化。

## 主要数据结构
### block结构
{
  height: int
  round: int
  producer: address
  parent: hash
  state-root: hash
  txs:tx[]
}

### tx 结构
{
  from:address
  to:address
  amount: int
  payload: bytes[]
  sig: bytes
}
### mpool
{
  mpool: map[tx-hash]tx
}

### state
blockheight-address/account

//用lastest-address为key，value是blockheigh-address

struct account{
  addr:address
  amount: int
  code: bytes
  height：int
  storage-root:hash // 按照字母对contractAddress-height-variableName排序，并用数组形式一下子hash
}

//智能合约永久存储的K/V
contractAddress-height-variableName/variableValue（bytes）
  


