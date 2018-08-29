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
|raw||Hash前的內容(`data = keccak256(raw)`, 參見簽署訊息格式))|

* 最後進行簽署的內容應為`'\x19Ethereum Signed Message:\n' + data.length + data`
* 例如 0xbbf0ab1e9bd8cff92628e7fa4a6044985df9399c8d3c7de6897d980b3e312734 的長度為32

輸出: 簽署後結果  

URI範例

    coolwallet://?action=signMessage&from=0x8c03377931f3ca36154399c5516370bc6d54e81e&data=0xbbf0ab1e9bd8cff92628e7fa4a6044985df9399c8d3c7de6897d980b3e312734&raw=0x04f062809b244e37e7fdc21d9409469c989c234200000000000000000000000000000000000000000000000000000000000f42400000000000000000000000000000000000000000000000000000b5e620f48000000000000000000000000000000000000000000000000000000000000005b8d85b50643f000a000500007d00dde12a12a6f67156e0da672be05c374e1b0a3e57&callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx&apiKey=ooo&source=JOYSO

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

輸出: 簽署後Raw Transaction  

URI範例

    coolwallet://?action=signTransaction&from=0x8c03377931f3ca36154399c5516370bc6d54e81e&to=0x7037734b180c44b7041a31666486f81f45860541&value=0xde0b6b3a7640000&gas=0x5208&gasPrice=0xee6b2800&nonce=0x141&data=0x1234&callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx&apiKey=ooo&source=JOYSO

回傳範例
```JSON
{
  "result": "0xf86e827f738509502f900082c350947b0db0f1af5920977746c632d107b302b5d669da880354c86135f350008026a0f57dd7cc619adbb3fd57e07063b970d5bba48205baf073dbe67fa32c3ecc06bda079b8ce6d8c59d53ab3e6d081ce48c348593a18e2fc9993e72b4e2023c4c4355e",
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

## 簽署訊息格式


### 1. 下單
長度: 296字元

範例:
賣出 1 JOY @ 價格 0.0002

04f062809b244e37e7fdc21d9409469c989c234200000000000000000000000000000000000000000000000000000000000f42400000000000000000000000000000000000000000000000000000b5e620f48000000000000000000000000000000000000000000000000000000000000005b8d85b50643f000a000500007d00dde12a12a6f67156e0da672be05c374e1b0a3e57

解析

|名稱|片段|說明|
|---|---|---|
|JOYSO合約位址|04f062809b244e37e7fdc21d9409469c989c2342||
|賣出的量|00000000000000000000000000000000000000000000000000000000000f4240|十進位1000000，JOY小數為6位，等值1 JOY|
|買入的量|0000000000000000000000000000000000000000000000000000b5e620f48000|十進位200000000000000，ETH小數為18位，等值0.0002 ETH|
|礦工費|000000000000000000000000000000000000000000000000000000000005b8d8|此範例為使用JOY支付。十進位375000，等值0.375 JOY|
|Nonce|5b50643f|隨機亂數，Timestamp|
|Taker手續費|000a|十進位10，表示0.1%|
|Maker手續費|000a|十進位5，表示0.05%|
|JOY報價|00007d0|當此欄位有值，表示使用JOY支付；否則為ETH。十進位2000，表示0.0002|
|買或賣|0|0表示賣出，1表示買入|
|代幣地址|dde12a12a6f67156e0da672be05c374e1b0a3e57|此範例為JOY|

### 2. Withdraw
長度: 232字元

範例:
提取 1 JOY, 手續費 0.25 JOY

04f062809b244e37e7fdc21d9409469c989c234200000000000000000000000000000000000000000000000000000000000b71b0000000000000000000000000000000000000000000000000000000000003d0905b51675d0000000000000001dde12a12a6f67156e0da672be05c374e1b0a3e57

解析

|名稱|片段|說明|
|---|---|---|
|JOYSO合約位址|04f062809b244e37e7fdc21d9409469c989c2342||
|提取的量|00000000000000000000000000000000000000000000000000000000000b71b0|十進位750000，JOY小數為6位，等值0.75 JOY|
|礦工費|000000000000000000000000000000000000000000000000000000000003d090|此範例為使用JOY支付。十進位250000，等值0.25 JOY|
|Nonce|5b51675d|隨機亂數，Timestamp|
|未使用|000000000000000||
|付款方式|1|0: ETH, 1: JOY, 2: 當前代幣|
|代幣地址|dde12a12a6f67156e0da672be05c374e1b0a3e57|此範例為JOY|

### 3. 登入
格式:
joyso{timestamp}

範例:
joyso1532080683
