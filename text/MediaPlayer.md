# MediaPlayer

## MediaPlayer + SurfaceViewで動画再生

### MediaPlayerの設定

```kotlin
mediaPlayer = MediaPlayer().apply {
//web上の動画を再生する場合
            setDataSource(this@MainActivity, Uri.parse(videoUrl))
            //prepare()を呼び出すことで再生準備が完了する
            prepare()
        }
```

### MediaPlayer.setOnPreparedListener

・prepareが呼び出された時（準備が完了した時）に呼び出されるリスナ

例

```kotlin
 mediaPlayer.setOnPreparedListener {
            //再生準備の状態フラグ
            isPrepare = true
            //動画読み込み中のプログレスバーを非表示にする
            b.loadProgress.visibility = View.GONE
            //動画の全体の時間を読み込むメソッドを呼び出す(後述)
            loadAllTime()
            //シークバーの長さを動画の長さに合わせる
            timeSeek.max = it.duration
            b.playCenter.visibility = View.VISIBLE
        }
```

### SurfaceViewとMediaPlayerを結びつけるCallBackを作成

```kotlin
private val callback = object : SurfaceHolder.Callback {
        override fun surfaceCreated(holder: SurfaceHolder) {
            //MediaPlayerに画面をアタッチ
            mediaPlayer.setSurface(holder.surface)
        }

        override fun surfaceChanged(holder: SurfaceHolder, format: Int, width: Int, height: Int) {
        }

        override fun surfaceDestroyed(holder: SurfaceHolder) {
        }
    }
```

### SurfaceViewに作成したCallBackを追加

```kotlin
surface.holder.addCallback(callback)
```

### 動画再生

```kotlin
mediaPlayer.start()
```

### setOnCompleteListener

・動画の再生が完了した時に呼び出される

例

```kotlin
 mediaPlayer.setOnCompletionListener {
            //再生中フラグの切り替え
            isPlay  = false
            //再生ボタンの画像変更
            b.play.setImageResource(R.drawable.baseline_play_arrow_24)
            //画面の再生マークの表示切り替え
            b.playCenter.visibility = View.VISIBLE
        }
```


### MediaPlayerの主なメソッド

|名前|概要|
|:--:|:--|
|prepare()|再生準備状態にする。`setOnPreParedListener`が呼び出される|
|start()|再生を開始する。この呼び出しの前に`prepare()`を呼んでおく必要がある|
|isPlaying()|再生状態であるかを取得できる。`Boolean`|
|pause()|一時停止する。再開するには`start()`を呼ぶ|
|stop()|再生を停止する。もう一度再生するには`prepare()`から呼ぶ必要がある|
|seekTo(long)|再生位置を設定する、シークが完了すると`setOnSeekCompleteListener`を呼ぶ|
|.currentPosition|現在の再生位置を取得する|
|setLooping(Boolean)|trueにすると再生完了時にループ再生する|









