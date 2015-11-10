#lambda
>輕量的api功能，讓你可以專注在功能的開發，不需要考慮infrastructure。

##use lambda with API Gateway with access key
1. 進入Lambda的console，在指定function的`API endpoints`點擊`Add API endpoint` 
2. `API endpoint type`選擇`API Gateway`，`Security`選擇`Open with access key`
3. 進入API Gateway的console，點擊左上方`APIs`選單的`API Keys`
4. 從左方點擊`Create API Key`或選擇已經建立的key
5. 進入指定的`API`，在右下方的`API Stage Association`選擇要綁定的API
6. 呼叫API時，在header加入 `x-api-key`與你的 `api-key`
