# MV-HEVC Stereoscopic playback

## How to Play MV-HEVC Stereoscopic Video
Internally detect codec type and if it uses MV-HEVC codec then enable stereoscopic and compose output video format as left and right.
To view the video as 3D, use `HISPlayerStereoTypes.LeftRight` for `shapeType` parameter of `addStreamWithEntity` function.


