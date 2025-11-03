# HISPlayer API
The SDK exposes the following classes and enums, which are essential for integration and playback control.

## HISPlayerManager (class)
`HISPlayerManager` is the main class of the SDK. It is required to add and manage streams, and to invoke the playback control functions provided by the library.

### Constructor

* **public HISPlayerManager(Context applicationContext, String license)**: Constructor of the `HISPlayerManager` class.
  * `applicationContext`: The global application context. It is recommended to use `getApplicationContext()` to avoid memory leaks caused by passing activity or view contexts.
  * `licenseKey`: License key for making the SDK works. If license key is not valid, an exception will be thrown.

### Methods
The `playerId` parameter refers to the index of the stream, based on the order of creation.

* **public final void setLogLevel([HISLogLevel](#hisloglevel-enum) level)**: Sets the log verbosity level for the SDK.
  * `level`: Specifies the log level. Possible values are `DEBUG`, `INFO`, `WARNING`, `ERROR`, or `NONE`.

* **public final void addStream(int playerId, [HISStreamProperties](#hisstreamproperties-class) stream)**: Adds a new video stream to be managed by the SDK.
  * `stream`: The configuration object for a single video stream.

* **public final void addStreamWithEntity(int playerId, [HISStreamEntityProperties](#hisstreamentityproperties-class) stream, [HISPlayerVideoShapeTypes](#hisplayervideoshapetypes-enum) shapeType, [HISPlayerStereoTypes](#hisplayerstereotypes-enum) stereoMode, float size, Vector3 position, Vector3 rotation, Boolean show, float fishEyeFOV, () -> Unit onComplete)**: Adds a new video stream and Media Panel to be managed by the SDK.
  * `stream`: The configuration object for a single video stream.
  * `shapeType`: MediaPanel type.
  * `stereoMode`: Stereoscopic video type.
  * `size`: Panel size in the scene (meter unit)
  * `position`: Initial Panel position
  * `rotation`: Initial Panel rotation for each axis
  * `show`: Initial Panel show status
  * `fishEyeFOV`: FishEye transform fov parameter. (0.0 ~ 1.0)
  * `onComplete`: Callback for completion of create Media Panel and addStream to HISPlayer 

<!-- * **public final void addMultiStreams([HISMultiStreamProperties](#hismultistreamproperties-class) streams)**: Adds multiple video streams simultaneously.
  * `streams`: The configuration object containing multiple video stream definitions. -->

* **public final void removeStream(int playerId)**: Removes a stream from the SDK.

* **public final int getTotalPlayers()**: Returns the current number of active streams managed by the SDK.

* **public final void play(int playerId)**: Starts or resumes playback of the specified stream.

* **public final void pause(int playerId)**: Pauses playback of the specified stream.

* **public final void stop(int playerId)**: Stops playback of the specified stream.

* **public final void seek(int playerId, long milliseconds)**: Seeks to the specified position in the video stream.
  * `milliseconds`: The position to seek to, in milliseconds.

* **public final long getVideoPosition(int playerId)**: Returns the current playback position of the stream in milliseconds.

* **public final long getVideoDuration(int playerId)**: Returns the total duration of the video stream in milliseconds.

* **public final float getVolume(int playerId)**: Returns the audio volume of the video stream.

* **public final void setVolume(int playerId, float volume)**: Sets the playback volume for the specified stream.
  * `volume`: A float value between `0.0` (mute) and `1.0` (maximum volume).

* **public final boolean isPlaying(int playerId)**: Returns whether the video is playing.

* **public final boolean isBuffering(int playerId)**: Returns whether the video is buffering.

* **public final boolean isLive(int playerId)**: Returns whether the video stream is live.

* **public final long getSpeedRate(int playerId)**: Returns the speed rate of the video stream.

* **public final void setSpeedRate(int playerId, float speed)**: Sets the playback speed rate for the specified stream.
  * `speed`: A float value must be greater than `0.0`, default value is `1.0` (normal speed).

* **public final List<HISPlayerVideoTrack> getVideoTracks(int playerId)**: Provides the list of the video tracks of the current video playback. Refer to **HISPlayerVideoTrack** class.

* **public final int getVideoTrackCount(int playerId)**: Get the number of tracks of a certain stream.

* **public final void setVideoTrack(int playerId, int trackIndex)**: Select a certain track of a certain stream to be used as the main track. This action will disable ABR, to enable it again you can use **enableABR** API. The possible tracks can be obtained from the tracks returned from the method **getVideoTracks**.
  * `trackIndex`: Index of the track to be selected.

* **public final void enableABR(int playerId)**: Enables the ABR to change automatically between tracks.

* **public final void disableABR(int playerId)**: Disables the ABR to prevent the player from changing tracks regardless of bandwidth.

* **public final void setMaxBitrate(int playerId, int bitrate)**: Set a new maximum bitrate (in bits per second) of a specific track. This doesn’t disable ABR. The possible tracks can be obtained from the tracks returned from the method **getVideoTracks**.
  * `bitrate`: The new maximum bitrate.

* **public final void setMinBitrate(int playerId, int bitrate)**: Set a new minimum bitrate (in bits per second) of a specific track. This doesn’t disable ABR. The possible tracks can be obtained from the tracks returned from the method **getVideoTracks**.
  * `bitrate`: The new minimum bitrate.

* **public final void setStreamSynchronization(int mainId, int followerId)**: Link the main video stream playback and the follower video stream playback for synchronization. The follower stream will be synchronized following the main stream. If playback controls (Play, Pause, Stop, Seek, SetSpeedRate) are applied to the main stream, it will be applied to the follower stream as well. In case of multistreams, this API needs to be called for every follower stream.
  * `mainId`: The id of the main video.
  * `followerId`: The id of the follower video.

* **public final void unsetStreamSynchronization(int mainId, int followerId)**: Unlink the main video stream playback and the follower video stream playback synchronization. The follower stream will be unsynchronized from the main stream. In case of multistreams, this API needs to be called for every follower stream.
  * `mainId`: The id of the main video.
  * `followerId`: The id of the follower video.

* **public final void release()**: Releases all SDK resources and performs necessary cleanup. This should be called before the application is closed to prevent memory leaks or unexpected behavior.

### Virtual Methods (Can be overridden)
The `eventParams` parameter provides different details depending on the event type. Specifically, its `stringParam` field contains a description relevant to the specific event.

* **public void eventPlaybackReady([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs when the current playback of a stream is ready to be used. Calling functions before this event is triggered will provide null information.
  <!--* `eventParams.param1`: Number of tracks of the playback.-->

* **public void eventPlaylistChange([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the current playlist has been modified. It could happen when an URL has been added or deleted.
  * `eventParams.param1`: Playlist length.

* **public void eventVideoSizeChange([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the internal video size of the current track changes.
  * `eventParams.param1`: Width of the video.
  * `eventParams.param2`: Height of the video.

* **public void eventPlaybackPlay([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback has been played.

* **public void eventPlaybackPause([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback has been paused.

* **public void eventPlaybackStop([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback has been stopped.

* **public void eventPlaybackSeek([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback has been sought to a new time position.
  * `eventParams.param1`: Value of the old track position in milliseconds.
  * `eventParams.param2`: Value of the new track position in milliseconds.

* **public void eventVolumeChange([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the volume has been modified.
  * `eventParams.param1`: New value for the volume.

* **public void eventEndOfPlaylist([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playlist reaches the end of the list.

* **public void eventOnTrackChange([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the tracks of the stream have changed. This event is not triggered by the ABR feature.
  <!--* `eventParams.param1`: Number of video tracks available.-->
  <!--* `eventParams.param2`: Number of subtitles tracks available.-->
  <!--* `eventParams.param3`: Number of audio tracks available.-->

* **public void eventOnStreamRelease([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever a player/stream has been released.
  * `eventParams.param1`: Number of players after releasing.

<!--* **public void eventTextRender(HISEventParams eventParams)**: This event occurs whenever a caption’s text has been generated.
  * `eventParams.param1`: The next generated caption text.-->

* **public void eventAutoTransition([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs when the playback has changed to the next video in the playlist automatically.

* **public void eventPlaybackBuffering([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback is buffering.

* **public void eventNetworkConnected([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback reaches the end of the video content.

* **public void eventErrorNetworkFailed([HISEventParams](#hiseventparams-class) eventParams)**: This error occurs when the Internet connection fails.

* **public void eventEndOfContent([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever an internal playback reaches the end of the video content.

* **public void eventTimelineUpdated([HISEventParams](#hiseventparams-class) eventParams)**: This event occurs whenever the timeline of the current video has been updated. In the case of live content this may happen every certain time during the playback. This may change the current video position value from `GetVideoPosition()`.

## HISStreamProperties (class)
`HISStreamProperties` is the class that defines the required parameters for creating a video stream.

### Constructors

* **public HISStreamProperties(Surface surface, String urls, String keyServerUrls, @Nullable Pair<String, String> drmToken, [HISPlaybackProperties](#hisplaybackproperties-class) properties)**: Constructor of the `HISStreamProperties` class when DRM is to be used.
  * `surface`: The `Surface` where the video will be rendered.
  * `urls`: The URL pointing to the video content to be streamed.
  * `keyServerUrls`: The DRM license server URL required to retrieve decryption keys for protected content.
  * `drmToken`: custom HTTP header parameter for DRM license key request. (Optional)
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

## HISStreamEntityProperties (class)
`HISStreamEntityProperties` is the class that defines the required parameters for creating a video stream and Media Panel Entity.

### Constructors

* **public HISStreamEntityProperties(String urls, String keyServerUrls, @Nullable Pair<String, String> drmToken, [HISPlaybackProperties](#hisplaybackproperties-class) properties)**: Constructor of the `HISStreamEntityProperties` class when DRM is to be used.
  * `urls`: The URL pointing to the video content to be streamed.
  * `keyServerUrls`: The DRM license server URL required to retrieve decryption keys for protected content. 
  * `drmToken`: custom HTTP header parameter for DRM license key request. (Optional)
  * `properties`: An instance of `HISPlayerProperties` defining playback settings such as autoplay or playback strategy.

* **public HISStreamEntityProperties(String urls, [HISPlaybackProperties](#hisplaybackproperties-class) properties)**: Constructor of the `HISStreamEntityProperties` class.
  * `urls`: The URL pointing to the video content to be streamed.
  * `properties`: An instance of `HISPlayerProperties` defining playback settings such as autoplay or playback strategy.

### Methods
* **public Surface getSurface()**
* **public String getUrl()**
* **public String getKeyServerUrl()**
* **public [HISPlaybackProperties](#hisplaybackproperties-class) getProperties()**


## HISPlayerProperties (class)

### Constructor

* **public HISPlayerProperties(boolean autoPlay, [HISPlaybackStrategy](#hisplaybackstrategy-enum) playbackStrategy, int maxBitrate)**: Constructor of the `HISPlayerProperties` class.
  * `autoPlay`: Plays the video automatically once it has loaded.
  * `playbackStrategy`: Behavior when video playback is terminated.
  * `maxBitrate`: Set the maximum bitrate of the video. This is optional, can be left empty.

### Methods
* **public boolean isAutoPlay()**
* **public [HISPlaybackStrategy](#hisplaybackstrategy-enum) getPlaybackStrategy()**

## HISPlayerEntity (class)

### Constructor

* **public HISPlayerEntity(int playerId, [HISPlayerManager](#hisplayermanager-class) hisPlayer, [HISPlayerStreamEntityProperties](#hisplayerstreamentityproperties-class) stream, float size, Vector3 position, Vector3 rotation, Boolean show, float fishEyeFOV, ()->Unit onComplete )**: Constructor of the `HISPlayerEntity` class.
  * `hisPlayer`: HISPlayerManager instance.
  * `stream`: The configuration object for a single video stream.
  * `shapeType`: MediaPanel type.
  * `stereoMode`: Stereoscopic video type.
  * `size`: Panel size in the scene (meter unit)
  * `position`: Initial Panel position
  * `rotation`: Initial Panel rotation for each axis
  * `show`: Initial Panel show status
  * `fishEyeFOV`: FishEye transform fov parameter. (0.0 ~ 1.0)
  * `onComplete`: Callback for completion of create Media Panel and addStream to HISPlayer

### Methods
* **public static void create(int playerId, [HISPlayerManager](#hisplayermanager-class) hisPlayer, [HISStreamEntityProperties](#hisstreamentityproperties-class) stream, float size, Vector3 position, Vector3 rotation, Boolean show, float fishEyeFOV, ()->Unit onComplete)**:
Static function to create HISPlayerEntity instance.
  * `hisPlayer`: HISPlayerManager instance.
  * `stream`: The configuration object for a single video stream.
  * `shapeType`: MediaPanel type.
  * `stereoMode`: Stereoscopic video type.
  * `size`: Panel size in the scene (meter unit)
  * `position`: Initial Panel position
  * `rotation`: Initial Panel rotation for each axis
  * `show`: Initial Panel show status
  * `fishEyeFOV`: FishEye transform fov parameter. (0.0 ~ 1.0)
  * `onComplete`: Callback for completion of create Media Panel and addStream to HISPlayer
* **public void setVideoResolution(Float width, Float height)**:
Adjust Media Panel's aspect ration based on video width and height.
  * `width`: video width
  * `height`: video height

* **public void setFishEyeFOV(Float param)**:
Set FishEye Shader FOV parameter.
  * `param`: fishEye FOV value. range from 0.0 to 1.0

* **public void destroy()**:
Destroy Media Panel

## HISEventParams (class)
Container class with event information:
* **public [HISPlayerEvents](#hisplayerevents-enum) event**: Type of event.
* **public int playerId**: Index of the stream that throw the event.
* **public float param1, param2, param3, param4**: Parameters with values that may vary depending on the type of event.
* **public String stringParam**: Description of the event and the parameters being used.

## HISPlayerVideoTrack (class)
Container class with video track information:
* **public int trackIndex**: Index of the track.
* **public String id**: Id of the track.
* **public int width**: Width of the track.
* **public int height**: Height of the track.
* **public int bitrate**: Bitrate of the track.
* **public float framerate**: Framerate of the track.

## HISPlayerVideoShapeTypes (enum)
* Rectilinear: Rectangle Video
* Equirect180: VR180 Video
* Equirect360: VR360 Video
* FishEye180: FishEye 180 Video
* FishEye360: FishEye 360 Video

## HISPlayerStereoTypes (enum)
* None
* LeftRight
* UpDown
* MonoLeft
* MonoUp


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
