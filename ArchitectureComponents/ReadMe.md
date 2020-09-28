# Guide to app architecture

## Mobile apps and user experinces
In the majority of cases, desktop apps have a single entry point from  a desktop or program launcher, then run as a single, monolithic process. Android apps, on the other hand, have a much more complext structure. A typical android app contians multiple app components, including activites, fragments, services, content providers and broadcast receivers. 

You declare most of thesse app components in your app  manifest. The android OS then uses this file to decide how to integrate your app into the device's overall user experince. Given that a properly written android app contains multiple components and that users often interact with multiple apps in a short peroid of time, apss need to adapt to different kinds of user-driven workflows and tasks. 

Keep in mind that mobile devices are also resource-constrained, so at any time, the operating system might kill some app rpocesses to make room for new ones. 

Given the condititions of this environment, it's possible for your app components to be launched individually and out-of-ordder, and the OS or user can destory them at any time. Becuase these events aren't under your control, you shouldn't store any app data or state in your app components, and your app components shouldn't depend on each other. 


## Seperation of concerns
The most important principle to follow is seperation of concnerns. Its a common mistake to write all your code in an activity or fragment. These UI-based classes should only contain logic that handles UI and operating system interactions. By keeping these classes as lean as possible, you can avoid many lifecycle-releated problems. 

Keep in mind that you don't own implmementations of Activity and Fragments; rather these are just glue classes that represent the contract between teh Android OS and your app. The OS can destroy them at any time based on user interactions or becuase of system conditions like low memory. To provide a satisfactory user experince and a more manageable app maintenance experince, it's best to minimize your dependency on them. 

## Drive UI from a model
Another important principle is that you should drive your UI from a model, preferably a persistent model. Models are components that are responsible for handling the data for an app. They're independent from the View objects and app components in your app, so they're unaffected by the app's lifecycle and associated concerns. 

Persistence is ideal for the following reasons
- Your users don't lose data if the Android OS destroys your app to free up resources. 
- Your app continues to work in cases when a network connection is flaky or not available. 
By basing your app on model classes with the well-defined responsibility of managing the data, your app is moretestable and consisten. 

## Recommend app architecture
It's impossible to have one way of writing apps that works best for every scenario. That being said, this recommended architecture is a good starting point for most situations and workflows.

## Build the userinterface
The UI consists of a fragment, UserProfileFragment, and its corresponding layout file, user_profile_layout.xml
To drive the UI, our data model needs to hold the following data elements. 

- User ID: The identifier for the user. It's best to pass this information into the fragment using the fragment arguments. If the Android OS destroys our process, this information is preserved, so the ID is available the next time our app is restarted. 

- User Object A data class that holds details about the user. 

We use a UserProvileViewModel, based on the ViewModel architecture component, to keep this information. 

A ViewModel object provides the data for a specific UI component, such as a fragment or activity, and contains data-hanlding business logic to  communicate with the  model. For example, the ViewModel can call other components to load the data, and it can forward user requests to modify the data. The ViewModel doesn't know about UI components, so it isn't affected by confiuration changes, such as recreating an activity whenrotating the device. 


 


