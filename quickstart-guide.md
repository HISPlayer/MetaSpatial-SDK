# Quickstart Guide
Through this guide, you will be introduced to the basic steps for setting up the playback.

## 1. Import Package
First, extract the SDK from the .zip file, copy the **hisplayer-sdk-version.aar** file, and paste it into the **~/ProjectName/ModuleName/libs/** directory in your project. If that directory doesn’t exist, create one.

<p align="center">
<img src="./images/libs-folder.jpg" style="width: 350px; height: auto;">
</p>

Then, add the following imports inside the module dependencies, in the build.gradle.kts file

```
  // HISPlayer Dependencies
  implementation(files("libs/hisplayer-sdk-1.0.0.aar"))
  implementation("androidx.media3:media3-exoplayer:1.6.1")
  implementation("androidx.media3:media3-exoplayer-hls:1.6.1")
  implementation("androidx.media3:media3-exoplayer-dash:1.6.1")
  implementation("androidx.media3:media3-ui:1.6.1")
  implementation("org.bouncycastle:bcprov-jdk16:1.45")
```

Finally, **Sync Project with Gradle Files** `Ctrl + Shift + O`

## 2. Configure HISPlayer
All public API classes of HISPlayer are located in the `com.hisplayer.sdk` package.
You can import individual classes as needed, but for simplicity in this guide, we’ll import all of them using:
```
import com.hisplayer.sdk.*
```



