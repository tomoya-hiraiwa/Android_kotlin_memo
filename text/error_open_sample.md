# サンプルプロジェクトを開くことができない時

### gradleのバージョンを変える

・gradle-wrapper.properties

```kotlin
distributionUrl=https\://services.gradle.org/distributions/gradle-7.6.4-all.zip //使用するverを指定
```

・gradle.propertiesに以下を追記

```kotlin
org.gradle.jvmargs=-Xmx1536M \
--add-exports=java.base/sun.nio.ch=ALL-UNNAMED \
--add-opens=java.base/java.lang=ALL-UNNAMED \
--add-opens=java.base/java.lang.reflect=ALL-UNNAMED \
--add-opens=java.base/java.io=ALL-UNNAMED \
--add-exports=jdk.unsupported/sun.misc=ALL-UNNAMED
```
