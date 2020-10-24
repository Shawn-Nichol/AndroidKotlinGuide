# Ties destination to menu items
Navigation UI also provides helpers for typing destinations to menu-driven UI components. NavigationUI contains a helper method, onNavDestinationSelected(), which taks a MenuItem along with the NavController that hosts the associated destination . If the id of the MenuItem matches the id of the destination, the NavController can then navigate to that destination. 

nav_graph
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    ... >

    ...

    <fragment android:id="@+id/details_page_fragment"
         android:label="@string/details"
         android:name="com.example.android.myapp.DetailsFragment" />
</navigation>

```

menu item
```
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    ...

    <item
        android:id="@+id/details_page_fragment"
        android:icon="@drawable/ic_details"
        android:title="@string/details" />
</menu>
```

call onNavDestionSelected()

```
   override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return when(item.itemId) {
            R.id.details_page_fragment -> {
                displayMenuItemSelection("Fragment secret one")
                val navController = findNavController(R.id.nav_host_fragment)
                return NavigationUI.onNavDestinationSelected(item!!, navController) || super.onOptionsItemSelected(item)
            }
            
            ... 
        }
  }
```
