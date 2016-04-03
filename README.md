[![Maven Central](https://img.shields.io/badge/maven%20central-appintro-green.svg)](http://search.maven.org/#browse%7C2137414099)
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-AppIntro-green.svg?style=flat)](https://android-arsenal.com/details/1/1939)
[![Android Gems](http://www.android-gems.com/badge/PaoloRotolo/AppIntro.svg?branch=master)](http://www.android-gems.com/lib/PaoloRotolo/AppIntro)

<p>Sample App:</p>
<a href="https://play.google.com/store/apps/details?id=christmaswallpaper.nazmul"><img alt="Get it on Google Play" src="https://play.google.com/intl/en_us/badges/images/apps/en-play-badge-border.png" width="300" /></a>
#### Important-Basic-Android-Codes
---------------------------------------
#Button
```java

Button sites =  (Button) findViewById(R.id.site);
        sites.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                
            }
        });

```
#Intent 
```java
Intent intent = new Intent(this, MainActivity.class);
        startActivity(intent);
```
or
```java
Intent intent = new Intent("com.the.name");
startActivity(i);
```

#SharedPreference Demmo For First time visit activity
```java
onCreate(){

Boolean flag;

        SharedPreferences pref;
        pref = getSharedPreferences("testapp", MODE_PRIVATE);


        SharedPreferences.Editor editor = pref .edit();

        flag = pref .getBoolean("flag", false);

        if (flag) {
            /**second time activity*/

            Intent intent = new Intent(this, MainActivity.class);
            startActivity(intent);

        }else{
            /**for first time*/
            editor.putBoolean("flag", true);
            editor.commit();
            Intent intent = new Intent(this, SecondLayoutIntro.class);
            startActivity(intent);
        }

 
}
```
#Relise Activity Memories 

```java

 // Here we add Pause mathod to release memory.
    @Override
    protected void onPause() {
        super.onPause();

        finish();//this mathod will release memory of splashActivity .
    }
    
 ```
:+1:

##Links
http://stackoverflow.com/questions/21980324/how-to-display-only-one-time-login-and-then-after-start-application-directly-in

http://upadhyayjiteshandroid.blogspot.in/2013/01/android-working-with-shared-preferences.html
