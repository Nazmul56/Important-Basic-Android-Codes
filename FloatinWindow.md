Floating Window
===============
Add this Permission On mainifest.xml give Draw Over Other Apps Permission from android App's Advance Settings.
mainifest.xml
-------------
```xml
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
```
Floating activity 
FloatingWindow.java
-------------------
```java
public class FloatingWindow extends Service {

    WindowManager wm;
    LinearLayout ll;
    View view;
    GLSurfaceView wsv;



    //private VideoRendererGui.ScalingType scalingType = VideoRendererGui.ScalingType.SCALE_ASPECT_FILL;
    private RendererCommon.ScalingType scalingType = RendererCommon.ScalingType.SCALE_ASPECT_FILL;

    private WebRtcClient client;
    private String mSocketAddress;

    MediaStream remoteStream = null;
    MediaStream localStream;
    @Override
    public IBinder onBind(Intent intent) {
        // TODO Auto-generated method stub
        return null;
    }

    @Override
    public void onCreate() {
        // TODO Auto-generated method stub
        super.onCreate();
        LayoutInflater layoutInflater = (LayoutInflater) getApplicationContext().getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        view = layoutInflater.inflate(R.layout.window, null);
        
       Button  b = (Button) view.findViewById(R.id.bt_stop);

  

// TODO OnTouch Listener
        view.setOnTouchListener(new View.OnTouchListener() {
            LayoutParams updatedParameters = parameters;
            double x;
            double y;
            double pressedX;
            double pressedY;

            @Override
            public boolean onTouch(View v, MotionEvent event) {

                switch (event.getAction()) {
                    case MotionEvent.ACTION_DOWN:

                        x = updatedParameters.x;
                        y = updatedParameters.y;

                        pressedX = event.getRawX();
                        pressedY = event.getRawY();

                        break;

                    case MotionEvent.ACTION_MOVE:
                        updatedParameters.x = (int) (x + (event.getRawX() - pressedX));
                        updatedParameters.y = (int) (y + (event.getRawY() - pressedY));

                        wm.updateViewLayout(view, updatedParameters);

                    default:
                        break;
                }

                return false;
            }
        });


       b.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                wm.removeView(view);
                stopSelf();
                //System.exit(0);
            }
        });
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        stopSelf();
    }
}
```

Layout 
window.xml
----------
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="500dp"
    android:layout_height="500dp"
    android:background="@android:color/white"
    >
    <Button
        android:id="@+id/bt_stop"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Stop" />

</RelativeLayout>
```

Call This Floating Activity From Any Activity

```java
   startService(new Intent(MainActivity.this,FloatingWindow.class));
```

