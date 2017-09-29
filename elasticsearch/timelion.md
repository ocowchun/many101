### 當你想要在一張圖表呈現多條直線, 不過線與線之間的差距很大時
可以使用 `.yaxis()`, 當作指定 query 的 y 軸

```
.es(q=A),.es(q=B).yaxis(2),.es(q=B).yaxis(3)
```

當我們想要比較線之間的走勢的時候很好用

### 如果只是單純想看線條的走勢, 不需要實際的數值時,也可以用 `range()`,比如說

```
.es(split=geo.country_code:5).range(0,100)
```


### 看某個 query 在整體數據所佔的比例, (i.e. 某個 path 在所有流量中的比例)
```
.es(A).divide(.es()).multiply(100)
```

### 使用 `offset` 來跟過去做比較

```
.es(), .es(offset=-1w)
```

或是我可以直接看兩者的差距
```
.es().substract(.es(offset=-1w))
```

### 使用 `fit` 來處理斷點(ie. 剛好這段時間沒有資料)
```
.es().fit(nearest)
```