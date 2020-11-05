# Optimikmizing layout Hierarchies
It is a common misconception that using the basic layout strcutres leads to most efficient layouts. However, each widget and layout you add to your applicatino requires initialization, layout, and drawing. For example, using nested instances of `Linearlayout` can lead to an excessively deep view hierarchy. Furthermore, nesting several instances of `LinearLayout` that use the `layout_wieght` parameter can be especially expensive as each child needs to be measured twice. This is particularly important when the layout is inflated repeatedly, such as wehn used in a `ListeView` or `GridView`. 

## Inspect your layout
The Android SDK tools include a tool called `Hierachy Viewer` that allows you oto analyze your layout while your application is running. Using this tool helps you discover bottlenecks in the layout performance. 

Hierarchy Viewer works by allowing you to select running processes on a connected device or emulator, then display the layoutree. The traffic lights on each bock represent its Measured, Layout and Draw performance, helping you identify potential issues. 

`Hierarchy VIewer` shows a list of aviable devices and their running componets. DChoose your componet from the Windows tab, and click Hierarchy View to view the layout Hierarchy of the selected component. 

Revise your layout
Becuase the layout performance above sholows down doe to a nested LLinearLayout, the perofmrnace might improve by flattening the layout make the layout shallow and wide, rather than narrow and deep. A RelativeLayout as the roo node allows for such layouts. So when this design is converted to use RealtiveLayout, you can see that the layout becomes a 2-level Hierarchy. Inspection of the new layout looks llike 

Most of the difference is due to the use layout_weight in the LinearLayout design, which can slow down the speed of mearuement. it is just one example of how each layout has appropriate uses and you should carefully consider whether using layout weight is necessary. 

In some comples layouts, the sywttme may waste effort measuring the same UI element more than once. THis phenonomeon is called double taxation. For more

## Use Lint
It is always good practice to run the lint tool on your layout files to serach for possible view hierarchy optimizations. Lint has replaced the Layoutopt tool and has much greater functionality.
- Use compond Drawables- A `LinearLayout` which contains an `ImageView` and a `TextView` can be more efficiently handled as compound drawable.
- Merfge root frame - If a `FrameLayout` is the root of a layout and does not provide background or padding it can be replaced with a merge tag which is slightly more efficient. 
- Useless leaf - A layotu that has no children or no background can often be removed for a flatter and more efficient layout hierarchy. 
- Useless parent - A layout with children that has no siblings, is not a `ScrollView or a root layout, and does not have a background, can be remvoed for a flatter and more efficient layout hierarchy 

- Deep layouts - Layouts with too much nesting are bad for performance. Consider using flatter layouts such as RealtiveLayout or GridLayout to improve perofrmance. The default maximum depth is 10

Another benefit of Lint is that it is integrated into Android Studio. Lint automatically runs whenever you compile your program. With Android Studio, you can alo run lint inspections for a specific build variant, or for all build variants. 

YOu can also manage inspection profiles and configure inpsections within Android Studi9o with the File>setting>Project>Settings options. The instepection Configuration page appears with the supported inspections
Lint has the ability to automtically fix some issues, prvoide suggestions for others and jjmp directly to the offending code for review. 
