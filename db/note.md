##不要把全部的資料放到RMDB

###不適合放的資料
* log
* audit log

##使用搜尋系統取代直接使用rmdb搜尋
i.e. 搜尋書名


##archive old data
讓寫入的速度不會受到資料量的影響

##留意需要soft_delete的table，加入`is_delete`欄位
