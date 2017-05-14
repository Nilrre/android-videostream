# Android-Simple-MjpegViewer
[![](https://jitpack.io/v/dydwo92/Android-Simple-MjpegViewer.svg)](https://jitpack.io/#dydwo92/Android-Simple-MjpegViewer)

# Installation
**Step1.** Add it in your root build.gradle at the end of repositories

```gradle
allprojects {
    repositories {
        maven { url "https://jitpack.io" }
    }
}
```

**Step2.** Add the dependency in your module build.gradle


```gradle
dependencies {
    compile 'com.github.dydwo92:Android-Simple-MjpegViewer:0.0'
}
```

**Step 3.** Build your project through **`Build > Make Project`**. And then, sync your project through **`Tools > Android > Sync Project with Gradle Files`**

**Step 4.** In **AndroidManifest.xml** in your app, add the following line to access internet.

```xml
<manifest ...>
    <uses-permission android:name="android.permission.INTERNET" />
```

Also, add the following tag. It will prevent video reloading when screen rotating.

```xml
<activity
    ...
    android:configChanges="orientation|screenSize"
>
```

# Usage
**Add MjpegViewer component.** In the layout xml, put the MjpegView tag.
```xml
    <MjpegViewer.MjpegView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/videwView" />
```

**Start video streaming.** In the activity class, start the video streaming.
```java
public class MainActivity extends AppCompatActivity {
    MjpegView mv;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mv = (MjpegView) findViewById(R.id.videwView);
        
        // By the time you do this, the mjpeg stream url below may not work..
        mv.Start("http://webcam.st-malo.com/axis-cgi/mjpg/video.cgi?resolution=352x288");
    }
}
```
<img src="http://i.imgur.com/8FXyqLZ.jpg" width="300px" />

# Methods
There are only three methods to control the MjpegView component.

### 1. Start
```java
mv.Start(String url);
mv.Start(String url, Handler parent_handler);
```
You can start the video streaming simply with **Start()** method. You can see there are two Start() methods above. If you want to handle events, insert the handle object in the Start() method. Handling events is described below(**[Event Handling](#event-handling)**).

### 2. Stop
```java
mv.Stop();
```
You can stop the video streaming.

### 3. SetDisplayMode
```java
mv.SetDisplayMode(mv.SIZE_FIT);
mv.SetDisplayMode(mv.SIZE_FULL);
```
When you set the display mode to *SIZE_FIT*, MjpegViewer will display the video to fit the screen properly. If you set the display mode to *SIZE_FULL*, MjpegViewer will crop the video appropriately and display it full on the screen.

![](http://i.imgur.com/ZwqBjeP.jpg)


# Authentication

Another Start method is available to authenticate the HTTP connection to the video stream
```
mv.Start(String url, String username, String password);
```


# Event Handling
```java
public class MainActivity extends AppCompatActivity {
    MjpegView mv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mv = (MjpegView) findViewById(R.id.videwView);
        mv.Start("http://webcam.st-malo.com/axis-cgi/mjpg/video.cgi?resolution=352x288", MjpegViewHandler);
    }

    final Handler MjpegViewHandler = new Handler(){
        @Override
        public void handleMessage(Message msg){
            Log.d("State : ", msg.obj.toString());
            
            switch (msg.obj.toString()){
                case "DISCONNECTED" :
                    // TODO : When video stream disconnected
                    break;
                case "CONNECTION_PROGRESS" :
                    // TODO : When connection progress
                    break;
                case "CONNECTED" :
                    // TODO : When video streaming connected
                    break;
                case "CONNECTION_ERROR" :
                    // TODO : When connection error
                    break;
                case "STOPPING_PROGRESS" :
                    // TODO : When MjpegViewer is in stopping progress
                    break;
            }
            
        }
    };

}
```
