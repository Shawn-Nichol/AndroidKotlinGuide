## Forcing an app chooser
When there is more than one app that responds to your impicit intent,the user can select which app to use and make that app the default choice for thaction. The ability to select a default is helpful when performing an action for which the user probably wants to use the same app everytime. 

However, if multiple app can respond to the intent and the user migh want to use a different app each time, you should explicityl show a chooser dialog. THe chooser dialog asksthe user t select whic app to use for the action. 

To show the chooser, create a intent using createChooser() and pass it to startActivity()
```
val sentIntent = INent(Intent.ACTION_SEND)
val title: String = resource.getString(R.string.choose_title)

// Create intent to show the chooser dialog
val chooser: Intent = Intent.createChooser(sendIntent, title)

// Verify the original intent will resolve to at least one activity
if(sendIntent.resolveActivity(packageManager) != null) {
    startActivity(chooser)
}
```
