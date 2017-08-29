Thread
======
RUN on UI Thread
----------------
```java
    runOnUiThread(new Runnable() {
                    @Override
                    public void run() {

                        if (!isFinishing()){
                            new AlertDialog.Builder(getApplicationContext())
                                    .setTitle("Your Alert")
                                    .setMessage("Your Message")
                                    .setCancelable(false)
                                    .setPositiveButton("ok", new DialogInterface.OnClickListener() {
                                        @Override
                                        public void onClick(DialogInterface dialog, int which) {
                                            // Whatever...
                                        }
                                    }).show();
                        }
                    }
                });
```
