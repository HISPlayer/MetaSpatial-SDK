# HISPlayer API
The SDK exposes the following classes and enums, which are essential for integration and playback control.

## HISPlayerManager (class)
`HISPlayerManager` is the main class of the SDK. It is required to add and manage streams, and to invoke the playback control functions provided by the library.

### Constructor

* **public HISPlayerManager(Context applicationContext, String license)**: Constructor of the `HISPlayerManager` class.
  * `applicationContext`: The global application context. It is recommended to use `getApplicationContext()` to avoid memory leaks caused by passing activity or view contexts.
  * `licenseKey`: License key for making the SDK works. If license key is not valid, an exception will be thrown.

### Methods
The `playerIndex` parameter refers to the index of the stream, based on the order of creation.

* **public final void setLogLevel([HISLogLevel](#hisloglevel-enum) level)**: Sets the log verbosity level for the SDK.
  * `level`: Specifies the log level. Possible values are `DEBUG`, `INFO`, `WARNING`, `ERROR`, or `NONE`.

* **public final void addStream([HISStreamProperties](#hisstreamproperties-class) stream)**: Adds a new video stream to be managed by the SDK.
  * `stream`: The configuration object for a single video stream.

* **public final void addMultiStreams([HISMultiStreamProperties](#hismultistreamproperties-class) streams)**: Adds multiple video streams simultaneously.
  * `streams`: The configuration object containing multiple video stream definitions.

* **public final void removeStream(int playerIndex)**: Removes a stream from the SDK.

* **public final int getTotalPlayers()**: Returns the current number of active streams managed by the SDK.

* **public final void play(int playerIndex)**: Starts or resumes playback of the specified stream.

* **public final void pause(int playerIndex)**: Pauses playback of the specified stream.

* **public final void stop(int playerIndex)**: Stops playback of the specified stream.

* **public final void seek(int playerIndex, long milliseconds)**: Seeks to the specified position in the video stream.
  * `milliseconds`: The position to seek to, in milliseconds.

* **public final long getVideoPosition(int playerIndex)**: Returns the current playback position of the stream in milliseconds.

* **public final long getVideoDuration(int playerIndex)**: Returns the total duration of the video stream in milliseconds.

* **public final void setVolume(int playerIndex, float volume)**: Sets the playback volume for the specified stream.
  * `volume`: A float value between `0.0` (mute) and `1.0` (maximum volume).

* **public final void release()**: Releases all SDK resources and performs necessary cleanup. This should be called before the application is closed to prevent memory leaks or unexpected behavior.

### Virtual Methods (Can be overridden)
The `eventParams` parameter provides different details depending on the event type. Specifically, its `_stringParam` field contains a description relevant to the specific event.

* **public void eventPlaybackReady([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs when the current playback of a stream is ready to be used. Calling functions before this event is triggered will provide null information.
  <!--* `eventParams._param1`: Number of tracks of the playback.-->

* **public void eventPlaylistChange([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the current playlist has been modified. It could happen when an URL has been added or deleted.
  * `eventParams._param1`: Playlist length.

* **public void eventVideoSizeChange([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the internal video size of the current track changes.
  * `eventParams._param1`: Width of the video.
  * `eventParams._param2`: Height of the video.

* **public void eventPlaybackPlay([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback has been played.

* **public void eventPlaybackPause([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback has been paused.

* **public void eventPlaybackStop([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback has been stopped.

* **public void eventPlaybackSeek([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback has been sought to a new time position.
  * `eventParams._param1`: Value of the old track position in milliseconds.
  * `eventParams._param2`: Value of the new track position in milliseconds.

* **public void eventVolumeChange([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the volume has been modified.
  * `eventParams._param1`: New value for the volume.

* **public void eventEndOfPlaylist([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playlist reaches the end of the list.

* **public void eventOnTrackChange([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the tracks of the stream have changed. This event is not triggered by the ABR feature.
  <!--* `eventParams._param1`: Number of video tracks available.-->
  <!--* `eventParams._param2`: Number of subtitles tracks available.-->
  <!--* `eventParams._param3`: Number of audio tracks available.-->

* **public void eventOnStreamRelease([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever a player/stream has been released.
  * `eventParams._param1`: Number of players after releasing.

<!--* **public void eventTextRender(HISEventParams eventParams)**: This event occurs whenever a caption’s text has been generated.
  * `eventParams._param1`: The next generated caption text.-->

* **public void eventAutoTransition([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs when the playback has changed to the next video in the playlist automatically.

* **public void eventPlaybackBuffering([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback is buffering.

* **public void eventNetworkConnected([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback reaches the end of the video content.

* **public void eventErrorNetworkFailed([HISEventParams](#hiseventparams-class) eventParams)**: This error occurs when the Internet connection fails.

* **public void eventEndOfContent([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback reaches the end of the video content.

* **public void eventTimelineUpdated([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the timeline of the current video has been updated. In the case of live content this may happen every certain time during the playback. This may change the current video position value from `GetVideoPosition()`.

## HISStreamProperties (class)
`HISStreamProperties` is the class that defines the required parameters for creating a video stream.

### Constructors

* **public HISStreamProperties(Surface surface, String urls, String keyServerUrls, [HISPlaybackProperties](#hisplaybackproperties-class) properties)**: Constructor of the `HISStreamProperties` class when DRM is to be used.
  * `surface`: The `Surface` where the video will be rendered.
  * `urls`: The URL pointing to the video content to be streamed.
  * `keyServerUrls`: The DRM license server URL required to retrieve decryption keys for protected content.
  * `properties`: An instance of `HISPlayerProperties` defining playback settings such as autoplay or playback strategy.

* **public HISStreamProperties(Surface surface, String urls, [HISPlaybackProperties](#hisplaybackproperties-class) properties)**: Constructor of the `HISStreamProperties` class.
  * `surface`: The `Surface` where the video will be rendered.
  * `urls`: The URL pointing to the video content to be streamed.
  * `properties`: An instance of `HISPlayerProperties` defining playback settings such as autoplay or playback strategy.

### Methods
* **public Surface getSurface()**
* **public String getUrl()**
* **public String getKeyServerUrl()**
* **public [HISPlaybackProperties](#hisplaybackproperties-class) getProperties()**

## HISMultiStreamProperties (class)

### Constructor

* **public HISMultiStreamProperties([HISStreamProperties](#hisstreamproperties-class)[] streams)**: Constructor of the `HISMultiStreamProperties` class.
  * `streams`: Array of HISStreamProperties.

### Methods
* **public [HISStreamProperties](#hisstreamproperties-class)[] getStreams()**

## HISPlayerProperties (class)

### Constructor

* **public HISPlayerProperties(boolean autoPlay, [HISPlaybackStrategy](#hisplaybackstrategy-enum) playbackStrategy)**: Constructor of the `HISPlayerProperties` class.
  * `autoPlay`: Plays the video automatically once it has loaded.
  * `playbackStrategy`: Behavior when video playback is terminated.

### Methods
* **public boolean isAutoPlay()**
* **public [HISPlaybackStrategy](#hisplaybackstrategy-enum) getPlaybackStrategy()**

## HISEventParams (class)
Container class with event information:
* **public [HISPlayerEvents](#hisplayerevents-enum) _event**: Type of event.
* **public int _playerIndex**: Index of the stream that throw the event.
* **public float _param1, _param2, _param3, _param4**: Parameters with values that may vary depending on the type of event.
* **public String _stringParam**: Description of the event and the parameters being used.

## HISPlayerEvents (enum)
* PLAYBACK_READY
* PLAYLIST_CHANGE
* VIDEO_SIZE_CHANGE
* PLAYBACK_PLAY
* PLAYBACK_PAUSE
* PLAYBACK_STOP
* PLAYBACK_SEEK
* VOLUME_CHANGE
<!--* END_OF_PLAYLIST-->
* ON_TRACK_CHANGE
* ON_STREAM_RELEASE
<!--* TEXT_RENDER-->
* AUTO_TRANSITION
* PLAYBACK_BUFFERING
<!--* NETWORK_CONNECTED-->
<!--* ERROR_NETWORK_FAILED-->
* END_OF_CONTENT
* TIMELINE_UPDATED

## HISPlaybackStrategy (enum)
* STOP_AFTER_END
* LOOP

## HISLogLevel (enum)
* DEBUG
* INFO
* WARNING
* ERROR
* NONE
