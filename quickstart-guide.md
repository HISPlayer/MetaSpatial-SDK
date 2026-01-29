# Quickstart Guide
This guide introduces the basic steps required to integrate and initialize playback functionality using the SDK.

## 1. Import Package
Download the HISPlayer Meta Spatial SDK. Copy the `hisplayer-sdk-[version].aar` file and place it in your project module’s libs/ directory. If that directory does not exist, create it manually.

<p align="center">
<img src="./images/libs-folder.jpg" style="width: 350px; height: auto;">
</p>

Then, add the following dependencies inside the `dependencies` block, in your project `build.gradle.kts` file.

```
// HISPlayer SDK Dependencies
implementation(files("libs/hisplayer-sdk-1.3.0.aar"))
implementation("org.bouncycastle:bcprov-jdk16:1.45")
implementation("androidx.annotation:annotation:1.9.1")
```

Finally, sync the project with Gradle files by selecting **Sync Project with Gradle Files** or using the shortcut `Ctrl + Shift + O`.

## 2. Configure HISPlayer
### 2.1 Import HISPlayer SDK
All public API classes of HISPlayer are located in the `com.hisplayer.sdk` package. You can import individual classes as needed, but for simplicity in this guide, we’ll import all of them using:

```
import com.hisplayer.sdk.*
```

### 2.2 Create HISPlayer SDK
`HISPlayerManager` is the central class used to control the player.
It provides all the necessary functionality for managing playback and interacting with the player.

This class can be declared as a `lateinit` variable and initialized later in your application. To initialize it, you must provide the `applicationContext` and a valid license key that you receive from HISPlayer. If you don't receive license key, please contact HISPlayer support. If the license key is invalid or missing, an exception will be thrown during initialization.

```
lateinit var hisPlayerManager: HISPlayerManager
```

```
try {
    hisPlayerManager = HISPlayerManager(this, "licenseKey")
}
catch (e: Exception) {
    Log.e(TAG, "Error creating HISPlayerManager, license key is invalid")
}
```
### 2.3 Set Log Level
Once the class is instantiated, you can specify the desired log level: `DEBUG`, `INFO`, `WARNING`, `ERROR`, or `NONE`.

```
hisPlayerManager.setLogLevel(LogLevel.INFO)
```

### 2.4 Handling HISPlayer SDK Events
`HISPlayerManager` class invokes player related events and application should handle these event as your purpose.
`HISPlayerEventListener` class defines events and you should override `onEvent` API.

```
{
    ...

    // Adding event listener
    hisPlayerManager.setEventListener(PlayerEventListener())   

    inner class PlayerEventListener() : HISPlayerEventListener() {
        override fun onEvent(eventParams: HISEventParams) {
        val playerId = eventParams.playerId

        when(eventParams.event) {
            HISPlayerEvents.PLAYBACK_READY -> {}
            HISPlayerEvents.PLAYLIST_CHANGE -> {}
            HISPlayerEvents.VIDEO_SIZE_CHANGE -> {}
            HISPlayerEvents.PLAYBACK_PLAY -> {}
            HISPlayerEvents.PLAYBACK_PAUSE -> {}
            HISPlayerEvents.PLAYBACK_STOP -> {}
            HISPlayerEvents.PLAYBACK_SEEK -> {}
            HISPlayerEvents.VOLUME_CHANGE -> {}
            HISPlayerEvents.END_OF_PLAYLIST -> {}
            HISPlayerEvents.ON_TRACK_CHANGE -> {}
            HISPlayerEvents.ON_STREAM_RELEASE -> {}
            HISPlayerEvents.TEXT_RENDER -> {}
            HISPlayerEvents.AUTO_TRANSITION -> {}
            HISPlayerEvents.PLAYBACK_BUFFERING -> {}
            HISPlayerEvents.NETWORK_CONNECTED -> {}
            HISPlayerEvents.ERROR_NETWORK_FAILED -> {}
            HISPlayerEvents.END_OF_CONTENT -> {}
            HISPlayerEvents.TIMELINE_UPDATED -> {}
            HISPlayerEvents.ERROR_DECODER_INIT_FAILED,
            HISPlayerEvents.ERROR_DECODING_FAILED -> {}
            HISPlayerEvents.ERROR_UNKNOWN -> {}
        }
    }    
}
```

## 3. Create a Stream
To create a stream you can choose below 2 options:

### 3.1 Create a Stream with MediaPanel Entity
The SDK will create a stream and automatically create a MediaPanel to display video. You need to use the `HISStreamEntityProperties` class, which requires MediaPanel properties, stream URL, and `HISPlayerProperties` instance.
The `HISPlayerProperties` class defines playback options such as autoplay and the playback strategy, specified by the `HISPlaybackStrategy` enum.

Here's an example:

```
val streamProperty = HISStreamEntityProperties(
        "https://a.com/content_url.mp4",
        HISPlayerProperties(
            content.autoPlay,
            HISPlaybackStrategy.LOOP,
            0,
            1000000,
        )
    )

val playerEntity = hisPlayer?.addStreamWithEntity(
    content.playerId,
    streamProperty,
    HISPlayerVideoShapeTypes.Rectilinear,
    HISPlayerStereoTypes.None,
    size,
    position,
    rotation,
    true,
    content.fishEyeFOV ?: 1.0f) {
        if (content.syncContentId != null) {
        hisPlayer?.setVolume(content.playerId, 0.0f)
        }
    }
```
The API takes the playerId as their first parameter (`playerId` in above example). If an invalid Id is passed, the method will throw an error.
Once the stream is created, you can use the playback control functions provided by the API, such as `hisPlayerManager.play(playerId)` or `hisPlayerManager.pause(playerId)`.

### 3.2 Create a Stream Without MediaPanel
The SDK will create a stream without automatically creating MediaPanel, you need to create the MediaPanel on your application side. You need to use the `HISStreamProperties` class, which requires a `Surface`, a stream URL, and a `HISPlayerProperties` instance.

The `HISPlayerProperties` class defines playback options such as autoplay and the playback strategy, specified by the `HISPlaybackStrategy` enum.

Here's an example:

```
val stream = HISStreamProperties(
    surface,
    "https://a.com/content_url.mp4",
    HISPlayerProperties(
        true,                       // Autoplay (Boolean)
        HISPlaybackStrategy.LOOP    // PlaybackStrategy
    )
)
hisPlayerManager.addStream(playerId, stream)
```

The API takes the playerId as their first parameter. If an invalid Id is passed, the method will throw an error.
Once the stream is created, you can use the playback control functions provided by the API, such as `hisPlayerManager.play(playerId)` or `hisPlayerManager.pause(playerId)`.

### 3.3 Local File Play
The SDK support local file playback.
To add local file input your app please follow the steps.

* Copy media file into "src/main/res/raw" folder.
* Set `HISStreamEntityProperties` or `HISStreamProperties` url parameter as file name without extension. (notice: File name should be all low-cases)
```
    Real file name : MVHEVC_Video.MOV
    Android Studio copy file name : mvhevc_video.mov
    url : mvhevc_video
```


## 4. Release HISPlayer
It is important to properly call the `hisPlayerManager.release()` method on the library before closing the application. This ensures that all internal resources are properly released.
