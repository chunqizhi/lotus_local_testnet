## 切换到 v0.3.0 发布版本

git reset --hard 1ac6f3fa33f44929f94e986b053070dddd0a4968

## 在调试模式下构建 Lotus Binaries，这可以使用2048个字节的扇区

make 2k

## 下载2048字节证明参数
env IPFS_GATEWAY="https://proof-parameters.s3.cn-south-1.jdcloud-oss.com/ipfs/" ./lotus fetch-params --proving-params 2048

## 预密封一些扇区：
./lotus-seed pre-seal --sector-size 2048 --num-sectors 2

## 创建创世块并启动第一个节点
### 创建创世块文件
./lotus-seed genesis new localtestnet.json

### 将创世矿工加入创世文件中
./lotus-seed genesis add-miner localtestnet.json $HOME/.genesis-sectors/pre-seal-t01000.json

## 启动 lotus 并将创世区块信息写入 genesis.gen 文件中
./lotus daemon --lotus-make-genesis=genesis.gen --genesis-template=localtestnet.json --bootstrap=false

## 导入创世矿工密钥

./lotus wallet import $HOME/.genesis-sectors/pre-seal-t01000.key

## 设置创世矿工

./lotus-storage-miner init --genesis-miner --actor=t01000 --sector-size=2048 --pre-sealed-sectors=$HOME/.genesis-sectors --pre-sealed-metadata=$HOME/.genesis-sectors/pre-seal-t01000.json --nosync

## 启动创世矿工

./lotus-storage-miner run --nosync

## 其他

### 查看lotus监听地址

./lotus net listen
/ip4/127.0.0.1/tcp/33993/p2p/12D3KooWD5ojR9wuTX6DEvv9G1Z4fQg9g2WD7bAMje63vPRZcgg1
/ip4/ip/tcp/33993/p2p/12D3KooWD5ojR9wuTX6DEvv9G1Z4fQg9g2WD7bAMje63vPRZcgg1
/ip4/ip/tcp/33993/p2p/12D3KooWD5ojR9wuTX6DEvv9G1Z4fQg9g2WD7bAMje63vPRZcgg1
/ip6/::1/tcp/45977/p2p/12D3KooWD5ojR9wuTX6DEvv9G1Z4fQg9g2WD7bAMje63vPRZcgg1

### 查看本地地址列表

./lotus wallet list
t3wkvfzrshohtuai46uauczv53dcddbkljlz357btrjiloxavvclzoxps2ufk67mnebgr5hwwigysfqdwwl4fa

### 设置默认的地址

./lotus wallet set-default t3wkvfzrshohtuai46uauczv53dcddbkljlz357btrjiloxavvclzoxps2ufk67mnebgr5hwwigysfqdwwl4fa

### 查询默认地址余额

./lotus wallet balance

## 需要创世节点转点钱给加入节点的地址

./lotus send toaddr 10000