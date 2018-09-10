# 安裝Parity步驟

**tags: 乙太坊 區塊鍊 geth**

## 安裝Parity步驟

### 安裝rust

```text
curl https://sh.rustup.rs -sSf | sh
```

### 安裝parity

```text
git clone https://github.com/paritytech/parity-ethereum
cd parity-ethereum
git checkout stable //只安裝stable版本
cargo build --release --features final

或直接下載 binary :
https://github.com/paritytech/parity-ethereum/releases
```

### 缺少套件

* libudev.h

  `yum install systemd-devel`

* cmake3:
  * 下載cmak3 source \([https://cmake.org/](https://cmake.org/)\)
  * ./bootstrap --prefix=/usr/local
  * make
  * make install

### 執行

```text
./parity  --jsonrpc-interface=w.x.y.z     --jsonrpc-apis=eth,web3,personal,net,traces --jsonrpc-cors=all   --chain=ropsten   --jsonrpc-threads=1   --tracing=on
```

**interface 表示要綁在哪一張網卡上**

### 參數說明

* --jsonrpc-interface 綁定rpc service的IP
* --jsonrpc-apis 5 RPC支援的API
* --jsonrpc-cors 允許CORS的網域
* --chain 使用的ether網路名稱
* --jsonrpc-threads RPC服務耗用CPU的資源
* --tracing=on 開啟trace模式
* --geth geth相容模式，讓RCP的指令可以跟geth相容一點點
* --warp 使用snapshot方式同步最近的資料，之後有空的時候會慢慢同步前面的資料

### config

* parity也可以使用設定檔，設定檔位址在~/.local/share/io.parity.ethereum/config.toml

  \`\`\`

  \[parity\]

  mode = "active"

  mode\_timeout = 300

  mode\_alarm = 3600

  auto\_update = "critical"

  release\_track = "current"

  public\_node = false

  no\_download = false

  no\_consensus = false

  no\_persistent\_txqueue = false

chain = "kovan" base\_path = "$HOME/.local/share/io.parity.ethereum" db\_path = "$HOME/.local/share/io.parity.ethereum/chains" keys\_path = "$HOME/.local/share/io.parity.ethereum/keys" identity = "" light = false

\[rpc\] disable = false port = 8545 interface = "w.x.y.z" cors = \["all"\] apis = \["web3", "eth", "net", "personal", "traces"\] hosts = \["none"\] server\_threads = 1

\[websockets\] disable = true

\[ipc\] disable = true

\[footprint\] tracing = "on"

\[misc\] logging = "own\_tx=trace" log\_file = "/var/log/parity.log" color = true

```text
## 問題排除
* 錯誤訊息 : TraceDB resync required
    修改~/.local/share/io.parity.ethereum/chains/test/user_defaults，把tracing改為true
* 當啟動tracing模式的時候將無法使用warp sync

## RPC 執行範例
* Command
```

curl --data '{"method":"trace\_filter","params":\[{"fromBlock":"0x1","toBlock":"latest","fromAddress":\["0x..............."\]"\]}\],"id":1,"jsonrpc":"2.0"}' -H "Content-Type: application/json" -X POST w.x.y.z:8545

```text

```

curl -X POST --data '{"jsonrpc": "2.0", "id":"curltest", "method": "eth\_getTransactionByHash", "params": \["0x..............."\] }' -H 'content-type: application/json' [http://w.x.y.z:8545](http://w.x.y.z:8545)

```text
* Result
```

{"jsonrpc":"2.0","result":\[{....}\],"id":1} \`\`\`

