# collwallet-docs

## URI APIs
所有API都應該帶入callback參數，將結果POST到指定的URL。以getAccounts為例，想要callback的URL為：

    https://relay.host/?session=xxx
    
將會執行以下URI：

    coolwallet://getAccounts?callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx

然後Coolwallet會將JSON結果POST到callback URL
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
函式簽章: `string[] getAccounts()`  
回傳: 錢包位址列表  
URI: `coolwallet://getAccounts`

URI範例

    coolwallet://getAccounts?callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx

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

### 2. 簽章訊息
函式簽章: `string signMessage(from, data)`  
回傳: 簽章後結果  
URI: `coolwallet://signMessage?from={from}&data={data}`

URI範例

    coolwallet://signMessage?from=0x8c03377931f3ca36154399c5516370bc6d54e81e&data=0xb69342236a2c44e986a6949ad172f9a5b0e7f91f8faaa77ee2e3ce08da727278&callback=https%3A%2F%2Frelay.host%2F%3Fsession%3Dxxx

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

