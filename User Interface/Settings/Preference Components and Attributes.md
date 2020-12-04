# Preference omponents and attributes

# Preference infastructure
PrefereneFragmentCompat - a Fragment that handles dispalying an interactive hierarchy of Preference objects. 

##Preference containers
-PreferenceScreen - a top-level container that represents a settign screen. This is the root compoennt of your Preference hierarchy. 

- PreferenceCategory - A contienaer that is used to group similar Preferences. a PreferenceCategory displays a category title and viaually separates groups of PRefernces

## Individual Preferences
- Preference - the basic building block that represents an individual setting. If a Prefence is set to persist, it has a corresponding key-value pair that holds the user's choice for the setting that can be accessed elsewhere in the application 

EditTextPReference - a Preference that persists a string value. Users can tap on the PReference to laucn a dialog containg the text field that sallows the user to chagne the persisted value. 

ListPReference - a PRefernce that persists a string value. Users can chagne this value in a dialog that contains a list of radio buttons with corresponding labels. 

MultiSelectListPRefernce - a Preference that persists a set of strings. Users can chagne these values in a dilog that contains a list of checkboxes with corresponding labels. 

SeekbarPRefernce -  Preference that persist an integer value. THis value can be chagned by dragging a correspondign seekbar that is dsplayed in the preference layout

SwitchPRefernceCompat - A preference that persist a boolean value. THis value can be chagned by sinteracitn gwith the corresp
