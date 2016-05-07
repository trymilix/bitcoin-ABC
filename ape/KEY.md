
比特币支付的记录中有两个脚本scriptPubKey和scriptSig，scriptPubKey用于下一次支付，scriptSig配合上一次支付的scriptPubKey验证比特币的合法性。这个脚本语言基于堆栈解释执行，有点不大好读，转换成C语言可读性就好多了。


```C
// 脚本语言自定义库
#define OP_EQUAL(v1, v2) (v1 == v2) 
#define OP_VERIFY(pass) if (!pass) return false 
  
#define OP_EQUALVERIFY(pubHashA, pubKeyHash) \
  OP_VERIFY(OP_EQUAL(pubHashA, pubKeyHash))
  
OP_HASH160(pubKey)
{
}
```

```C
// scriptPubKey和scriptSig合并后的功能
bool Pay_to_PubkeyHash(sig, pubKey, pubKeyHash)
{
  pubHashA = OP_HASH160(pubKey);
  OP_EQUALVERIFY(pubHashA, pubKeyHash);
  return OP_CHECKSIG(sig, pubKey);
}
```

Pay_to_PubkeyHash有三个参数，sig和pubkey是scriptSig脚本提供的，pubKeyHash是scriptPubKey提供的。一笔支付记录中所有的信息包括pubKeyHash是公开的，但想要花这个钱就必须提供sig和pubkey。

两个脚本其实完成一种约定，而上述只是其中一种。

# Reference
1. [Transaction](https://en.bitcoin.it/wiki/Transaction)
2. [Bitcoin Developer Reference](https://bitcoin.org/en/developer-reference)
