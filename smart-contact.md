# 智能合約



**tags: 乙太坊 區塊鍊**

## 智能合約

* 使用remix.ethereum.org來編寫與發佈合約
* 使用官方最簡易的智能合約範本

  \`\`\`

  pragma solidity ^0.4.20;

contract MyToken { / _This creates an array with all balances_ / mapping \(address =&gt; uint256\) public balanceOf;

```text
/* Initializes contract with initial supply tokens to the creator of the contract */
function MyToken(
    uint256 initialSupply
    ) public {
    balanceOf[msg.sender] = initialSupply;              // Give the creator all initial tokens
}

/* Send coins */
function transfer(address _to, uint256 _value) public {
    require(balanceOf[msg.sender] >= _value);           // Check if the sender has enough
    require(balanceOf[_to] + _value >= balanceOf[_to]); // Check for overflows
    balanceOf[msg.sender] -= _value;                    // Subtract from the sender
    balanceOf[_to] += _value;                           // Add the same to the recipient
}
```

}

```text
待補充...

* eth_call : 呼叫智能合約的函數
  * from: 哪個帳戶發起的
  * to: 智能合約的token
  * data: 加密過的資料，格式為function的hash取前十碼，後面加上64個位元的參數，不足64的部分前面要補0(例如錢包地址有40個字元前面就要補24個0)
  * 取function hash的方式 :
    * web3.sha3('transfer(address,uint256)').slice(0,10) -> "0xa9059cbb"
    * web3.sha3('balanceOf(address)').slice(0,10) -> "70a08231"
  >*函數裡面的參數是型態，不是變數名稱 !!*
  * 範例
    * 查詢0x72f186455251d78e4069583e0e18d2691788b0fd的帳戶餘額，最後面加上argument "latest"表示抓最新的紀錄
    ```curl 127.0.0.1:8545 -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xe4bf710bf189b58e0b29b7f22a6e05ae14827d92", "data":"0x70a0823100000000000000000000000072f186455251d78e4069583e0e18d2691788b0fd"},"latest"],"id":1}'
```

* 查詢0x12675f827d591f177a4b0e87387617d45fe2911a的帳戶餘額，最後的 `curl 127.0.0.1:8545 -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{"to":"0xyyyyy", "data":"0xxxxxx"},"latest"],"id":2}'`
  * eth\_sendTransaction : 呼叫智能合約的轉帳函數
  * 參數的帶法類似eth\_call，不用加argument
  * 呼叫成功後會帶回Transaction Receipt

    `{"jsonrpc":"2.0","id":3,"result":"0xzzzz"}` 可以用`eth.getTransactionReceipt`取回收據的內容

  * 呼叫範例: 
  * * `0xa9059cbb是`transfer的sha
    * zzzzz是收款者地址
    * xxxxx是發款者地址
    * yyyyy是合約地址
    * `000000000000000000000000000000000000000000000000000000000000012c`

    _是匯款金額，不足64個位元要全部補0_
  * `curl 127.0.0.1:8545 -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{"from":"0xxxxxx", "to":"0xyyyyy","value":"x00", "data":"0xa9059cbb000000000000000000000000zzzzz000000000000000000000000000000000000000000000000000000000000012c"}],"id":3}'`



