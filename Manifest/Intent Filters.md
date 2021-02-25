### Intent filters
An intent is a message defined by an Intent object that describes an action to perform, including the data to be acted upon, the cateogry of component that should perfrom the action, and other instructions

When an app issues an intent to the system, the system locates an app component that can handle the intent based on intent filter declarations in each app's manifest file. THe system launches an instance of the matching component and passes the Intent object to the component. If more than one app can handle the intent, then the user can selecct which app to use. 

An app component can have any number of intent filters (defined with the <intent-filter> element), each one describing a different capability of that component. 



