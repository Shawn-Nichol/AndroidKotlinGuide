## Alias resources. 
When you have a resource that you'd like  to use for more than one device configuration (but don't want to provide as a default resource), you don't need to put the same resource in more than one alternative resource directoyr. Instead, you can create an alternative resource taht acts as an alias for a resource saved in your default resource directory. 

Note: Not all resources offer a mechanism by which you can create an alias to another resource. In particular, animation, menu, raw, and other unspecified resources in the xml/ directory don't offer this feature. 

### Exmaple
imagine you have an app icon, icon.png and need unique version of it for a didferent locales. However, two locales, English-Canadain and French-Canadian, need to use the same version. You might assume that you need to copy the same image inot the reource direcotry for both English and French, but it's not true. Instead, you can save the image that's used for both as icon_ca.png and put it in the default res/drawable/ directory. Then create an icon.xml file in res/drawable-en-rCA/ and res/drawable-fr-rCA/ that refers to the icon_ca.png resource using the <bitmap> element. This allows you to store just one veresion of the PNG file and two small XML files that point to it
 
