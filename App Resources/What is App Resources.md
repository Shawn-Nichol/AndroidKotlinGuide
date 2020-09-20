# App Resources. 
Resources are the additional files and static content that your code uses, such as bitmaps, layout definitions, user interface strings, animation instructions, and more. 
You should always externalize app resources such as images and string sfrom your code, so that you can maintain them independently. You should also provide alternative resources for specific device configurations, by grouping them in specially named resources directories. At runtime, Andoid uses the appropriate resource based on the current configuration. For exmample, you might want to provide a different UI layout depending on the screen size or different strings depending on the language. Once you externalize your app resources, you can access them using resource IDs that are generated in your projects's R class. T

## Grouping resource types. 
You should place each type of resource in a specific subdirectory of your progject's res/ directory 

animator/ XML files that define property animations

anim/ XML files that define tween animations. (Property animations can also be saved in this directoyr, but the animator/ directory is preferred for property animations to distinguish betweeen the two types. 

color/ XML filese that define a state list of colors

drawable/ Bitmap files(.png, .9.png, .jpg, .gif) or XML files are complied into the following drawable resource subtypes. 

mipmap/ Drawable filese for different launcher icon densities. For more information on managing launcher icons with mipmap/ folders see Managing Progjects Overview

layout/ XML files that define a user interface layout.

menu/ XML files that define app menus, such as an options menu, context menu, or sub menu

raw/ Arbitrary files to save in their raw form. To oopen these resoources with a raw InputStream, call Resources.openRawResource() with the resource ID, which is R.raw.filename
however, if you need access to original file names and file hierarchy, you might consider saving some resources in the assets/ directory (instead of res/raw/). Files in assets/ aren't given a resrouce ID, so you can read them only using AssetManager. 

values/ XML files that contain simple values, such as strings, integers and colors. Where as XML resource files in other res/ subdirectiories define a single resource based on the XML filename, files inteh values/ directory describe multiple resources. For a file in this directory, each child of the <resources> element defines a single resource. For example, a <String> element creates an R.string resource and <color> element creates an R.color resource. 
  
Becuase each resource is defined with its own XML element, you can name the file whatever you want and place different resource types in one file. However, for clarity, you might want to place unique resource types in different files. For example, here aree some filename conventions for resources you can create in this directory. 
- arrays
- colors
- dimens
- strings
- styles

xml/ Arbitrary XML files that can be read at runtime by calling Resources.getXML(). Various XML configuration files must be saved here, such as a searchable configuration. 
font/ Font files with extensions such as .ttf, otf, or .ttc, or XML files that include a <font-family> element. 
  
The resouces that you save in the subdirectories defined are "default resources. That is, these ressources define the default design and content for your app. However, different types of Android-powered devices might call for different types of resources. For example, if a device has a larger than normal screen, then you should provide different layout resources that take advantage of different string resources that translate the text in your user interafce. To provide these different resources for different device configurations, you need to provide alternative resources, in addition to your default resources. 

## Providing alternative resources
Almost every app should provide alternative resoources to support specific device configurations. For instance, you should include alternative drawable resources for different screen densities and alternative string resources for different languages. At runtime, Android detects the current device configuration and loads the appropriate resources for your app. 

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

# Qualifier name rules
Here are some rules about using configuration qualifier names:
- You can specify multple qualifiers for a single set of resources, separated by dashes. For example drawable-en-rUS-land applies to US-English devices in landscape orientation.
- The qualifiers must be in the order listed
  - wrong: crawable-hdpi-port
  - correct drawable-port-hdpi/
- Alternative resource directories cannot be nested. For example, you cannot have res/drawable/drawable-en/
- Values are case-insensitive. The resource compiler converts directory names to lower case before processing to avoid problems on case-insensitive files systems. Any capitalization in the names is only to benefit readability.
- Only one value for each qualifier type is supported. For example, if you want to use the same drawable files for spainand France, you cannot have a directory named drawable-es-fr/. Instead you need two resource directories, such as drawable-es/ and drawable-fr/, which contain the appropriate files. however, you aren't required to actually duplicate the same files in both locationn. instead, you can create an alias to a resource. 

After you save alternative resources into directories named with these qualifiers, Android automatically applies the resources in your app based on teh current device configuration. Each time a resource is requeseted, Android checks for alternative resource directories that contain teh requested resourcce file, then finds the best-matching resource. If there are no alternative resources that match a particular device configuration, then Android uses the correesponding default resources (the seto f resoures for a particular resource type  that doesn''t include a configuration qualifier).

## Creating alias resources. 
When you have a resource that you'd like  to use for more than one evice configuration (but don't want to provide as a default resource), you don't need to put the same resource in more than one alternative resource directoyr. Instead, you can create an alternative resource taht acts as an alias for a resource saved in your default resource directory. 

Note: Not all resources offer a mechanism by which you can create an alias to another resource. In particular, animation, menu, raw, and other unspecified resources in the xml/ directory don't offer this feature. 

For exmaple, imagine ou have an app icon, icon.png and need unique version of it for a didferent locales. However, two locales, English-Canadain and French-Canadian, need to use the same version. You might assume that you need to copy the same image inot the reource direcotry for both English and French, but it's not true. Instead, you can save the image that's used for both as icon_ca.png and put it in the default res/drawable/ directory. Then create an icon.xml file in res/drawable-en-rCA/ and res/drawable-fr-rCA/ that refers to the icon_ca.png resource using the <bitmap> element. This allows you to store just one veresion of the PNG file and two small XML files that point to it
  
## Accessing your app resources
Once you provide a resource in your application, you can apply it by referencing its resource ID. All resource iDs are defined in you progject's R class, which the apt tool automatically generates. 

When your application is compiled, apt generates the R class, which contains resource IDs for all the resources in your res/ directory. For each type of resource, there is an R subclass, and for each resource of that type, there is  static integer. This integer is the resource ID that you can use to retrieve your resource. 

Although the R class is where resource IDs are specified you should never need to look there to discover a resource ID. A resource ID is always composed of:
- The resource type: each resource is grouped into a "type", such as string, drawable, and layout.
- The resource name, which is either: the filename, excluding the extension; or the value in the XML android: name attibute, if the resource is a simple value (such as a string)

There are two ways you can access a resource
- In code using a static integer from a sub-class of your R class, such as.  R.string.hello, string is the resource type and hello is the resource name. There are many Android APIs that can access your resources when you provide a reosurce ID in this format. 
- In XML Using a special XML syntax that also corresponds to the resource ID defined in your R class such as " @string/hello"  String is the resource type and hello is the resource name. You can use this syntax in an XML resource any place where a value is expected  that you provide in a resource

## Accessing resources in code
You can use a resource in code by passing the resource ID as a method parameter. For example, you can set an ImageView to use the res/drawable/myimage.png resource using setImageResource(). You can also retrieve indivdiual resources uing methods in Resources, which you can get an instance of with getReources().

## Accesssing resources from XML
You can define values for some XML aatributes and elements using a reference to an existing resource. You will often do this when creating layout files, to supply string and images for your widgets. 


  
  
  
  
