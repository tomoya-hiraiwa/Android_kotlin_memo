# Button

## 丸みを変更したい時

### `MaterialButton`を使用

例

```xml
<com.google.android.material.button.MaterialButton
        android:id="@+id/login_button"
        android:text="Log in"
        android:backgroundTint="@color/orange"
        app:cornerRadius="8dp"
        android:layout_marginTop="100dp"
        android:paddingBottom="10dp"
        android:paddingTop="10dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
```

・このコンポーネントを使用すると、`cornerRadius`属性で簡単に形を変更できる
