# FishEye

## How to Play FishEye Video
To play FishEye Video, use `HISPlayerVideoShapeTypes.FishEye180` or `HISPlayerVideoShapeTypes.FishEye360` for `shapeType` parameter of `addStreamWithEntity` function.

Each FishEye video has different camera calibration, so user should set `fishEyeFOV` parameter correctly to watch video correctly when you call `addStreamWithEntity` function.

If video shows whole circle, then use `fishEyeFOV` value as 1.0. If video is cropped then use smaller value.
You can change `fishEyeFOV` value while playing the video with `HISPlayerEntity.setFishEyeFOV()` function.

HISPlayer supports Widevie L3 DRM for FishEye video.
To play FishEye DRM content, you should set key server url.
and drm Token information which will be included HTTP header when request license key.

```
val streamProperty = HISStreamEntityProperties(
        "https://api.hisplayer.com/media/hisplayer/ce77405f-d7c8-4523-95a4-b3715ec57a12/master.m3u8?contentKey=ScrVdlMh",   // Content URL
        "https://drm.license.com,                   // drm Key Server URL
        Pair("Authorization", "Bearer XXXXX"),       // drm Token
        HISPlayerProperties(
            content.autoPlay,
            HISPlaybackStrategy.LOOP,
            0,
            1000000,
        )
    )
```    

