# Layout and Binding expressions

The expression language allows you to write expressions that handle events dispatched by the views. The data binding library automatically generates the classes required to bind the views in the layout with your data bojects. 

Data bidning layout files start with a root tag of layout followed by data element and view root element. 

```
<layout ...>
   <data>
      <variable>
         ...
      </variable>
   </data>
</layout>

```

A variable within data describes a property that may be used within this layout. 
```
<variable name="user" type="com.exmaple.MyActivity"/>
```

Expressions within the layout are written in the attribute properties using the @{} syntax
```
<TextView
   ...
   android:text="@{user.firstName}"
```


