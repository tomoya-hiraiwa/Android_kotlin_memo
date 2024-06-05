# Notification

## NotificationChannelの作成

```kotlin
//通知の所属するチャンネルを指定するIdを定義
val channelId = "1"

private fun createNotificationChannel() {
        if (Build.VERSION.SDK_INT >= 26) {
            //チャンネル名
            val name = "SampleChannel"
            //チャンネルの説明文(option)
            val descriptionText = "This is sample notification channel."
            //通知の重要度
            val importance = NotificationManager.IMPORTANCE_HIGH
            //通知の音声変更用audioAttributes(option)
            val audioAttributes = AudioAttributes.Builder().apply {
                setContentType(AudioAttributes.CONTENT_TYPE_SONIFICATION)
                setUsage(AudioAttributes.USAGE_ALARM)
            }.build()
            //通知チャンネルのインスタンス
            mChannel = NotificationChannel(channelId, name, importance).apply {
                description = descriptionText
                //バイブレーションのパターンを変更(option)
                vibrationPattern = longArrayOf(5, 10)
                //サウンドの設定(rawファイルに音声フォルダを配置)(option)
                setSound(
                    Uri.parse("android.resource://" + getPackageName() + "/" + R.raw.notification),
                    audioAttributes
                )
            }
            //チャンネルを登録
            val notificationManager = getSystemService(NOTIFICATION_SERVICE) as NotificationManager
            notificationManager.createNotificationChannel(mChannel)
        }
    }
```

## 通知の作成と表示

```kotlin
    private fun createSimpleNotification() {
        //各通知ごとの一意のid
        val notifyId = 0
        //通知クリック時にアプリを起動する用のインテントを作成
        val intent = Intent(this, MainActivity::class.java).apply {
            flags = Intent.FLAG_ACTIVITY_CLEAR_TASK or Intent.FLAG_ACTIVITY_NEW_TASK
        }
        val pendingIntent = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_IMMUTABLE)
        //通知を作成
        val builder = NotificationCompat.Builder(this, channelId).apply {
            //通知に表示されるアイコンを設定
            setSmallIcon(R.drawable.baseline_celebration_24)
            //通知のタイトルを設定
            setContentTitle("Simple Notification")
            //通知のテキストを設定、ここで設定できるテキストはmaxLines = 2
            setContentText("This is simple notification sample")
            //インテントを設定
            setContentIntent(pendingIntent)
            //通知クリック時にこの通知を削除
            setAutoCancel(true)
        }.build()
        //通知を表示する
        with(NotificationManagerCompat.from(this)) {
            if (ActivityCompat.checkSelfPermission(
                    this@MainActivity,
                    Manifest.permission.POST_NOTIFICATIONS
                ) != PackageManager.PERMISSION_GRANTED
            ) {
                return
            }
            notify(notifyId, builder)
        }
    }
```

## 通知のバリエーション

・長文テキスト表示用

```kotlin
 private fun createLongTextNotification() {
        val notifyId = 1
        val intent = Intent(this, MainActivity::class.java).apply {
            flags = Intent.FLAG_ACTIVITY_CLEAR_TASK or Intent.FLAG_ACTIVITY_NEW_TASK
        }
        val pendingIntent = PendingIntent.getActivity(this, 1, intent, PendingIntent.FLAG_IMMUTABLE)

        val builder = NotificationCompat.Builder(this, channelId).apply {
            setSmallIcon(R.drawable.baseline_celebration_24)
            setContentTitle("Long Text Notification")
            setContentText("This is Long text notification sample\nHello, what's up?\nI coming the beach and doing BBQ.\nwhat do you like the meat?")
            setContentIntent(pendingIntent)
            //スタイルでBigTextStyleを指定
            setStyle(NotificationCompat.BigTextStyle())
            setAutoCancel(true)
        }.build()
        with(NotificationManagerCompat.from(this)) {
            if (ActivityCompat.checkSelfPermission(
                    this@MainActivity,
                    Manifest.permission.POST_NOTIFICATIONS
                ) != PackageManager.PERMISSION_GRANTED
            ) {

                return
            }
            notify(notifyId, builder)
        }
    }
```

・画像表示用

```kotlin
 private fun createBigPictureNotification() {
        val notifyId = 2
        val builder = NotificationCompat.Builder(this, channelId).apply {
            setSmallIcon(R.drawable.baseline_celebration_24)
            setContentTitle("Big image notification")
            setContentText("This is big image notification")
            //スタイルでBigPictureStyleを設定
            setStyle(
                NotificationCompat.BigPictureStyle()
                    //画像をbitmapで渡す
                    .bigPicture(BitmapFactory.decodeResource(resources, R.drawable.sample))
                    //画像表示時にアイコンを表示しない
                    .bigLargeIcon(null)
            )
        }
            .build()
        with(NotificationManagerCompat.from(this@MainActivity)) {
            if (ActivityCompat.checkSelfPermission(
                    this@MainActivity,
                    Manifest.permission.POST_NOTIFICATIONS
                ) != PackageManager.PERMISSION_GRANTED
            ) {
                return
            }
            notify(notifyId, builder)
        }
    }
```
