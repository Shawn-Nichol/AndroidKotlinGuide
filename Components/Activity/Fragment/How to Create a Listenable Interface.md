# How to create a listenable interface for fragments. 
The recommend way to communicate with an activity or fragment is with a shared ViewModel, if ViewModel isn't an option you can use a listenable interface.
There are three steps to creating a Listenable interface


## 1) step define the interface
Create an interface in the Fragment and link the methods you want to call. 

```
class MyFragment : Fragment() {
  
  interface MyFragmentListener {
    // Method in the MainActivity
    fun numberSelected(number: Int)
  }

```

## 2) Define the callback variable
The callback will be implemented in the set...() method called after onAttach() and before onCreate()
```
class MyFragment : Fragment() {
  lateinit var callback: MyFragmentListener
 
  fun setFragmentListener(callback: MyFragmentListener) {
    this.callback = callback
  }
 
  interface MyFragmentListener {
    fun MainActivityMethod(position: Int) 
  }
```

## 3) Setup the Listener

```
class MainActivity: Activity(), MyFragment.MyFragmentListener {
  
  override onAttachFragment(fragment: Fragment) {
    if(fragment is MyFragment) {
      fragment.setMyFragmentListener(this)
    }
  }
  
  // The method you want to run. 
  fun numberSelected(number: Int) {
    ...
  }
}
```
