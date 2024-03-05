# 動画再生

## MediaPlayer + SurfaceViewの場合

[MediaPlayer](text/MediaPlayer.md)を参照

### 動画の総時間を取得

```kotlin
private fun loadAllTime(){
        val time = mediaPlayer.duration
        val minutes = time / 60000
        val seconds = (time / 1000) % 60
        val timeText = String.format("%02d:%02d",minutes,seconds)
        b.allTime.text = timeText
    }
```

### シークバーの長さを動画に合わせる

```kotlin
 seekBar.max = mediaPlayer.duration
```

### 動画の現在の再生時間を取得

```kotlin
  private fun getNowTime(){
        val handler = Handler(Looper.getMainLooper())
        handler.postDelayed(object: Runnable{
            override fun run() {
                val time = mediaPlayer.currentPosition
                println(time)
                val minutes = time / 60000
                val seconds = (time / 1000) % 60
                val timeText = String.format("%02d:%02d",minutes,seconds)
                b.nowtime.text = timeText
                handler.postDelayed(this,1000)
            }
        },0)
    }
```

### シークバーの位置を合わせる

```kotlin
private fun getNowSeek(){
        val handler = Handler(Looper.getMainLooper())
        handler.postDelayed(object: Runnable{
            override fun run() {
                timeSeek.progress = mediaPlayer.currentPosition
                handler.postDelayed(this,1000)
            }
        },0)
    }
```

### シークバーによって動画の再生位置を変更する

```kotlin
seekBar.setOnSeekBarChangeListener(object: SeekBar.OnSeekBarChangeListener{
            //シークバーが操作された時のメソッド
            override fun onProgressChanged(seekBar: SeekBar?, progress: Int, fromUser: Boolean) {
                //ユーザーの操作かどうかを確認
                if (fromUser){
                    mediaPlayer.pause()
                    mediaPlayer.seekTo(progress)
                    mediaPlayer.start()
                }
            }
            //シークバーに触れられた時の処理を記述
            override fun onStartTrackingTouch(seekBar: SeekBar?) {
            }
            //シークバーから離されたときの処理を記述
            override fun onStopTrackingTouch(seekBar: SeekBar?) {
            }
        })
```


