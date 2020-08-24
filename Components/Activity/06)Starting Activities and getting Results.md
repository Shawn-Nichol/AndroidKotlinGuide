# Starting Activites and Gettign Results
The startActivity(Intent) is used to start a new activity, which will be placed at the top of the activity stack. It takes a single argument, an Intent which describes the activity to be executed. 

For example, you may start an activity that lets the user pick a person in a list of contacts: when it ends, it retuns the person that was selected. Todo this, you call the startActivityForResult(intent, int) version with a second integer parameter identifying the call. The result will come back through your onActivityResult(int, int, Intent) method

When an activity exits, it can call setResult(int) to return data back to its parent. It must always supply a result code, which can be the standard results RESULT_CANCELED, RESULT_OK, or any custom values starting at RESULT_FIRST_USER. In addition, it can optionally return back an intent containing any additional data it wants. All of this information appears back on the parent's Activity.onActivityResult(), along with the integer identifier it orignailly suplied. 

If a result fails for any reason the parent activity will receive a result with the code RESULT_CANCELED


Activity 1
```
// start the Activity 2
fun btnClick() {
  // Create an intent handle, you can also pass data in the intent
  val intent = Intent(this, SecondActivity::class.java)
  
  // startAcitivytForResult has two arguments, the intet to start the activity and the result code to ensure the correct return block of code runs. 
  startActivityForResult(intent, MY_REQUEST_CODE)
}


// The results from the second activity
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
  super.onActivityResult(requestCode, resultCode, data)
  
  // If you are running more than one startActivity for results you will need to have a unique request code for each seperate instance. 
  if(requestcode == 1) {
      if(result == Activity.RESULT_OK && data != null) {
        Log.i("ActivityResults", data.getStringExtra(RESULTS_KEY)
      }
  }
}

```


Activity 2
```
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  
}



// Create a method that return the data when you are done with the activity. 
furn retunResults() {
  val intent = Intent().apply {
    putExtra(RESULTS_KEY, text)
  }
  
  // flags the status of activity 2, so activity 1 knows how to respond. 
  setResult(RESULT_OK, data) 
  
  // End the Activity 2, and returns the activity that started and runs onActivityResults()
  finsih()
}


```
