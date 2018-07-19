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

### 1. 取的錢包位址
函式簽章: `string[] getAccounts()`  
URI: `coolwallet://getAccounts`

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
