# private-eth-chain
demonstrate how to run ethereum private chain

https://www.cnblogs.com/wkfvawl/p/11211600.html

为了简单，所有的account的password: 123456

```
../go-ethereum.git/build/bin/geth --datadir node1 account new
../go-ethereum.git/build/bin/geth --datadir node2 account new
../go-ethereum.git/build/bin/geth --datadir node3 account new
```

## 每个node文件夹里面创建一个account。这个account默认作为挖矿的coinbase。

```
../go-ethereum.git/build/bin/geth --datadir node1 account list
../go-ethereum.git/build/bin/geth --datadir node2 account list
../go-ethereum.git/build/bin/geth --datadir node3 account list
```

```
../go-ethereum.git/build/bin/geth --datadir node1 init CustomGenesis.json
../go-ethereum.git/build/bin/geth --datadir node2 init CustomGenesis.json
../go-ethereum.git/build/bin/geth --datadir node3 init CustomGenesis.json
```

## 启动第一个节点，也是我们的挖矿节点：

```
../go-ethereum.git/build/bin/geth \
    --identity "node1" \
    --datadir node1 \
    --networkid 6161 \
    --http \
    --http.port "3031" \
    --http.corsdomain "*" \
    --port "30313" \
    --maxpeers 5 \
    --http.api "admin,eth,debug,miner,net,txpool,personal,web3,ethash " \
    --allow-insecure-unlock \
    console 2>node1/geth.log
```

--allow-insecure-unlock参数表示可以通过HTTP协议解锁account（我们的测试私链，不想整复杂的HTTPS）。

注意上面的输出重定向到了各自节点的geth.log文件里面。可以通过：

```
tail -f geth.log
```
来动态查看最近的输出。之所以这么做，是为了将console和log输出分开，避免混杂在一起。
后面的两个节点，也要这么做。

启动之后，键入下面的命令：
```
admin.nodeInfo.enode
```
获得该节点的enode值，后面需要用到。


## 启动第二个节点（需要用到第一个节点的enode值）：
```
../go-ethereum.git/build/bin/geth \
    --identity "node2" \
    --datadir node2 \
    --networkid 6161 \
    --http \
    --http.port "3032" \
    --http.corsdomain "*" \
    --port "30323" \
    --maxpeers 5 \
    --http.api "admin,eth,debug,miner,net,txpool,personal,web3,ethash " \
    --allow-insecure-unlock \
    --bootnodes "enode://db13635cf83cd57047446cdf2f884a7d0c622593c0d40004db52878a365d1314e3160585d5128f32ce7b5a27ec5cd0cf90db37fc821593af6a9f5c5e3024d65d@127.0.0.1:30313" \
    console 2>node2/geth.log
```
记得替换--bootnodes的参数为第一个节点的enode值。

## 启动第三个节点（需要用到第一个节点的enode值）：
```
../go-ethereum.git/build/bin/geth \
    --identity "node3" \
    --datadir node3 \
    --networkid 6161 \
    --http \
    --http.port "3033" \
    --http.corsdomain "*" \
    --port "30333" \
    --maxpeers 5 \
    --http.api "admin,eth,debug,miner,net,txpool,personal,web3,ethash " \
    --allow-insecure-unlock \
    --bootnodes "enode://db13635cf83cd57047446cdf2f884a7d0c622593c0d40004db52878a365d1314e3160585d5128f32ce7b5a27ec5cd0cf90db37fc821593af6a9f5c5e3024d65d@127.0.0.1:30313" \
    console 2>node3/geth.log
```

记得替换--bootnodes的参数为第一个节点的enode值。


    --nodiscover \

## 对三个节点的私有链进行简单的测试

某个节点中的所有account列表：
```
eth.accounts
```

获取三个账户的余额（单位为wei）
```
eth.getBalance("cc44e4c1a4ad2af0632cc4a016d9e1953ec5cc0a")
eth.getBalance("2b428f6430af683ea4adfe30016407747db905a4")
eth.getBalance("a2c37f91fd94fb55a1d8a7f0d1fb75e1b28355b0")
```

单位从wei换算为ether（以太币）：
```
web3.fromWei(eth.getBalance(eth.accounts[0]),'ether')
```

尝试转账：
```
amount = web3.toWei(5,'ether')
web3.fromWei(eth.getBalance(eth.accounts[0]),'ether')

personal.unlockAccount(eth.accounts[0])

eth.sendTransaction({from:eth.accounts[0],to:eth.accounts[1],value:web3.toWei(1,"ether")})
eth.sendTransaction({from:eth.accounts[0],to:"0xc9194a8ea28d76389af4c7e9c81222386a6ab47a",value:amount})

txpool.status
miner.start(1);admin.sleepBlocks(1);miner.stop();
eth.getBlock(180)
```
