Meteor Sample Class
==================
* Demo
```java
package bd.mil.army.crs;

import android.app.Dialog;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.view.View;
import android.widget.EditText;
import android.widget.ProgressBar;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;

import bd.mil.army.crs.adapter.LogAdapter;
import bd.mil.army.crs.interfaces.LogUpdater;
import bd.mil.army.crs.model.LogMsg;
import bd.mil.army.crs.utils.C;
import bd.mil.army.crs.utils.D;
import bd.mil.army.crs.utils.Lg;
import bd.mil.army.crs.utils.MeteorHelper;
import bd.mil.army.crs.utils.P;
import bd.mil.army.crs.utils.RetryCallBackForNoInternet;
import bd.mil.army.crs.utils.U;
import im.delight.android.ddp.Meteor;
import im.delight.android.ddp.ResultListener;
import im.delight.android.ddp.SubscribeListener;
import im.delight.android.ddp.db.Collection;
import im.delight.android.ddp.db.Document;

/**
 * Created by touhid on 9/10/2017.
 * bismillah :)
 */

public class LogActivity extends AppCompatActivity {

    private static final String TAG = LogActivity.class.getSimpleName();
    private Meteor meteor;

    private LogAdapter adapter;

    private ProgressBar pbarLoading;
    private String ownGollaId;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_log);

        findViewById(R.id.tvLogTitle).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                finish();
            }
        });
        pbarLoading = findViewById(R.id.pb);
        pbarLoading.setVisibility(View.GONE);

        RecyclerView rv = findViewById(R.id.rvLogs);
        rv.setLayoutManager(new LinearLayoutManager(this));
        adapter = new LogAdapter(this, new ArrayList<LogMsg>(), new LogUpdater() {
            @Override
            public void updateLog(int pos, LogMsg logMsg) {
                logEntryDialog(pos, logMsg);
            }

            @Override
            public void deleteLog(int pos, LogMsg logMsg) {
                D.showToastShort(LogActivity.this, "Deleting log: \n" + logMsg.getMessage());
                deleteLogMsg(pos, logMsg);
            }
        });
        rv.setAdapter(adapter);

        findViewById(R.id.fabAddLog).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                logEntryDialog(-1, null);
            }
        });
        ownGollaId = P.getOwnGollaId(LogActivity.this);
        meteor = MeteorHelper.getInstance(this);
        meteor.addCallback(MeteorHelper.METEOR_CALLBACK_DEF_LOGGER);

        if (!meteor.isConnected()) {
            if (U.isNetConnected(this)) {
                meteor.connect();
            } else if (showItems() && adapter.getItemCount() < 1)
                D.showNoInternetDialog(this, new RetryCallBackForNoInternet() {
                    @Override
                    public void retry() {
                        if (!meteor.isConnected())
                            meteor.connect();
                    }
                });
        } else
            subscribeForLog();
    }

    private void logEntryDialog(final int pos, final LogMsg logMsg) {
        final Dialog d = new Dialog(LogActivity.this, R.style.AppTheme_DialogTheme);
        d.setContentView(R.layout.dialog_log_entry);

        final EditText etFrom = d.findViewById(R.id.etLogFrom);
        final EditText etMsg = d.findViewById(R.id.etLogMsg);
        final EditText etRem = d.findViewById(R.id.etLogRemarks);
        final ProgressBar pb = d.findViewById(R.id.pbSaveLog);
        pb.setVisibility(View.GONE);

        if (logMsg != null) {
            etFrom.setText(logMsg.getFrom());
            etMsg.setText(logMsg.getMessage());
            etRem.setText(logMsg.getRemarks());
        }

        d.findViewById(R.id.btnSaveLog).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if (meteor.isConnected()) {
                    String from = etFrom.getText().toString();
                    String msg = etMsg.getText().toString();
                    String remarks = etRem.getText().toString();
                    if (msg.length() > 1 && from.length() > 1) {
                        if (pos > -1 && logMsg != null)
                            updateLog(pos,
                                    new LogMsg(logMsg.getId(), from, logMsg.getTor(), msg, remarks),
                                    pb, d);
                        else
                            saveLog(from, msg, remarks, pb, d);
                    } else
                        D.showToastLong(LogActivity.this, "Please type in the FROM & MESSAGE fields properly first.");
                } else
                    D.showToastShort(LogActivity.this, "Server not connected");
            }
        });

        D.setDialogThemes(d);
        d.show();
    }

    private void updateLog(final int pos, final LogMsg logMsg, ProgressBar pb, final Dialog d) {
        Map<String, String> m = new HashMap<>();
        m.put("id", logMsg.getId());
        m.put("gollaId", ownGollaId);
        m.put("from", logMsg.getFrom());
        m.put("message", logMsg.getMessage());
        m.put("remarks", logMsg.getRemarks());
        Object[] params = {m};

        pb.setVisibility(View.VISIBLE);
        meteor.call("update.log", params, new ResultListener() {
            @Override
            public void onSuccess(String result) {
                Lg.i(TAG, "onSuccess");
                pbarLoading.setVisibility(View.GONE);
                adapter.getAllItems().set(pos, logMsg);
                adapter.notifyDataSetChanged();
                D.showToastShort(LogActivity.this, "Log Saved");
                showItems();
                d.dismiss();
            }

            @Override
            public void onError(String error, String reason, String details) {
                Lg.e(TAG, "onError : error=" + error + ", reason=" + reason
                        + ", details=" + details);
                pbarLoading.setVisibility(View.GONE);
            }
        });
    }

    private void saveLog(String from, String message, String remarks, ProgressBar pb, final Dialog dialog) {
        Map<String, String> m = new HashMap<>();
        m.put("gollaId", ownGollaId);
        m.put("from", from);
        m.put("message", message);
        m.put("remarks", remarks);
        Object[] params = {m};

        pb.setVisibility(View.VISIBLE);
        meteor.call("add.log", params, new ResultListener() {
            @Override
            public void onSuccess(String result) {
                Lg.i(TAG, "onSuccess");
                pbarLoading.setVisibility(View.GONE);
                // adapter.add(new LogMsg("", ));   <- can't add readily due to lack of ID,
                // which is required if update/delete is tried
                D.showToastShort(LogActivity.this, "Log Saved");
                showItems();
                dialog.dismiss();
            }

            @Override
            public void onError(String error, String reason, String details) {
                Lg.e(TAG, "onError : error=" + error + ", reason=" + reason
                        + ", details=" + details);
                pbarLoading.setVisibility(View.GONE);
            }
        });
    }

    private void deleteLogMsg(final int pos, LogMsg logMsg) {
        if (!meteor.isConnected()) {
            D.showToastShort(this, "Not Connected");
            return;
        }
        pbarLoading.setVisibility(View.VISIBLE);
        meteor.call("remove.log", new String[]{logMsg.getId()}, new ResultListener() {
            @Override
            public void onSuccess(String result) {
                pbarLoading.setVisibility(View.GONE);
                adapter.remove(pos);
                D.showToastShort(LogActivity.this, "Log Deleted");
            }

            @Override
            public void onError(String error, String reason, String details) {
                pbarLoading.setVisibility(View.GONE);
                Lg.e(TAG, "onError : error=" + error + ", reason=" + reason
                        + ", details=" + details);
            }
        });

    }

    private void subscribeForLog() {
        pbarLoading.setVisibility(View.VISIBLE);
        String subscriptionId = meteor.subscribe("log.fetch", new String[]{ownGollaId},
                new SubscribeListener() {
                    @Override
                    public void onSuccess() {
                        Lg.i(TAG, "onSuccess");
                        pbarLoading.setVisibility(View.GONE);
                        showItems();
                    }

                    @Override
                    public void onError(String error, String reason, String details) {
                        Lg.e(TAG, "onError : error=" + error + ", reason=" + reason
                                + ", details=" + details);
                        pbarLoading.setVisibility(View.GONE);
                    }
                });
        Lg.i(TAG, "subscriptionId = " + subscriptionId);
    }

    private boolean showItems() {
        Collection coll = meteor.getDatabase().getCollection(C.COLLECTION_LOG);
        Document[] docs = coll.find();
        ArrayList<LogMsg> logs = LogMsg.parseDocArray(docs);
        adapter.clear();
        adapter.addAll(logs);
        return logs.size() > 0;
    }

    /*private ArrayList<LogMsg> getDummyLogs() {
        ArrayList<LogMsg> logMsgs = new ArrayList<>();
        for (int i = 1; i < 21; i++)
            logMsgs.add(new LogMsg("From " + i, "To " + i,
                    "This is the message body. This is the message body. This is the message body.\n" +
                            "This is the message body. This is the message body. ",
                    "Remarks remarks remarks remarks"));
        return logMsgs;
    }*/
}
```
