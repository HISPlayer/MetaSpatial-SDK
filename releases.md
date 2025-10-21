# HISPlayer Meta Spatial SDK Release Notes

### Version 1.1.0
##### October 21, 2025
- [Improvement] Changed playerIndex with playerId for multistreams.
- [Added] getVolume (int playerId): Return the audio volume of the video.
- [Added] isPlaying(int playerId): Returns whether the video is playing.
- [Added] isBuffering(int playerId): Returns whether the video is buffering.
- [Added] isLive(int playerId): Returns whether the video stream is live.
- [Added] getSpeedRate(int playerId): Returns the speed rate of the video stream.
- [Added] setSpeedRate(int playerId, float speed): Sets the playback speed rate for the specified stream.
- [Added] getVideoTracks(int playerId): Provides the list of the video tracks of the current video playback.
- [Added] getVideoTrackCount(int playerId): Get the number of tracks of a certain stream.
- [Added] setVideoTrack(int playerId, int trackIndex): Select a certain track of a certain stream to be used as the main track. 
- [Added] enableABR(int playerId): Enables the ABR to change automatically between tracks.
- [Added] disableABR(int playerId): Disables the ABR to prevent the player from changing tracks regardless of bandwidth.
- [Added] setMaxBitrate(int playerId, int bitrate): Set a new maximum bitrate (in bits per second) of a specific track. 
- [Added] setMinBitrate(int playerId, int bitrate): Set a new minimum bitrate (in bits per second) of a specific track. 
- [Added] setStreamSynchronization(int mainId, int followerId): Link the main video stream playback and the follower video stream playback for synchronization. 
- [Added] unsetStreamSynchronization(int mainId, int followerId): Unlink the main video stream playback and the follower video stream playback for synchronization.
- [Added] HISPlayerVideoTrack class

### Version 1.0.0
##### May 21, 2025
- First release of HISPlayer Meta Spatial SDK
