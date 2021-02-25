## Accessing your app resources
Once you provide a resource in your application, you can apply it by referencing its resource ID. All resource iDs are defined in you progject's R class, which the apt tool automatically generates. 

When your application is compiled, apt generates the R class, which contains resource IDs for all the resources in your res/ directory. For each type of resource, there is an R subclass, and for each resource of that type, there is  static integer. This integer is the resource ID that you can use to retrieve your resource. 

Although the R class is where resource IDs are specified you should never need to look there to discover a resource ID. A resource ID is always composed of:
- The resource type: each resource is grouped into a "type", such as string, drawable, and layout.
- The resource name, which is either: the filename, excluding the extension; or the value in the XML android: name attibute, if the resource is a simple value (such as a string)

There are two ways you can access a resource
- In code using a static integer from a sub-class of your R class, such as.  R.string.hello, string is the resource type and hello is the resource name. There are many Android APIs that can access your resources when you provide a reosurce ID in this format. 
- In XML Using a special XML syntax that also corresponds to the resource ID defined in your R class such as " @string/hello"  String is the resource type and hello is the resource name. You can use this syntax in an XML resource any place where a value is expected  that you provide in a resource

#### Accessing resources in code
You can use a resource in code by passing the resource ID as a method parameter. For example, you can set an ImageView to use the res/drawable/myimage.png resource using setImageResource(). You can also retrieve indivdiual resources uing methods in Resources, which you can get an instance of with getReources().

#### Accesssing resources from XML
You can define values for some XML aatributes and elements using a reference to an existing resource. You will often do this when creating layout files, to supply string and images for your widgets. 
