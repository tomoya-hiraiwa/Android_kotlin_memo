# SQLite
## テーブル内の件数を取得する

```kotlin
val itemCount = DatabaseUtils.queryNumEntries(wdb,WordHelper.tableName)
```

・queryNumEntriesメソッド

第1引数→SQLiteDatabase, 第2引数→テーブル名

戻り値→Long
