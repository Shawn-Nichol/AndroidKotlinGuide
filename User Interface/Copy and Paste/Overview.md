Android provides a powerful clipboard-based framework for copyingn and pasting. It supports both simple and complex data types, including text strings, complex datat structures, text and binary stream data, and even application assets. Simple text data is stored directly in the clipboard, while complex data is stored as a reference that the pasting application resolves with a content provider. Copying and pasting works both within an application and between applications that implement the framework. 

## The clipboardframework
When you use the  clipboard framework, you put data into a clip object and then put the clip object on the systme -wide clipboard. The clip object can take one of three forms

Text: 
A text string. You put the string directly into the cliip object, hwich you then put onto the clipboard. To paste the string, you get teh clip object from the clip board and copy the string into your application. 

URI:
A Uri object represnting any form of URI. This is primarily for copying complex data from a content provider. To copy data, you put a Uri object into a clip object and put the clip object onto the clipboard. To paste the data, you get the clip object, get the URi object, resolve it to a data source such as a content provider, and copy the data from the source into your application's storage. 

Intent
An intent. This supports copying application shorcuts. To copy data, you create an intent put it into a clip object, and put the clip object onto the clipboard. To paste  the data, you get teh clip object and then copy the intent object into your application's memory area. 

The clipboard holds only on clip object at a time. When an application puts a clip object on the clibooard. the pregvious clip object disappears. 

If you want to allow users to paste dat into your application you don't have to handle all types of data. You can examine the data on teh clipbaord before you give users the option to paste it. Besides having a certain data form, the clip object also contain metadata that tells you what MIME type ofr types are available. This metadata helps you decide if your application can do somethign useful with the clipboard data. For example, if you have an application that primarily handles text, you may want to ignore clip objects that contain a URI or intent. 

You may also want to allow users to paste text regardless of the form of data on the clipboard. To do this, you can force the clipboard data into a text representation, and then paste this text. This is described in the section coercing the clipboard to text. 

# Clipboard classes
##ClipboardManager
In the android system, the system clipboard is represented by the global ClipboardManager class. you do not instantiate this class directly insted you get a reference to it by invoking getSystemService

## ClipData, ClipData.Item and ClipDescription
To add data to the clipboard, you createa a ClipData object that contains both a description of the dta and the data itself. The clipboard holds only one ClipData at a time. AclipData contains a ClipDEscription object and one mor more ClipData.ITem objects

Aclipdescription object contains metadata about the clip. In particular it contains an array of avaiable MIME types for the cllip's data. When you put a clip on th e clipboard, this array is aviable to pasting applications, which can examine it to see if they can handle any the available MIME types. 

A ClipData.Item object contains the text, URI or Intent data:

## CllipData convenicne methods
The clipData class provides static convenicne methods for creating  a ClipData object with a single ClipdAta.Item object and a simple ClipDescription object


Not finished no need for this yet will be moving on for now. 
