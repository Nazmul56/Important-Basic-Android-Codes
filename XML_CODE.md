XML CODES
=========
Image Fit to Scteen 
-------------------
```xml
<ImageView
     android:layout_width="match_parent"
     android:layout_height="match_parent"
     xmlns:android="http://schemas.android.com/apk/res/android"
     app:srcCompat="@drawable/background_green_solution"
     android:scaleType="centerCrop">
 </ImageView>
 ```
 Layout Gravity Direction
 -------------------------
 ```xml
 android:layoutDirection="ltr" <!--Full Layout Draw left to Right -->
 
 android:layout_gravity="center"
 android:layout_gravity="center_horizontal"
 android:layout_gravity="center_vertical"
 
 ```
 AlignParent
 -----------
 ```xml
 android:layout_alignParentBottom="true"
 android:layout_alignParentLeft="true"
 ```
Make Center a Button
--------------------
```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal"
        android:gravity="center">
        <Button
            android:id = "@+id/btn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            />
    </LinearLayout>
</LinearLayout>
```
