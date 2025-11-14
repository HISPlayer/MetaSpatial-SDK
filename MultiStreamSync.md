# Multistream Synchronization

## How to Play Multiple Streams and Synchronize Each Other.
HISPlayer supports multiple streams and each stream can synchronize with another stream.

You can add multiple streams by calling `addStream` or `addStreamWithEntity` api multiple times.
And if you want to synchronize one stream to another stream you can set `setStreamSynchronization(int rootId, int followerId)`.
Stream with followerId will be synched to follow the rootId stream.
Multiple streams can be synchronized to follow one root stream.
If streams are synchronized to follow the root stream, then they will play the same play positions, pause and seek process will follow the root stream.

If root stream is VOD content then all follower streams should be VOD too.
If root stream is Live content ,then all follower streams should be Live too.

For Live streams, playing time will be synchronized by Unix epoch time.

If you want to clear synchronization, you can call `unsetStreamSynchronization(int rootId, int followerId)`.

This is the sample 

```
// Create Root Stream
val streamProperty = HISStreamEntityProperties(
    "https://test.com/rootContent.m3u8",   // Content URL
    HISPlayerProperties(
        content.autoPlay,
        HISPlaybackStrategy.LOOP,
        0,
        1000000,
    )
)
val playerEntity = hisPlayer?.addStreamWithEntity(
    1,      // Root Player Id
    streamProperty,
    shapeType,
    stereoMode,
    size,
    position,
    rotation,
    true
)

// Create Follower Stream
val streamProperty01 = HISStreamEntityProperties(
    "https://test.com/followerContent.m3u8",   // Content URL
    HISPlayerProperties(
        content.autoPlay,
        HISPlaybackStrategy.LOOP,
        0,
        1000000,
    )
)
val playerEntity01 = hisPlayer?.addStreamWithEntity(
    2,      // Follower Player Id
    streamProperty01,
    shapeType,
    stereoMode,
    size,
    position,
    rotation,
    true
)

// Set Synchronization
hisPlayer?.setStreamSynchronization(
    1,  // Root Player Id
    2   // Follower Player Id
)

// Unset Synchronization
hisPlayer?.unsetStreamSynchronization(
    1,  // Root Player Id
    2   // Follower Player Id
)
```    

## Live Stream Seek
Each live stream has different live window(playable live content segment).
Commonly seekable range is the same as live window range.
But if multiple streams are synchronized then seekable range will be affected by each stream's live window.
So our SDK provide an API to retrieve seekable range via `getSeekableRange(int playerId)`.

When you call seek you should set seek point within seekable range. If you call outside of seekable range then adjust seek point internally. So seek result can be different from requested point.

And current play position is out of seekable range then we recommand to seek within seekable range.

This is the sample code included at our sample app.

```
  fun updateState() {
    _isBuffering.value = hisPlayer.isBuffering(playerId)
    _isLive.value = hisPlayer.isLive(playerId)
    _currPosition.value = hisPlayer.getVideoPosition(playerId) * 0.001f
    _duration.value = hisPlayer.getVideoDuration(playerId) * 0.001f
    val seekableRange = hisPlayer.getSeekableRange(playerId)

    /**
     * Adjust current play position.
     * If current position is out of seekable range then move into seekable range.
     */
    if (seekableRange != null) {
      if (_currPosition.value * 1000f < seekableRange.first!!) hisPlayer.seek(playerId, seekableRange.first!!)
      else if (_currPosition.value * 1000f > seekableRange.second!!) hisPlayer.seek(playerId, seekableRange.second!!)
    }
  }
```

