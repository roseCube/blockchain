# 比特幣官方RPC設定



**tags: 比特幣 區塊鍊 bitcore**

## 比特幣官方RPC設定

* 下載 [https://bitcoincore.org/en/download/](https://bitcoincore.org/en/download/)
* 解壓縮後即可直接使用

## bitcoin.conf範例

```text
# 設定要連到testnet
testnet=1

# Run a regression test network
#regtest=0

# 跟外部同步時需要porxy
#proxy=127.0.0.1:9050

# Bind to given address and always listen on it. Use [host]:port notation for IPv6
#bind=<addr>

# Bind to given address and whitelist peers connecting to it. Use [host]:port notation for IPv6
#whitebind=<addr>

#資料的目錄
datadir=/bitcoin/bitcoind

#addnode=69.164.218.197
#addnode=10.0.0.2:8333

# Alternatively use as many connect= settings as you like to connect ONLY to specific peers
#connect=69.164.218.197
#connect=10.0.0.1:8333

# 不太清楚，但是設定1會GG
listen=0

# 讓server可以接收 JSON-RPC 命令
server=1

# 要將RPC綁定在哪個IP
rpcbind=0.0.0.0

#RCP的帳號與密碼
rpcuser=wales
rpcpassword=password
#
# The second method `rpcauth` can be added to server startup argument. It is set at intialization time
# using the output from the script in share/rpcuser/rpcuser.py after providing a username:
#
# ./share/rpcuser/rpcuser.py alice
# String to be appended to bitcoin.conf:
# rpcauth=alice:f7efda5c189b999524f151318c0c86$d5b51b3beffbc02b724e5d095828e0bc8b2456e9ac8757ae3211a5d9b16a22ae
# Your password:
# DONT_USE_THIS_YOU_WILL_GET_ROBBED_8ak1gI25KFTvjovL3gAM967mies3E=
#

# How many seconds bitcoin will wait for a complete RPC HTTP request.
# after the HTTP connection is established.
#rpcclienttimeout=30

# By default, only RPC connections from localhost are allowed.
# Specify as many rpcallowip= settings as you like to allow connections from other hosts,
# either as a single IPv4/IPv6 or with a subnet specification.

# NOTE: opening up the RPC port to hosts outside your local trusted network is NOT RECOMMENDED,
# because the rpcpassword is transmitted over the network unencrypted.

#允許RPC連線的IP
rpcallowip=w.x.y.z/24

# RPC 監聽的port:
rpcport=18333

# You can use Bitcoin or bitcoind to send commands to Bitcoin/bitcoind
# running on another host using this option:
rpcconnect=127.0.0.1

# Create transactions that have enough fees so they are likely to begin confirmation within n blocks (default: 6).
# This setting is over-ridden by the -paytxfee option.
#txconfirmtarget=n

# Miscellaneous options
# Pre-generate this many public/private key pairs, so wallet backups will be valid for
# both prior transactions and several dozen future transactions.
#keypool=100

# Pay an optional transaction fee every time you send bitcoins.  Transactions with fees
# are more likely than free transactions to be included in generated blocks, so may
# be validated sooner.
#paytxfee=0.00

# Enable pruning to reduce storage requirements by deleting old blocks.
# This mode is incompatible with -txindex and -rescan.
# 0 = default (no pruning).
# 1 = allows manual pruning via RPC.
# >=550 = target to stay under in MiB.
#prune=550

zmqpubrawtx=tcp://0.0.0.0:28332
zmqpubrawblock=tcp://0.0.0.0:28332
zmqpubhashtx=tcp://0.0.0.0:28332
zmqpubhashblock=tcp://0.0.0.0:28332
```

## 以daemon來執行

```text
bitcoind -daemon
```

## 用法

* curl

    curl --user wales:holyshit --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "uptime", "params": \[\] }' -H 'content-type: text/plain;' [http://127.0.0.1:18333](http://127.0.0.1:18333)

* postman

    使用postman必須開啟basic auth的選項，並填入帳號密碼

  **ZeroMQ支援**

  **開啟**

  firewall-cmd --permanent --add-rich-rule="rule family=ipv4 source address=w.x.y.z  port port=28332  protocol=tcp  accept"

