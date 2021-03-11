# Accessing your app resources
Once you provide a resource in your application, you can access it by referencing its resource ID. All resource IDs are defined in the project's R class, which the APT tool automatically generates. 

When your application is compiled, APT generates the R class, which contains resource IDs for all the resources in your res/ directory. For each type of resource, there is an R subclass, and for each resource of that type, there is  static integer. This integer is the resource ID that you can use to retrieve your resource. 


## Accessing resources in code
You can use a resource in code by passing the resource ID as a method parameter. 

<br/>
Example, you can set an ImageView to use the res/drawable/myimage.png resource using setImageResource(). You can also retrieve indivdiual resources uing methods in Resources, which you can get an instance of with getReources().

## Accesssing resources from XML
You can define values for some XML atributes and elements using a reference to an existing resource.

<br/>
Example " @string/hello"  String is the resource type and hello is the resource name. You can use this syntax in an XML resource any place where a value is expected  that you provide in a resource

