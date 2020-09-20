Although a fragment is an independent object it is directly tied to the activity that called it. 

Specifically, the fragment can access the FragmentActivity instance with getActivity() and easily perfrom tasks such as find view in the activity layout.
```
val listView: View? = activity?.findViewById(R.id.list)
```
Likewise, your activity can call methods in the fragment by acquiring a reference to the Fragment from FragmentManager, using findFragmentById() or findFragmentByTAg().
```
val fragment = supportFragmentManager.findFragmentByID(R.id.my_fragmnet) as MyFragment
```
