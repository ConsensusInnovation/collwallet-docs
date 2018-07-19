# collwallet-docs

## URI APIs
所有API都應該帶入參數：

1. callback: 將結果POST到指定的URL
2. source: 開發商名稱
3. apiKey: Coolwallet發放給DAPP開發商的金鑰

依據callback將結果POST到指定的URL。以getAccounts為例，想要callback的URL為：

    https://relay.host/?session=xxx
    
假設source為：

    JOYSO

假設apiKey為：

    ooo

將會執行以下URI：

    coolwallet://?action=getAccounts&callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx&apiKey=ooo&source=JOYSO

然後Coolwallet應該將結果以JSON格式POST到callback URL
```
POST https://relay.host/?session=xxx
BODY
```
```JSON
{
  "result": ["0x8c03377931f3ca36154399c5516370bc6d54e81e"],
  "error": null
}
```
或者有錯誤
```JSON
{
  "result": null,
  "error": "錯誤訊息"
}
```

### 1. 取得錢包位址
URI: `coolwallet://?action=getAccounts`  
輸出: 錢包位址列表

URI範例

    coolwallet://?action=getAccounts&callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx&apiKey=ooo&source=JOYSO

回傳範例
```JSON
{
  "result": ["0x8c03377931f3ca36154399c5516370bc6d54e81e"],
  "error": null
}
```
錯誤
```JSON
{
  "result": null,
  "error": "錯誤訊息"
}
```

### 2. 簽署訊息
URI: `coolwallet://?action=signMessage`  
輸入

|參數|必填|說明|
|---|---|---|
|from|O|簽署人地址|
|data|O|簽署內容|

輸出: 簽署後結果  

URI範例

    coolwallet://?action=signMessage&from=0x8c03377931f3ca36154399c5516370bc6d54e81e&data=Message&callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx&apiKey=ooo&source=JOYSO

回傳範例
```JSON
{
  "result": "0x50d7441ad2e08c07cf296ea4bddee7c6930a0e593bb5a4cacd1901cb902f61787e28cfd4e112d0f944e107c066118e52999525cb964a2e6648fc399dadd3de901b",
  "error": null
}
```
錯誤
```JSON
{
  "result": null,
  "error": "錯誤訊息"
}
```

### 3. 簽署交易
URI: `coolwallet://?action=signTransaction`  
輸入

|參數|必填|說明|
|---|---|---|
|from|O|發送者地址|
|to||目標地址|
|value||傳送的ETH量 (wei)|
|gas||Gas limit|
|gasPrice||Gas price (wei)|
|data||交易的Input data|
|nonce||Nonce|

輸出: 簽署後Transaction Hash  

URI範例

    coolwallet://?action=signTransaction&from=0x8c03377931f3ca36154399c5516370bc6d54e81e&to=0x7037734b180c44b7041a31666486f81f45860541&value=0xde0b6b3a7640000&gas=0x5208&gasPrice=0xee6b2800&nonce=0x141&data=0x1234&callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx&apiKey=ooo&source=JOYSO

回傳範例
```JSON
{
  "result": "0x8c8bfa46d2a68b7994049653ec24ddd6849ab5e84cba886e527b77ad391ff26e",
  "error": null
}
```
錯誤
```JSON
{
  "result": null,
  "error": "錯誤訊息"
}
```
