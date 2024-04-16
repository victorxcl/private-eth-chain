# private-eth-chain
demonstrate how to run ethereum private chain

https://www.cnblogs.com/wkfvawl/p/11211600.html

password: 123456

../go-ethereum.git/build/bin/geth --datadir node1 account new
../go-ethereum.git/build/bin/geth --datadir node2 account new
../go-ethereum.git/build/bin/geth --datadir node3 account new

../go-ethereum.git/build/bin/geth --datadir node1 account list
../go-ethereum.git/build/bin/geth --datadir node2 account list
../go-ethereum.git/build/bin/geth --datadir node3 account list

../go-ethereum.git/build/bin/geth --datadir node1 init CustomGenesis.json
../go-ethereum.git/build/bin/geth --datadir node2 init CustomGenesis.json
../go-ethereum.git/build/bin/geth --datadir node3 init CustomGenesis.json


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



    --nodiscover \

eth.getBalance("cc44e4c1a4ad2af0632cc4a016d9e1953ec5cc0a")
eth.getBalance("2b428f6430af683ea4adfe30016407747db905a4")
eth.getBalance("a2c37f91fd94fb55a1d8a7f0d1fb75e1b28355b0")

admin.nodeInfo.enode


amount = web3.toWei(5,'ether')
web3.fromWei(eth.getBalance(eth.accounts[0]),'ether')

personal.unlockAccount(eth.accounts[0])

eth.sendTransaction({from:eth.accounts[0],to:eth.accounts[1],value:web3.toWei(1,"ether")})
eth.sendTransaction({from:eth.accounts[0],to:"0xc9194a8ea28d76389af4c7e9c81222386a6ab47a",value:amount})

txpool.status
miner.start(1);admin.sleepBlocks(1);miner.stop();
eth.getBlock(180)
