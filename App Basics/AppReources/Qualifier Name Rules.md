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
