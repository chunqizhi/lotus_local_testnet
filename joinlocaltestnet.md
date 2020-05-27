## 切换到 v0.3.0 版本

git reset --hard 1ac6f3fa33f44929f94e986b053070dddd0a4968

## 构建支持 2048 个字节的扇区大小的程序

make 2k

## 下载2048字节证明参数

env IPFS_GATEWAY="https://proof-parameters.s3.cn-south-1.jdcloud-oss.com/ipfs/" ./lotus fetch-params --proving-params 2048

## 下载创世区块文件到本地

genesis.gen

## 使用创世区块文件启动本地 lotus

./lotus daemon --genesis=genesis.gen --bootstrap=false

## 把当前节点连接到创世节点进行同步

./lotus net connect /ip4/ip/tcp/33993/p2p/12D3KooWD5ojR9wuTX6DEvv9G1Z4fQg9g2WD7bAMje63vPRZcgg1

## 创建 bls 地址（注意：这里的地址没有余额，需要创世节点转点 fil，因为矿工需要质押一些 fil）

./lotus wallet new bls
t3xfh3hbpc5t7eqcg6o4yc7qtiu3r5iafgh3yfbvfza6yfklkjrrlxnfiipm62j72pudskkb4ulcanexmnbw6a

## 查看余额

./lotus wallet balance
10000

## 设置新矿工

./lotus-storage-miner init --owner=t3xfh3hbpc5t7eqcg6o4yc7qtiu3r5iafgh3yfbvfza6yfklkjrrlxnfiipm62j72pudskkb4ulcanexmnbw6a --sector-size=2048 --nosync

## 启动新矿工

./lotus-storage-miner run --nosync

## 查看新开工信息

./lotus-storage-miner info

## 密封随机数据以开始生成PoSts

./lotus-storage-miner sectors pledge

## 查看新开工信息

./lotus-storage-miner info