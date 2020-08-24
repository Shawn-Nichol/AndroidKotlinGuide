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

## Imports
Importa allow you to reference classes inside your layout file, just like in managed code. Zero or more import elements my be used inside the data element. The following code example imports the View class to the layout file. 

```
   <data>
      <import  type="android.view.View"/>
   </data>
```

Importing the View class allows you to reference it from your binding expressions. The following example shows how to reference the VISIBLE and GONE constants of the View class. 
```
<TextView
   android:text="@{user.lastName}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>
```


## Variables 

You can use multiple variables elements inside the data element. Each variable element describes a property that may be set on the layout to be use in binding expresions within the layout file.

```
<data>
   <import> type="android.graphics.drawable.Drawable"/>
   <variable name="user' type="com.example.User"/>
   <variable name="image" type="Drawable"/>
   <variable name="note" type="String"/>
</data>

The variable types are inspected at compile time, so if a variable implements Observable or is an observable collection, that should be reflected in the type. If the variable is a base class or interface that doesn't implement the Observable interface, the variables are not observed. 

When there are different layoutfiles for various configuartions the variables are combined. There must not be conflicting variable definitions between these layot filse. 

The generated binding class has a setter and getter for each of the descibed variables. The variables take the default managed code values until the setter is called for reference types

A special vaiable named context is generated for the use in binding expressions as needed. The value for context is the Context object form the root View's getContext() method the context vaiable is overriden by an explicit variable declaration with the that name. 
```

## Include
Variables may be passed into a n included layout's binding from the containg layout by using the app namespace and the variable name in an attribute. The following example shows included user variables form the name.xml and context.xml layout files. 

```
<Layout
   ...
   />
   <data>
      <variable name="user" type="com.example.User"/>
   </data>
   
   <LinearLayout
      ...>
      <include layout="@layout/name"
         binding:user"@{user}"/>
      <include layout="@layout/contact"
         binding:user="@{user}"/>
      
</Layout
```
Data binding doesn't include as a driect child of merge. 
