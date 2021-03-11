# Qualifier name rules
- You can specify multiple qualifiers for a single set of resources, separated by dashes. </br>
ex. drawable-en-rUS-land applies to US-English devices in landscape orientation.

- The qualifiers must be in the order listed
  - wrong: drawable-hdpi-port
  - correct drawable-port-hdpi/
- Alternative resource directories cannot be nested. </br>
EX. you cannot have res/drawable/drawable-en/
- Values are case-insensitive. The resource compiler converts directory names to lower case before processing to avoid problems on case-insensitive files systems. Any capitalization in the names is only to benefit readability.
- Only one value for each qualifier type is supported. </br>
Ex.  if you want to use the same drawable files for spain and France, you cannot have a directory named drawable-es-fr/. Instead you need two resource directories, such as drawable-es/ and drawable-fr/, which contain the appropriate files. however, you aren't required to actually duplicate the same files in both locationn. instead, you can create an alias to a resource. 


