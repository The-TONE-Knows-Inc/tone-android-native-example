# Tone Framework Android Documentation 
This repository contains an example of framework usage in android for Kotlin.
## Importing Library into Project  

Add Maven Jitpack.io

In root build.gradle 
```css
	allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
```
or settings.gradle
```css
dependencyResolutionManagement {  
  repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)  
	    repositories {  
			google()  
	        	mavenCentral()  
	        	maven { url 'https://jitpack.io' }
		}
 }
```
in app module build.gradle
Latest version on Jitpack: [![](https://jitpack.io/v/The-TONE-Knows-Inc/framework-core-tone-android.svg)](https://jitpack.io/#The-TONE-Knows-Inc/framework-core-tone-android)
```css
dependencies {
	        implementation 'com.github.The-TONE-Knows-Inc:framework-core-tone-android:v0.0.1'
	}
```
in App's AndroidManifest.xml add:
```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>  
<uses-permission android:name="android.permission.INTERNET" />  
<uses-permission android:name="android.permission.RECORD_AUDIO" />  
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />  
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<!-- replace icon and theme to avoid manifest merger issues-->
<application tools:replace="android:icon,android:theme">
	<!-- Declare Service-->
	<service  
	  android:name="com.strutbebetter.tonelisten.core.ToneServiceManager"  
	  android:enabled="true"  
	  android:exported="true"  
	  android:usesCleartextTraffic="true"  
	  android:foregroundServiceType="location|microphone"  
	  android:process=":ToneListeningService">  
	</service>
</application>
```

##  MainActivity
Import
```java
	import com.strutbebetter.tonelisten.ToneFramework
	import com.strutbebetter.tonelisten.core.ToneUIEventListener  
	import com.strutbebetter.tonelisten.models.ToneModel
```
outside onCreate:

Kotlin
```kotlin
var toneFramework: ToneFramework? = null
```
Java
```java
ToneFramework toneFramework;
```
inside onCreate 

Kotlin
```kotlin
//Instantiate ToneFramework Singleton with api key and context
toneFramework = ToneFramework("apiKeyDebug", this@MainActivity) 
//Handle Intent is for executing actions that come from a notification
toneFramework!!.handleIntent(intent)  
// Asks for the required permissions to run 
toneFramework!!.checkPermission<MainActivity>(ToneFramework.TONE_PERMISSION_CODE, this@MainActivity)  
// Start the service if permissions were given
toneFramework!!.start()  
// Finish the service
toneFramework!!.finish()
```
Java
```java
// Create instance with @apiKey and @Context
toneFramework = new  ToneFramework("apiKeyDebug", MainActivity.this);
// handleIntent is for executing actions that come from a notification @Intent intent
toneFramework.handleIntent(getIntent());
// Asks for the required permissions to run @String PermissionCode to handle activityResult and Context context
toneFramework.checkPermission(ToneFramework.TONE_PERMISSION_CODE, MainActivity.this);
// Start the service if permissions were given
toneFramework.start();
// Finish the service
toneFramework.finish();

```
To receive ToneTag detection on the UI implement 
ToneUIEventListener on Main Activity  and override onToneReceived method

Kotlin:
```kotlin
class MainActivity : AppCompatActivity(), ToneUIEventListener {
	//Handle tones received yourself or let the framework handle the Tone Automatically
	override fun onToneReceived(toneModel: ToneModel?) {  
	    toneFramework!!.handleToneResponse(toneModel, this@MainActivity);  
	}
}
```
Java
```java
public  class  MainActivity  extends  AppCompatActivity  implements  ToneUIEventListener {
	@Override
	public  void  onToneReceived(ToneModel  toneModel) {
		// Handle tone automatically or handle it yourself
		toneFramework.handleToneResponse(toneModel, mActivity);
	}
}
```
ToneModel.class
```java
class  ToneModel {
	String  actionType; // Action to be performed
	String  actionUrl; // Data needed to perform the action
	String  body; // Optional data to perform the action
}
```
