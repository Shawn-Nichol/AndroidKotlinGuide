# Create resizable bitmaps

The Draw 9-patch tool is a WYSIWYG editor included in Android Studio that allows you to create bitmap images that automatically resize to accommodate the contents of the view and the size of the screen. Selected parts of the image scaled horizontally or vertically based on indicators drawn within the image. 

Quick Guide
1. Righ-click the PNG image you'd like to cretae a NinPatch image from, then click Create 9-patch file
2. Type a file name
3. Open the NinePatch file. The left pane is your drawing area, in which you can edit the lines for the stretchable patches and content area. The right pane is the preview area. 
4. Click within the 1 pixel perimeter to draw the lines that defien the stretchable patches and optional content area. Right-click to erase previously drawn lines
5. Save

To make sure that your NinPatch graphics scale down properly, verify that any stretchable regions are at least 2X2 pixels in size. Otherwise, they may disappear when scaled down. Also , provide one pixel of extra safe space in the graphics before and after stretchable regions to avoid interpolation during scaling that may cuase the color at the boundaries to change. 

