#### Important-Basic-Android-Codes
---------------------------------------
#SharedPreference Demmo For First time visit activity
<pre>
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
</pre>

##Links
http://stackoverflow.com/questions/21980324/how-to-display-only-one-time-login-and-then-after-start-application-directly-in

http://upadhyayjiteshandroid.blogspot.in/2013/01/android-working-with-shared-preferences.html
