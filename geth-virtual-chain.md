# 使用geth建立虛擬鏈



**tags: 乙太坊 區塊鍊 geth**

## 乙太坊筆記 -- 建立虛擬鏈

* 初始化私有ETH網路，可以用puppeth指令產生創世區塊的json內容

  > geth --datadir "./privatechain" init custom\_genesis.json
  >
  > * 建立兩個節點，這樣之後一個節點可以提供rpc服務，一個可以用來挖礦
  > * 如果是要在同一台機器上建立不同的節點，所有的節點都必須在獨立的目錄底下運作

```text
{
  "config": {
    "chainId": 657,
    "homesteadBlock": 1,
    "eip150Block": 2,
    "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "eip155Block": 3,
    "eip158Block": 3,
    "byzantiumBlock": 4,
    "ethash": {}
  },
  "nonce": "0x0",
  "timestamp": "0x5ad4459f",
  "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "gasLimit": "0x47b760",
  "difficulty": "0x80000",
  "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "coinbase": "0x0000000000000000000000000000000000000000",
  "alloc": {
    "xxxx": {
      "balance": "0x200000000000000000000000000000000000000000000000000000000000000"
    }
  },
  "number": "0x0",
  "gasUsed": "0x0",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

* custom\_genesis.json 裡面可以放一些錢給測試用的帳號:

  ```text
    "alloc": {
       "0xxxxxx":
           {"balance": "10000"}
    }
  ```

* 在console啟動節點
  * 常用參數
    * --networkid 為啟動在哪個網路id，避免衝突
    * --datadir 為指定區塊鏈資料位置
    * --ipcdisable 不加這個參數的話在本地啟動多個服務會出現access deny\(CentOS沒這個問題，windows才會\)
    * --config 後面可以加上自定義的TOML檔案來設定參數
    * 在/geth/底下的static-nodes.json可以定義要和這個node同步的peers
    * 指令後面加上　dumpconfig　可以把設定值匯出成toml檔案
  * node1\(監聽2000 port\) :

    ```text
    geth --datadir ./privatechain --networkid 1510 --port 2000 --ipcdisable console
    ```

  * node2\(監聽2001 port\) :

    ```text
    geth --datadir ./privatechain --networkid 1510 --port 2001 --ipcdisable console
    ```
* 讓兩個節點可以互相溝通
  * 在node1執行`admin.nodeInfo`

    在輸出的結果中找到enode 資訊，會有類似底下的格式 : 

    `enode://xxxxx@[::]:2000`，把enode的xxxx抄下來

  * 回到node2，執行

    `admin.addPeer("enode://xxxxx@127.0.0.1:2000")`

    127.0.0.1:2000 是node1監聽的位置

    照相同的步驟在node1把node2的端點加進去

    執行 `admin.peers` 檢查一下是否兩個node都互相連結了

  * 每次重啟node都需要重新做一次，可以直接toml設定檔來寫入設定，避免每次都要重新加
* 在console模式底下的指令:
  * 查看帳戶

    `eth.accounts`

  * 建立帳號，設定密碼 123456 `personal.newAccount("123456")` 產生的私鑰會放在keystore目錄裡面
  * 解鎖帳號，這樣就不需要輸入密鑰也能交易 `personal.unlockAccount('0xxxxxx','pass')`
* 啟動RPC服務 `geth --datadir ./privatechain --networkid 1510 --rpc` 會開始監聽 8545 port
* 挖礦 `geth --datadir ./privatechain --networkid 1510 --port 2001 --ipcdisable --mine --minerthreads=1 --etherbase=0x0000000000000000000000000000000000000000`
  * etherbase 用來指定挖礦的coin要歸戶給哪個帳戶
  * minerthreads 表示要用幾個thread來挖礦
* 交易 Ethereum EVM的單位是Wei 1 Ether = 1,000,000,000,000,000,000 Wei 交易費用 ＝ 實際用到的Gas \* Gas Price 如果Gas Limit不夠，交易將會無法完成，但是手續費會被扣掉 如果Gas足夠，剩下的Gas會釋出所以不會多收手續費
* 注意事項
  * 不管是要在哪一個node對虛擬鏈下交易指令，都必須在那一台node上啟動挖礦，否則交易都會卡在pending裡面\(有時候在各個node可以查到交易成功，但是用了第三方套件例如metamask則會無法顯示正確交易結果\)
  * 有時候挖礦指令會當掉，必須先`miner.stop()`再`miner.start(1)`重新開啟
* 每個node最後執行的指令
  * node 1 : 

    `geth --datadir ./privatechain --networkid 1510 --port 2000 --config config.toml --rpc --rpcaddr x.x.x.x --rpccorsdomain "*" console`

  * node 2: 

    `geth --datadir ./privatechain --networkid 1510 --port 2001 --config config.toml --mine --minerthreads=1 --etherbase=0xxxxx console`

