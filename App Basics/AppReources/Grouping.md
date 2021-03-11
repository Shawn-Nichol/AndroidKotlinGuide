## Grouping resource types. 
You should place each type of resource in a specific subdirectory of your progject's res/ directory 


`animator:` XML files that define property animations


`anim:` XML files that define tween animations. (Property animations can also be saved in this directoyr, but the animator/ directory is preferred for property animations to distinguish betweeen the two types. 


`color:` XML filese that define a state list of colors

>
`drawable` Bitmap files(.png, .9.png, .jpg, .gif) or XML files are complied into the following drawable resource subtypes. 


`mipmap:` Drawable filese for different launcher icon densities. For more information on managing launcher icons with mipmap/ folders see Managing Progjects Overview


`layout:` XML files that define a user interface layout.

`menu:` XML files that define app menus, such as an options menu, context menu, or sub menu

`raw:` Arbitrary files to save in their raw form. To oopen these resoources with a raw InputStream, call Resources.openRawResource() with the resource ID, which is R.raw.filename
however, if you need access to original file names and file hierarchy, you might consider saving some resources in the assets/ directory (instead of res/raw/). Files in assets/ aren't given a resrouce ID, so you can read them only using AssetManager. 

`values:` XML files that contain simple values, such as strings, integers and colors. Where as XML resource files in other res/ subdirectiories define a single resource based on the XML filename, files inteh values/ directory describe multiple resources. For a file in this directory, each child of the <resources> element defines a single resource. For example, a <String> element creates an R.string resource and <color> element creates an R.color resource. 
  
Becuase each resource is defined with its own XML element, you can name the file whatever you want and place different resource types in one file. However, for clarity, you might want to place unique resource types in different files. For example, here aree some filename conventions for resources you can create in this directory. 
- arrays
- colors
- dimens
- strings
- styles


  
## Providing alternative resources
Almost every app should provide alternative resources to support specific device configurations and different screen sizes. For instance, you should include alternative drawable resources for different screen densities and alternative string resources for different languages. At runtime, Android detects the current device configuration and loads the appropriate resources for your app. 

To specify configuration-specific alternatives for a set of resources:
1) Create a new directory in res/ named in the form <resources_name>-<qualifier>
  - <resource_name> is the directory name of the corresponding default resources
  - <qualifier> is a name that specifies an indvidual configuration for which these resources are to be used.
  
2) Save the respective alternative resources in this new directory. The resource files must be named exxactly the same as the default resource files. 
```
res/ 
  drawabe/
    icon.png
    background.png
  drawable-hdqi/
    icong.png
    background.png
```

The hdqi qualifiier indicates that the resources in that directory are for devices with a high-density screen. The images in each of these drawable directories are sized for a specific screen density, but the filenames are exactly the same. This way, the resource ID that you use to reference th eicon.png or background.png image is always the same, but Android selects the version of each resource that best matches the current device, by comparing the device configuration information with the quilifiers in the resource directory name.

Android supports several configuration qualifiers and you can add multiple qualifiers to one directory name, by separating each qualifier with a dash. Table 2 lists the valid configuration qualifiers, in order of precedence if you use multple qualifiers for a resource firectory, you must add them to the directory name in the order they are listed in the table. 
