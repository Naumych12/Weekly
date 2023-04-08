# Creating Cordova App:


## Installing Android SDK
#### You can do it installing Android Studio or SDK commandlinetool. You can download it by the same link. I used Android Studio

1. [Download Android Studio](https://developer.android.com/studio#command-tools)
2. After installing open Android Studio
3. Click the "More Actions" button (looks like three dots) > SDK Manager
4. Choose SDK to install. You can install the latest
5. Click "Apply". You check see installation path
6. After installing you should to set envinment variable
7. Go to Control Panel > System > Advanced system settings > Environment Variables
8. Under system variables table click "New"
9. Set name 
>ANDROID_HOME 

and path 
>C:\Users\username\AppData\Local\Android\Sdk

or you installation path if it is different and save

10. Add another one with name "ANDROID_SDK_ROOT" and path "C:\Users\username\AppData\Local\Android\Sdk"


## Installing JDK
#### I installed JDK 8, but you can try JKD 11

1. [Download JDK](https://www.oracle.com/java/technologies/downloads/#java8)
2. Set new environment variable with name 
>JAVA_HOME 

and path 
>C:\Program Files\Java\jdk1.8.0_361 
 
or your own JDK path
3. Add to existing variable with name 
>Path 

JDK path with bin directory 
>C:\Program Files\Java\jdk1.8.0_361\bin


## Installing Gradle

1. [Download Gradle of a very specific version that cordova requires](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#setting-environment-variables)
2. Extract .zip and place folder where you want
3. Add to existing variable with name "Path" path to your gradle bin directory like this "C:\Program Files\gradle-4.10.3\bin"


## Creating Cordova App

1. Install Node.js
2. In vue.js project root folder open console
3. Install cordova cli

```bash
npm install -g cordova
```

4. Create cordova project

```bash
cordova create <project-name>
```
or
```bash
cordova create <project-name> <package-name> <app-name>
```

5. Go to cordova project root dir

```bash
cd <project-name>
```

6. Add Android to your project

```bash
cordova platform add android
```

7. To add plugins you can run

```bash
cordova plugin add <plugin-name>
```

## Initializing Cordova App fetched from remote repository

1. Ensure that in Cordova app root folder there are "www" dir, "config.xml" and "package.json" files
2. Add Android to your project

```bash
cordova platform add android
```

## Building apk

1. Build you vue.js app with

```bash
npm run build
```

2. Move all files from
>dist/spa

to
><cordova-app-name>/www
3. In cordova root folder run

```bash
cordova build
```

4. File will be available by path 
>platforms\android\app\build\outputs\apk\debug\app-debug.apk


## Building aab file for release

1. Build you vue.js app with

```bash
npm run build
```

2. Move all files from 
>dist/spa
 
to
><cordova-app-name>/www
3. In cordova root folder run

```bash
cordova build android --release
```

4. File will be available by path 
>platforms\android\app\build\outputs\bundle\release\app-release.aab
