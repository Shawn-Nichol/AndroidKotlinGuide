# How Property annimation differs from view animation

## View Animation
View animation is only able to animate views. 

Disavanatage
- You cannot animate properties such as color. 
- It is only modified where the View was drawn and not the acutal View itself. If a button moves to a new location a click will not take place at the new location, instead it will take place at the orignal draw location. 

Advantages
- The view animation takes less time to setup and requires less code. to write. 

## Property Animation
Property animation allows you to animate any object. Disavantages
- Takes longer to setup and has more code. 

Advantages
- You can animate any object and property. 
- The object is modifed, which means you would click the button that has moved instead of the space where the button was before it moved. 
- It is more robuts, you can assign animators to the properties that you want such as color position, or size and can define aspects of the animation such as interpolcation and syncronaization of multiple animators. 


## Summary
The View animation system, however, takes less time to setup and requries less code to write. If view animation accomplishes everyting that you need to do, or if yourexisting code already works the way you want, there i s no need to use the property animation system. It also might make sense to use both animation systems for different situations if the use case arises. 
