# Material theme and widgets
To take advantage of the material features such as styling for standard UI widgest and to streamline your app's style defintion apply a materail-based them to your app. 

Material design most common UX patters
- Promote your UI's maint action with a Float Action Button
- Show your brand, navigation, search and other action with the App Bar.
- Show and hide your app's navigation with the Navigation Drawer
- Useone of the many other materail componets for your app layout and navigation such as collapsing toolbars tabas, a bottom nav bar and more. To see them all, echk out the Materail components for android cataglog.

Whenever possible, use predefined materail icons. For exmaple, the navigation 'menu' button for your navigationdrawer should use the standard "hamburger" icon

## Elevation shadows and cards
In addition to the X and Y properties, views in Android hava Z property. This new property represents the elevation ofa view, which detereines
- the size of the shadow: views with higher Z values cast bigger shadows. 
- The drawing order: views with higher Z values appear on top of other views. 

Elvation is often applied when your layoutincludes a card-based layout, which helps you display important pieces of information inside cards that providess a material look. You can can use `CardView` widget to create cards with a default elevation

## Animations
The new animation APIs let you create custom animations for touch feedback in UI controls, changes in view state and activity transitions. 

- Respond to touch events in your views with touch feedback animations
- Hide and show views with cricular reveal animations
- Switch between activites with custom activity transition sanimations. 
- Create more natural animations with curved motion. 
- Animate changes in one or more view properites with view state change animations. 
- Show animations in state list drawables between view state changes. 

Touch feedback animations are built into several standard views, such as buttons, The new APIs llet you customize these animations and add them to your custom views. 

## Drawables. 
These new capabilities for drarwables helpyou implement material design apps
- Vector drawables are scalable without losing definition and are perfect for single-color in app iconrs. 
- Drawable tinting lets you define bitmaps as an alpha mask and tintt them with a color at runtime.
- Color extraction lets you atutomtaicallye xtract prominent colors from a bitmap image. 
