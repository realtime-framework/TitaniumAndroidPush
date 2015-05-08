# Android Push Notifications with Realtime and GCM
This project shows how to build an Android Titanium app able to receive push notifications. This project uses the Realtime Android module and Push Notifications from (GCM).

## Realtime + Android push notifications guide

- Create a google project, more info [here](http://messaging-public.realtime.co/documentation/starting-guide/mobilePushGCM.html).

- Set notification title to be displayed in the notification manager.

		ortc.setNotificationTitle('[App name for example]');


- Before connect set your Google project id: 

		ortc.setGoogleProjectId('462540995476');
		ortc.clusterUrl = 'http://ortc-developers.realtime.co/server/2.1';
		if(taAuthToken.value != '') {
			ortc.connect(taAppKey.value, taAuthToken.value);
		} else {
			ortc.connect(taAppKey.value);
		}
		
- Set onNotification eventListener. 

		ortc.addEventListener('onNotification', function(e) {
			addRowToEvents('(onNotification Channel: '+e.channel+') Message received: '+e.message+' Payload received: '+e.payload);
			Titanium.API.log('(onNotification Channel: '+e.channel+') Message received: '+e.message);
		});
		
- Use subscribeWithNotifications to subscribe the channel.
		
		ortc.subscribeWithNotifications(taChannel.value, true);
		
- Add the folling entries to your application TiApp.xml, replace **[Titanium project identifier]** :

	    <android xmlns:android="http://schemas.android.com/apk/res/android">
	        <manifest>
	            <permission
	                android:name="[Titanium project identifier].permission.C2D_MESSAGE" android:protectionLevel="signature"/>
	            <uses-permission android:name="[Titanium project identifier].permission.C2D_MESSAGE"/>
	            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
	            <uses-permission
	                android:name="android.permission.GET_TASKS"/> //detect if app is running from service
	            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE"/>
	            <uses-permission android:name="android.permission.WAKE_LOCK"/>
	            <application android:debuggable="true">
	                <service android:name="co.realtime.ortc.BackgroundService"/>
	                <service android:name="ibt.ortc.extensibility.GcmOrtcIntentService"/>
	                <receiver
	                    android:name="ibt.ortc.extensibility.GcmOrtcBroadcastReceiver" android:permission="com.google.android.c2dm.permission.SEND">
	                    <intent-filter>
	                        <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
	                        <action android:name="android.intent.action.BOOT_COMPLETED"/>
	                        <category android:name="[Titanium project identifier]"/>
	                    </intent-filter>
	                    <intent-filter>
	                        <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
	                        <category android:name="[Titanium project identifier]"/>
	                        <action android:name="android.intent.action.BOOT_COMPLETED"/>
	                    </intent-filter>
	                </receiver>
	            </application>
	        </manifest>
	    </android>

