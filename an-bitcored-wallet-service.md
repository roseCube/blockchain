# 安裝Bitcored Wallet Service



**tags: 比特幣 區塊鍊 bitcored**

## 安裝Bitcored Wallet Service

### 安裝nvm \( npm version manager\)

```text
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

### 安裝node v8

```text
nvm install v8
```

### 安裝zeromq

```text
yum install zeromq
yum install zeromq-devel
```

### 安裝mongodb跟kerbos

```text
yum -y install  mongodb
yum -y install mongodb-server
yum -y install  krb5-libs 
yum -y install  krb5-devel
```

### 安裝 Bitcore

```text
npm install -g --unsafe-perm=true bitcore
npm install -g bitcore@4.1.0
```

### 

* 新增一個testnet的節點

  ```text
  bitcore create mynode --testnet
  ```

* 安裝錢包服務與API

  ```text
  cd mynode
  bitcore install bitcore-wallet-service
  bitcore install insight-api
  ```

  **執行服務**

  ```text
  bitcored
  ```

  由於程式有問題，所以還要改一下node\_modules/bitcore-wallet-service/bitcorenode/index.js，參考

* [https://github.com/bitpay/bitcore/issues/1454](https://github.com/bitpay/bitcore/issues/1454) 
* [https://github.com/bitpay/bitcore/issues/1461\#issuecomment-318700544](https://github.com/bitpay/bitcore/issues/1461#issuecomment-318700544)

  **開啟防火牆**

  ```text
  firewall-cmd --add-port=3232/tcp
  ```



