@(工作)[谷米]

[TOC]

---
#区块链
**作者：[卢宁](mailto:luning@vip.163.com)**

>##写在前面
>区块链作为比特币的底层基本技术支撑着比特币的流通和实现，随着比特币的流行以及政府和公共对比特币的重视，区块链技术也渐渐地浮出水面。本文使用信息获取方法论结合技术以人为本的原则，以5W1H（What-Where-Why-Who-When-How）提问法为基础，创造出（2WHT）提问法来阐述区块链，旨在做到让读者明白什么是区块链，为什么区块链会出现，区块链是怎样工作的和我们可以用区块链技术做什么。鉴于本人知识水平实在有限，若有任何问题或者不对的地方，欢迎大家通过邮箱积极与我讨论，谢谢大家！
>
##区块链是什么？（What？）

>Double-spending is the result of successfully spending some money more than once. Bitcoin protects against double spending by verifying each transaction added to the block chain to ensure that the inputs for the transaction had not previously already been spent.
Other electronic systems prevent double-spending by having a master authoritative source that follows business rules for authorizing each transaction. Bitcoin uses a decentralized system, where a consensus among nodes following the same protocol is substituted for a central authority.
Bitcoin has some exposure to fraudulent double-spending when a transaction is first made, with less and less risk as a transaction gains confirmations.* [\[1\]](#rel)
##区块链为什么会产生？（Why？）
##区块链是怎么样工作的？ （How？）

### Block Hashing Algorithm

| Field      |     Purpose |   Updated when...   |Size (Bytes)|
| :-------- | :--------| :------ |:------|
|Version|	Block version number|	You upgrade the software and it specifies a new version	|4|
|hashPrevBlock	|256-bit hash of the previous block header|	A new block comes in	|32|
|hashMerkleRoot|	256-bit hash based on all of the transactions in the block	A transaction is accepted|	32|
|Time|	Current timestamp as seconds since 1970-01-01T00:00 UTC|	Every few seconds	|4
|Bits|	Current target in compact format	|The difficulty is adjusted	|4
|Nonce	|32-bit number (starts at 0)|	A hash is tried (increments)	|4

比特币使用哈希碰撞算法作为POW，一个哈希碰撞算法需要一个服务字符串（service string），一个随机数（nonce）和一个计数器（counter）。在比特币中，服务字符串包含了版本字段，前一个块的hash值，交易Merkle Root Hash，当前时间戳和难度值。随机数存放在extraNonce字段中，作为Coinbase 交易的一部分存在于Merkle Tree的最左边的叶子节点。在挖矿时，hash 碰撞算法会不断地hash 区块头，如果不满足target，Counter 加1，Nonce也会加1，从而重新计算 Merkle Root Hash，继而重新计算 Block Header Hash。


##区块链可以干什么？（Then？）


---
##参考文献
><span id="ref_1"> [1] J. Gama, I. Žliobaitė, A. Bifet, M. Pechenizkiy, and A. Bouchachia, “A survey on concept drift adaptation,” ACM Comput. Surv., vol. 46, no. 4, pp. 1–37, Mar. 2014.</span>

