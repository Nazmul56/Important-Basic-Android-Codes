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
#Map
```java
  Map map = new HashMap();

    map.put("name", "John");
    map.put("time", "9648512236521");
    map.put("age", "25");

    long time = Long.valueOf((String)map.get("time")).longValue() ;
    int age = Integer.valueOf((String)  map.get("aget")).intValue();
    System.out.println(time);
    System.out.println(age);
    ```
#EditText
```java 
 EditText Member_id = (EditText)findViewById(R.id.member_id_etx);
 Member_id.getText().toString()
 ```
#Toast
```java
 Toast.makeText(getApplicationContext(), "Hello World", Toast.LENGTH_SHORT).show();
```
#Pass Argument between different activitys
####Sender Activity
```java
String stringExtra = "Some string you want to pass";

Intent intent = new Intent(this, AndroidTabRestaurantDescSearchListView.class);

//include the string in your intent
intent.putExtra("string", stringExtra);

startActivity(intent)
```
####Recever Activity
```java
//fetch the string  from the intent
String extraFromAct1 = getIntent().getStringExtra("string");

Intent intent = new Intent(this, RatingDescriptionSearchActivity.class);

//attach same string and send it with the intent
intent.putExtra("string", extraFromAct1);
startActivity(intent);
```


#Pause For 1 Second
```java
try {


                Thread.sleep(2000);
                // i=1000;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
```
#Different String Format
```java
   tvDay.setText("" + String.format("%02d", days));
   tvHour.setText("" + String.format("%02d", hours));
   tvMinute.setText("" + String.format("%02d", minutes));
   tvSecond.setText("" + String.format("%02d", seconds));
 ```
#System Curent Date and Time 
```java
   TextView tvDisplayDate = (TextView) findViewbyId(R.id.datetv);
   long date = System.currentTimeMillis(); 

   SimpleDateFormat sdf = new SimpleDateFormat("MMM MM dd, yyyy h:mm a");
   String dateString = sdf.format(date);   
   tvDisplayDate.setText(dateString);
```
```java
Mon Jan 5, 2009 4:55 PM
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
