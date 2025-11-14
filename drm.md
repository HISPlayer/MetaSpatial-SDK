# DRM

In the video streaming industry, the DRM (Digital Rights Management) makes possible a secure distribution of contents over the network.
Encrypted content is prepared using an encryption server and stored in a content library. The encrypted content is streamed or downloaded from the content library to client devices via content servers. Licenses to view the content are obtained from the License Server.

The HISPlayer SDK for Meta Spatial SDK supports [Widevine DRM](https://www.widevine.com/solutions/widevine-drm) and provides an API to the users for usage of the MediaDRM module.

You may find the Widevine DRM video content URL and the key server URL sample here: [Widevine DRM Sample](https://integration.widevine.com/player). 

## Create a Stream with DRM Protected Video 
To create a stream with DRM protected video, you need to use the `HISStreamProperties` class, which requires a `Surface`, a stream URL, a Widevine key server URI and a `HISPlayerProperties` instance.

Here's an example:

```
val stream = HISStreamProperties(
    surface,
    "https://storage.googleapis.com/wvmedia/cenc/h264/tears/tears_uhd.mpd",  // Stream URL
    "https://proxy.staging.widevine.com/proxy",                              // Widevine key server URI
    HISPlayerProperties(
        true,                                                               // Autoplay (Boolean)
        HISPlaybackStrategy.LOOP                                            // PlaybackStrategy
    )
)
hisPlayerManager.addStream(playerId, stream)
```

The API takes the playerId as their first parameter. If an invalid Id is passed, the method will throw an error.
Once the stream is created, you can use the playback control functions provided by the API, such as `hisPlayerManager.play(playerId)` or `hisPlayerManager.pause(playerId)`.
