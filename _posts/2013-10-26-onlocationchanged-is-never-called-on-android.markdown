---
layout: post
status: publish
published: true
title: onLocationChanged is never called on Android
author: Julien Lengrand-Lambert
author_login: jlengrand
author_email: julien@lengrand.fr
author_url: http://www.lengrand.fr
wordpress_id: 892
wordpress_url: http://www.lengrand.fr/?p=892
date: 2013-10-26 16:28:40.000000000 +02:00
categories:
- Uncategorized
- programming
tags:
- android
- programming
- brestram
- java
- gps
comments:
- id: 21831
  author: Robert
  author_email: robertbobrek@gmail.com
  author_url: ''
  date: !binary |-
    MjAxMy0xMi0xMiAyMTo0NzozNyArMDEwMA==
  date_gmt: !binary |-
    MjAxMy0xMi0xMiAyMDo0NzozNyArMDEwMA==
  content: ! "Hi,\r\n\r\nthanks for the code but it is not working...\r\n\tprivate
    LocationInfo li;\r\n\r\ngives me: \"LocationInfo can not be resolved to a type\""
- id: 21886
  author: Julien Lengrand-Lambert
  author_email: julien@lengrand.fr
  author_url: http://www.lengrand.fr
  date: !binary |-
    MjAxMy0xMi0xNCAxMDo0OToyOSArMDEwMA==
  date_gmt: !binary |-
    MjAxMy0xMi0xNCAwOTo0OToyOSArMDEwMA==
  content: ! "Hey, \r\n\r\nYou are totally right, sorry for that.\r\nLocationInfo
    is a class I use internally. I removed the references to LocationInfo, so things
    should be ok now :).\r\n\r\nThanks for noticing!"
---
I had problems with this while developing #<strong><a title="brestram play store page" href="https://play.google.com/store/apps/details?id=fr.lengrand.brestram" target="_blank">BresTram</a></strong>.

I was developing a new feature, allowing my users to find bus stops nearby using their GPS location.

But whatever I was trying, my location was never set, and <a title="java doc onlocationchanged" href="http://developer.android.com/reference/com/google/android/gms/location/LocationListener.html#onLocationChanged(android.location.Location)" target="_blank">onLocationChanged</a> was never called.

Here is what my Activity would look like :

[java]

public class DisplayGPSInfoActivity extends BaseActivity implements LocationListener {
private static final String TAG = &quot;DisplayGPSInfoActivity&quot;;

private ViewFlipper vf;

private LocationManager locationManager;
private String provider;

@SuppressLint(&quot;NewApi&quot;)
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);

// Initiating ViewFlipper
setContentView(R.layout.activity_display_gpsinfo_request);
vf = (ViewFlipper) findViewById(R.id.viewFlipper);

this.locationManager = (LocationManager) getSystemService(LOCATION_SERVICE);

//Choosing the best criteria depending on what is available.
Criteria criteria = new Criteria();
provider = locationManager.getBestProvider(criteria, false);
//provider = LocationManager.GPS_PROVIDER; // We want to use the GPS

// Initialize the location fields
Location location = locationManager.getLastKnownLocation(provider);
}

@Override
public void onLocationChanged(Location location) {
Log.d(TAG, &quot;GPS LocationChanged&quot;);
double lat = location.getLatitude();
double lng = location.getLongitude();
Log.d(TAG, &quot;Received GPS request for &quot; + String.valueOf(lat) + &quot;,&quot; + String.valueOf(lng) + &quot; , ready to rumble!&quot;);

// Do clever stuff here
}
}

[/java]

You can forget about the <a title="viewFlipper javadoc" href="http://developer.android.com/reference/android/widget/ViewFlipper.html" target="_blank">ViewFlipper</a>, that is here only to show something to the user.
Basically, I am letting android decide which provider he wants to use (GPS or Network), and request for the last known location.
Then, I want to do something clever each time onLocationChanged is called.

Problem is, it is not. never. Ever. . .

After having verified hundred times that I had

[xml]
&lt;uses-permission android:name=&quot;android.permission.ACCESS_COARSE_LOCATION&quot; /&gt;
&lt;uses-permission android:name=&quot;android.permission.ACCESS_FINE_LOCATION&quot; /&gt;

[/xml]

correctly defined in my manifest, and that yes, other GPS based apps were working fine on my phone; I finally found the solution.

As stupid as it seems,<strong> I had forgotten to request the updates. . .</strong>
Something like that would do the job :

[java]

@Override
 protected void onResume() {
super.onResume();
Log.v(TAG, &quot;Resuming&quot;);
locationManager.requestLocationUpdates(provider, 400, 1, this);
 }

[/java]

I was somehow expecting that is was automatic, implied by the fact that my activity implements <a title="Location listener javadoc" href="http://developer.android.com/reference/android/location/LocationListener.html" target="_blank">LocationListener</a>.

<strong>Well it is not.</strong>
So, if any of you has the same problem, look whether you actually ask for something before getting angry because you don't receive it :D.

<strong>Have fun hacking around.</strong>
Oh, and if you leave in Brest, give a shot to #<strong><a title="Brestram play store" href="https://play.google.com/store/apps/details?id=fr.lengrand.brestram" target="_blank">BresTram</a></strong>; it is awesome!