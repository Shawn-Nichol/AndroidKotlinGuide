## Starting Activites and Gettign Results
The startActivity(Intent) is used to start a new activity, which will be placed at the top of the activity stack. It takes a single argument, an Intent which describes the activity to be executed. 

Sometimes you want to get a result back from an activity when it ends. For example, you may start an activity that lets the user pick a person in a list of contacts: when it ends, it retuns the person that was selected. Todo this, you call the startActivityForResult(intent, int) version with a second integer parameter identifying the call. The result will come back through your onActivityResult(int, int, Intent) method

When an activity exits, it can call setResult(int) to return data back to its parent. It must always supply a result code, which can be the standard results RESULT_CANCELED, RESULT_OK, or any custom values starting at RESULT_FIRST_USER. In addition, it can optionally return back an intent containing any additional data it wants. All of this information appears back on the parent's Activity.onActivityResult(), along with the integer identifier it orignailly suplied. 

If a result fails for any reason the parent activity will receive a result with the code RESULT_CANCELED
