Async Task
----------
```java
    *//**
     * Async task class to get json by making HTTP call
     *//*
    private class GetContacts extends AsyncTask<Void, Void, Void> {

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            // Showing progress dialog
            pDialog = new ProgressDialog(RtcActivity.this);
            pDialog.setMessage("Please wait...");
            pDialog.setCancelable(false);
            pDialog.show();

        }

        @Override
        protected Void doInBackground(Void... arg0) {
            HttpHandler sh = new HttpHandler();

            // Making a request to url and getting response

            url = "http://" + getResources().getString(R.string.host);
            url += (":" + getResources().getString(R.string.port) + "/streams.json");
            String jsonStr = sh.makeServiceCall(url);

            //   Log.e(TAG, "Response from url: " + jsonStr);

            if (jsonStr != null) {
                try {
                    // JSONObject jsonObj = new JSONObject(jsonStr);
                    // Getting JSON Array node
                    JSONArray contacts =  new JSONArray(jsonStr);
                    //jsonObj.getJSONArray("contacts");

                    // looping through All Contacts
                    for (int i = 0; i < contacts.length(); i++) {
                        JSONObject c = contacts.getJSONObject(i);

                        String id = c.getString("id");
                        String name = c.getString("name");
                        // tmp hash map for single contact
                        HashMap<String, String> contact = new HashMap<>();
                        // adding each child node to HashMap key => value
                        contact.put("id", id);
                        contact.put("name", id);
                        contact.put("email", name);
                        contact.put("mobile", name);

                        // adding contact to contact list
                        contactList.add(contact);
                    }
                } catch (final JSONException e) {
                    //   Log.e(TAG, "Json parsing error: " + e.getMessage());
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            Toast.makeText(getApplicationContext(),
                                    "Json parsing error: " + e.getMessage(),
                                    Toast.LENGTH_LONG)
                                    .show();
                        }
                    });

                }
            } else {
                //  Log.e(TAG, "Couldn't get json from server.");
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        Toast.makeText(getApplicationContext(),
                                "Couldn't get json from server. Check LogCat for possible errors!",
                                Toast.LENGTH_LONG)
                                .show();
                    }
                });

            }

            return null;
        }

        @Override
        protected void onPostExecute(Void result) {
            super.onPostExecute(result);
            // Dismiss the progress dialog
            if (pDialog.isShowing())
                pDialog.dismiss();
            *//**
             * Updating parsed JSON data into ListView
             * *//*
            ListAdapter adapter = new SimpleAdapter(
                    RtcActivity.this, contactList,
                    R.layout.list_item, new String[]{"name", "email",
                    "mobile"}, new int[]{R.id.name,
                    R.id.email, R.id.mobile});

            lv.setAdapter(adapter);

            lv.setOnItemClickListener(new AdapterView.OnItemClickListener() {

                @Override
                public void onItemClick(AdapterView<?> parant, View arg1, int position,
                                        long long_id) {
                    // TODO Auto-generated method stub

                    HashMap<String, String> item = (HashMap<String, String>)  parant.getItemAtPosition(position);
                    String id = item.get("name");
                    String name = item.get("email");
                    callerId = id;
                    try {

                        //if(OwnCallID != callerId) {
                            callRequestFlag = CALL_REQUEST_SENT;
                            client.sendMessage(id, "init", null, false);
                            lv.setVisibility(View.INVISIBLE);
                            outgoinglinearLayout.setVisibility(View.VISIBLE);
                            listViewTrack = false;
                            caller = true;
                       // }
                    } catch (JSONException e) {
                        e.printStackTrace();
                    }
                }

            });
        }

    }
    ```
