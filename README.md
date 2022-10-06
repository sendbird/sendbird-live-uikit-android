
# Sendbird Live UIKit for Android

[![Platform](https://img.shields.io/badge/platform-android-orange.svg)](https://github.com/sendbird/sendbird-live-uikit-android)
[![Languages](https://img.shields.io/badge/language-kotlin-orange.svg)](https://github.com/sendbird/sendbird-live-uikit-android)
[![Latest version of 'sendbird-live-uikit' @ Cloudsmith](https://api-prd.cloudsmith.io/v1/badges/version/sendbird/release/maven/sendbird-live-uikit/latest/a=noarch;xg=com.sendbird.sdk/?render=true&show_latest=true)](https://cloudsmith.io/~sendbird/repos/release/packages/detail/maven/sendbird-live-uikit/latest/a=noarch;xg=com.sendbird.sdk/)
[![Commercial License](https://img.shields.io/badge/license-Commercial-brightgreen.svg)](https://github.com/sendbird/sendbird-live-uikit-android/blob/main/LICENSE.md)

We are introducing a UIKit for Sendbird Live SDK. This UIKit includes pre-built UI components specifically for live streaming features provided by the Sendbird Live SDK. Live UIKit is built on top of Sendbird Chat UIKit. 

## Table of contents
Requirements
Before you start
Getting started
Implementation Guide


# Requirements

The minimum requirements for UiKit for Android are:
 
* `Android 5.0 (API level 21) or higher`
* `Java 8 or higher`
* `Support androidx only`
* `Android Gradle plugin 4.0.1 or higher`
* `Sendbird Chat SDK for Android 4.0.3 and later`
* `Sendbird Live SDK for Android 1.0.0-beta2 and later`

—

---

## Before you start

Sendbird Live UIKit is an add-on to Sendbird Live SDK which provides live streaming feature and uses open channels from Sendbird Chat SDK for chat. Installing the Sendbird Live UIKit will automatically install the Live SDK and the Chat SDK.

Before installing the Live UIKit, create a Sendbird account to acquire an application ID which you will need to initialize the Live UIKit. Go to [Sendbird Dashboard](https://dashboard.sendbird.com/) and create an application by selecting **Calls+Live** in product type. Once you have created an application, go to **Overview** and you will see the **Application ID**.

---

## Get Started

You can start building your first live event by installing the Live UIKit. When you install the Live UIKit, Sendbird Live SDK will be installed implicitly.

### **Step 1** Create a project

To get started, open `Android Studio` and create a new project for the Live UIKit in the **Project window** as follows:

1. Click **Start a new Android Studio project** in the **Welcome to Android Studio** window.

2. Select **Empty Activity** in the **Select a Project Template** window and click **Next**.

3. Enter your project name in the **Name** field in the **Configure your project** window.

4. Select your language as either **Java** or **Kotlin** from the **Language** drop-down menu.

5. Make sure **Use legacy android.support.libraries** is unchecked.

6. Select minimum API level as 21 or higher.

![Image|Setting up your project in the Create new project dialog.](https://static.sendbird.com/docs/uikit-android-introduction-send-first-message.png)

---

### **Step 2** Install the Live UIKit

You can install the Live UIKit for Android through [`Gradle`](https://gradle.org/). If you're using Gradle 6.7 or lower, add the following code to your **root** `build.gradle` file.

<div component="AdvancedCode" title="build.gradle">

```gradle
allprojects {
	repositories {
        maven { url "https://jitpack.io" }

		maven { url "https://repo.sendbird.com/public/maven" }
	}
}
```

</div>

> __Note__: Make sure the above code block isn't added to your module `build.gradle` file.

If using Gradle 6.8 or higher, add the following to your `settings.gradle` file.

<div component="AdvancedCode" title="settings.gradle">

```gradle
dependencyResolutionManagement {
    repositories {
        maven { url "https://jitpack.io" }
        maven { url "https://repo.sendbird.com/public/maven" }
    }
}
```

</div>

>__Note__: To learn more about updates to Gradle, visit [this page](https://docs.gradle.org/6.8/release-notes.html#dm-features).

Next, for all Gradle versions, open the `build.gradle` file at the application level. For both `Java` and `Kotlin`, add the following code block and dependencies.

<div component="AdvancedCode" title="build.gradle">

```gradle
apply plugin: 'com.android.application'

android {
    buildFeatures {
        viewBinding true
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8    // Make sure you have JavaVersion 1.8.
        targetCompatibility JavaVersion.VERSION_1_8    // Make sure you have JavaVersion 1.8.
    }
}

dependencies {
    implementation 'com.sendbird.sdk:live-uikit:1.0.0-beta.1'
}
```

</div>

Before saving the `build.gradle` file, check if you’ve enabled `viewBinding`. Then, click the **Sync** button to apply all changes.

> __Note__: If you have already been using Sendbird Chat or want to know the minimum version of the Chat SDK to use the Live UIKit, you can check the information in [Sendbird Live SDK](https://github.com/sendbird/sendbird-live-sdk-android).

### **Step 3** Request permission to access camera and microphone

Your users need to grant the client app the permission to access camera and microphone to stream media. They also need to allow the access to the photo library to send and download images and videos.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.sendbird.liveuikitapplication">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />

    <!--  To detect bluetooth audio device  -->
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />

    <!--  To utilize audio in livestreaming event  -->
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-feature android:name="android.hardware.camera2.full" />
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />

    <!--  To utilize video in livestreaming event for audio  -->
    <uses-permission android:name="android.permission.CALL_PHONE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.AUDIO_SESSION_ID_GENERATE" />

    ...
</manifest>
```

### **Step 4** Initialize the SendbirdLiveUIKit instance

To integrate the UIKit in the client app, you need to initialize it first. You can initialize `SendbirdLiveUIKit` instance by passing the `SendbirdUIKitAdapter` instance as an argument to a parameter in the `SendbirdLiveUIKit.init()` method. The `SendbirdLiveUIKit.init()` must be called once in the `onCreate()` method of your app’s authentication [`Activity`](https://developer.android.com/reference/android/app/Activity) instance.

<div component="AdvancedCode" title="Kotlin">

```kotlin
package com.example.liveuikitapplication;

import androidx.appcompat.app.AppCompatActivity
import com.sendbird.android.exception.SendbirdException
import com.sendbird.android.handler.InitResultHandler
import com.sendbird.live.uikit.SendbirdLiveUIKit
import com.sendbird.live.uikit.internal.extensions.changeValue
import com.sendbird.uikit.adapter.SendbirdUIKitAdapter
import com.sendbird.uikit.interfaces.UserInfo

class AuthenticationActivity : AppCompatActivity() {
    ...

    override fun onCreate() {
        super.onCreate()
        val adapter = object : SendbirdUIKitAdapter {
            override fun getAppId(): String {
                return YOUR_APP_ID // Specify your Sendbird application ID.
            }

            override fun getAccessToken(): String? {
                return ""
            }

            override fun getUserInfo(): UserInfo {
                return object : UserInfo {
                    override fun getUserId(): String {
                        return USER_ID // Specify your user ID.
                    }

                    override fun getNickname(): String? {
                        return USER_NICKNAME // Specify your user nickname.
                    }

                    override fun getProfileUrl(): String? {
                        return ""
                    }
                }
            }

            override fun getInitResultHandler(): InitResultHandler {
                return object : InitResultHandler {
                    override fun onMigrationStarted() {
                        // DB migration has started.
                    }

                    override fun onInitFailed(e: SendbirdException?) {
                        // If DB migration fails, this method is called.
                    }

                    override fun onInitFailed(e: SendbirdException) {
                        // If DB migration fails, this method is called.
                    }

                    override fun onInitSucceed() {
                        // If DB migration is successful, this method is called and you can proceed to the next step.
                        // In the sample app, the LiveData class notifies you on the initialization progress
                        // and observes the `MutableLiveData<InitState> initState` value in `SplashActivity()`.
                        // If successful, the `LoginActivity` screen
                        // or the `HomeActivity` screen will show.
                    }
                }
            }

        }
    }
}
```

</div>

### **Step 5** Add BaseApplication

Add the created `BaseApplication` to `AndroidManifest.xml`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.sendbird.liveuikitapplication">

    <!--  sets android:name  -->
    <application
        android:name=".BaseApplication"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                ...
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

### **Step 6** Display a live event list

Launch the Live UIKit in the client app with `LiveEventListActivity`, which serves as the starting point for using the Live UIKit to create, enter, and list live events. Implement the code below to start the Live UIKit wherever you would like in the client app.

<div component="AdvancedCode" title="Kotlin">

```kotlin
package com.example.liveuikitapplication;

import com.sendbird.live.uikit.activities.LiveEventListActivity

public class MainActivity extends LiveEventListActivity() {
    // Add this line.
    // If you’re going to inherit LiveEventListActivity, don’t implement setContentView() in the activity.
}
```

</div>

### **Step 7** Start your first live

You can now run the app on a simulator or a plugged-in device. To start a live event, you must first create a live event by tapping the button in the top-right corner. Add a title, a cover image and users who can be host or default values will show. Once you have created a live event, tap on the live event to enter. You can choose to enter as host or participant and other users can enter the live event as participants.

You can also enter a live event manually as shown below.

<div component="AdvancedCode" title="Kotlin">

```kotlin

liveEvent.enterAsHost(MediaOptions(videoDevice = VIDEO_DEVICE, audioDevice = AUDIO_DEVICE.value)) {
    if (it != null) {
        return@enterAsHost
    }
}
```

</div>

<video src="https://static.sendbird.com/docs/live-v1-android-start-your-first-live-enter.mp4" controls autoplay loop></video>

You've successfully started your first live event with Sendbird Live.

