#Overview
## Guiding princple
In general, any task that takes more than a few milliseconds should be delegated to a background thread. Common long-running tasks includ things like decoign a bitmap, acccessing storeage working on a mahicn elearning model, or perform network requersts. 

## Defintion of background work
An app is considered to be running in the backgroun as long as each of the following donditions are staisfied. 1
1. None of the app's activites are currently visible to the user.
2. The app isnt' running any foregournd services that started while an activity from the app was visible to the user. 

The following list shows common pending tasks that an app manages while it runs in the background
- Your app registers a broadcast receiver in teh manifest
- Your app schedules a repeating alarm Using Alarm Manager
- Your app shcedules a background task, either as a worker using WorkManager or a job using Job scheduler. 

## Categories of background tasks 
Backgroun tasks fall into one of the following main categories
- Immediate 
- Deferred 
- Exact

To categorize a task, answer the following questions, and traverse the corresponndign decision tree

### Does the task need to completet while the user is interacting with the application?

IF so, this task should be categorized for immediate execution. If not, proceed

### Does the task need to run at an exact time?
If you do need to run a task at an exact time, categorize the task as exact
Most tasks don't need to be run at an exact time. Tasks gerneally allow for slight variations in when they run that are based on conditions such as network availability and remraining battery. Tasks that dont' need to be run at an exact time should be categorized as deferred. 

## Recommended solutions
Immediate tasks
We reommend Kotlin coroutines for tasks that should end when the user leaves a certain scope or finsihes an interaction. Many Android KTX libraries contain ready to use coroutines scopes for common app components like ViewModel and common application lifecycles. 

For tasks that should be executed immdiately and need continued processing, even if the user puts the application in background or the device restarts, we reommend using WorkManager and its support for long-running tasks. 

In specific cases, such as with media playback or active navigation,you might want touse foregourn service, directly. .

## Deferred tasks
Every task that is not directly oconnected to a user interaction and can run at any time in the futre can be deferred. The recommend solution for deferred tasks is WorkManager. 

WorkManager makes it easy to schedule deferrable, ashynchrounous task that are epxected to run evern if the app exits or the device restarts.

## exact tasks 
A task that needs to bexecuted at an exact poitn in time can use AlarmManager

