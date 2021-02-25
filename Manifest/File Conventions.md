## File conventions
### Elements
Only the <manifest> and <application> elements are requrired. They each must occur only once. Most of the other elements can occur zero or more times. However, some of them ust be present to make the manifest file useful. All o fthe values are set through attributes, not as character data within an element. 
  
Elements at the same level are  generally not ordered. For example, the <activity>, <provider>, and <service> elemetns can be placed in any order. There are two key exceptions to this rule. 
  
1. <activity-alias> element must follow the <activity> for which it is an alias. 
2. <application> element must be the last element inside the <manifest> element. 

### Attributes
Techincally, all attributes are optional. However, many attributes must be specified so that an element can accomplish its purpose. 

Except for some attributes of the root, <manifest> element, all attribute names begin with an android: prefix. For example, andoidr: always RetainTaskState. Becuase the prefix is univeral, the documentatioin generally omits it when referring to attributes by name. 
  
