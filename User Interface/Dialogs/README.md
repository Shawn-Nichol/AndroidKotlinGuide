# What is a dialog
A dialog is a smal lwindow that prompts the user to make a decision onenter additional infomration. A dialog does not fill the screen and is normally used for modal events that require users to take an action before they can proceed. 

The dialog class is the base class for dialogs, but you should avoid instantiating Dialog direclty instead use one of th following subclasses `AlertDailog`, `DatePicker` or `TimePicker`

`AlertDialog`: can show a title, icons, and up to three buttons. 

These classes define the style and strucutre ofr your dialog, but you should use a DialogGrament as a container for your dialog. The `DialogFragment` class provides all the controls you need to create your dialog and manage its appearance, instead of calling methods on the Dialog object.

Using `DialogFrgament` to manag ethe dialog ensuress that it correctly handles lifecycle events such as when the user press the back button or rotates the screen. The `DialogFragment` clas also ensures that it correctly handle slifecycle events such as when the user presses the back button or urotates the screen. The `DialogFragment` class also allows you to reuse the dialog's UI as an embeddable component in a larger UI, just like a traditional Fragment such as when you want to the dialog uI to appear differently on large and small screen. 


Alert Dailog

DialogFragment


Dialog List:
There are three kind  of list available with the AlertDailog
1. Single choice list
2. Single choice radio buttons
3. a persisten multiple choice list(checkboxes)

