https://www.postgresql.org/docs/9.5/static/transaction-iso.html
https://wiki.postgresql.org/wiki/Serializable

write skew
step 1. perform a query to get required data (e.g. search for a unique username, search for available meeting room)
step 2. depends on the result of step 1, the app decide where to go (continue the transaction or abort)
step 3. make a write(insert, update or delete) and commit the transaction. the effect of write changes the precondition of the decision in step2. In others word, if you retry the query in step1 after commiting the write you would get a different result.

You can try `materializing conflict` that is create another table contains  24 * 7 timesheet for meeting room(that say one hour per row), when someone try to booking in given time, the transaction will lock some row in the timesheet table. Therefore when two transaction try to book the sametime or overlap time, only one transaction can obtain the lock which mean only one transaction can succeed.

But materializing conflict is not good in some case, for example when user try to type their unique username in your website, if you want to use `materializing conflict` you have to store every possible username in the table.

## Serializable Snapshot Isolation(SSI)
SSI 不會在 tx 執行的時候做檢查, 不過在 commit 的時候, 他會去看這個 tx 是否會受到其他最近完成的 tx 影響, 導致 tx 內原本 read 的結果發生改變 (e.g. 尋找 uniq username 原本沒有 row, 現在有 row) 如果有類似情況的話, 就把 tx abort。

因為在執行 tx 的時候不會 lock, 所以被稱為 optimistic concurrency control，與之相比較, two phase locking(2PL) 或是 serial execution 因為會在 tx 執行前作檢查與 lock 所以被稱為 perssimistic concurrency control。