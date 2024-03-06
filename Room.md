# Room

## 使用方法

### 依存関係の追加

```kotlin
ksp("androidx.room:room-compiler:$room_version")
implementation("androidx.room:room-runtime:$room_version")
```

### Roomの構成

・Roomは主に以下3つのコンポーネントで構成される

| クラス | 用途 |
|:---|:---|
|データエンティティ|テーブルを表す|
|データ アクセス オブジェクト|DAOと呼ばれる、データベースのクエリメソッドを定義する|
|データベースクラス|データベースの保持、アクセスポイントの提供|

### エンティティの作成

例

```kotlin
@Entity(tableName = "item")
data class Item(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    @ColumnInfo(name = "name")
    val itemName: String,
    @ColumnInfo(name = "price")
    val itemPrice: Double,
    @ColumnInfo(name = "quantity")
    val quantityInStock: Int
)
```

・エンティティクラスはデータクラスで作成、`@Entity`アノテーションをつける

・主キーには`@PrimaryKey`をつける

・各列は`name=列名`で定義する

### DAOの作成

例

```kotlin
@Dao
interface ItemDao {
    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(item: Item)
    @Update
    suspend fun update(item: Item)
    @Delete
    suspend fun delete(item: Item)

    @Query("SELECT * FROM item WHERE id = :id")
    fun getItem(id: Int): Flow<Item>

    @Query("SELECT * FROM item ORDER BY name ASC")
    fun getItems(): Flow<List<Item>>
}
```

・`@Dao`アノテーションをつける

・各メソッドはアノテーションで定義し、引数の形はエンティティクラスのデータクラス

・`@Query`の場合は、後ろにクエリ文を書く

・データベースアクセスのため、suspend関数とする

・`@Insert`の`onConflict = OnConflictStrategy.IGNORE`は、重複するデータの場合、挿入しない設定

### データベースクラスの作成

例

```kotlin
@Database(entities = [Item::class], version = 1, exportSchema = false)
abstract class ItemRoomDatabase: RoomDatabase(){
    abstract fun itemDao(): ItemDao
    companion object {
        @Volatile
        private var INSTANCE: ItemRoomDatabase? = null
        fun getDatabase(context: Context): ItemRoomDatabase{
            return INSTANCE ?: synchronized(this){
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    ItemRoomDatabase::class.java,
                    "item_database"
                ).fallbackToDestructiveMigration()
                    .build()
                return instance
            }
        }
    }
}
```

・`@Database`アノテーションをつける

・abstractでRoomDatabaseを拡張する

・objectとしてItemRoomDatabaseのインスタンスを持つことで、複数のインスタンスが開かれることを防ぐ

### Roomデータベースの呼び出し

・データベースのインスタンス化

・呼び出し用のクラスを作成する場合

```kotlin
class InventoryApplication : Application(){
    val database: ItemRoomDatabase by lazy { ItemRoomDatabase.getDatabase(this) }
}
```

・ViewModelの作成

```kotlin
class InventoryViewModel(private val itemDao: ItemDao): ViewModel() {
    private fun insertItem(item: Item){
        viewModelScope.launch {
            itemDao.insert(item)
        }
    }
}
```

・VireModelFactoryの作成

```kotlin
class InventoryViewModelFactory(private val itemDao: ItemDao): ViewModelProvider.Factory{
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(InventoryViewModel::class.java)){
            @Suppress("UNCHECKED_CAST")
            return InventoryViewModel(itemDao) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}
```

・ViewModelの引数ItemDaoを渡すためにFactoryクラスを作成

・viewModelの呼び出し

```kotlin
 private val viewModel: InventoryViewModel by activityViewModels {
        InventoryViewModelFactory(
            (activity?.application as InventoryApplication).database
                .itemDao()
        )
    }
```

・データの追加

```kotlin
private fun insertItem(item: Item){
        viewModelScope.launch {
            itemDao.insert(item)
        }
    }
```

・ViewModelで`viewModelScope`を用いて非同期処理

・引数に`ItemDao`が渡されているので、そのクラス内のinsertメソッドにアクセス

・他のリクエストも`itemDao.~`で書くことができる


