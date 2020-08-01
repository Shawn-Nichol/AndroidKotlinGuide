A shader is a type of computer program that was originally used for shading(the productino of appropriate levels of light, darkness, and color witin an image), but which now performs a variety of specialized functions in various fields of computer graphics special effects.

In Android, shader defines the colors of the texture with chich the paint object should draw(other than a bitmap). Android defiens several subclasses of shader for Paint to use, such as BitmapShader, ComposeShader, LinearGradient, RadialGradient, and SweepGradient. 

## Shader
A shader defines teh texture ofr a Paint object. A subclass of Shader is installed in a Paint by calling paint.setShader(shader). After that, any object that is drawn with that Paint will get its colors from the shader. Android provides teh following subclassess of Shader for Paint to use

LinearGradient: draws a linear gradient using two or more given colors

RadialGradient: draws a radial gradient using the given colors, the center, and the radius. The colors are distributed between the center adn edge of the circle. 

SweepingGradient, draws a sweeping gradient around a center point with the specified colors. 

- ComposeShader is a composition of two shaders. ComposeShader and composing modes are beyond

- BitmapShader draws a bitmap drawable as a texture. The bitmap can be repeated or mirrored by setting the TileMode mode. You willl learn more about BitmapShader and TileMode later in this codelab. 

## Concept:  PorterDuff.Mode
The PorterDuff.Mode class provides several Alpha compositing and blending modes. Alpha compositing is the process of compositing a source image with a detination image to create the appearance of partial or full transparency. Transparency is defined by the alpha channel. The alpha channel represents the degree of transparency of a color, that is for its red, green and blue channesl

DST The source pixels are discarded, leaving the destination intact.

DST_ATOP The desetination pixels  that are not covered by source pixels are discarded. 

DST_IN The destination pixels that cover source pixels, are kept and the remaining source and destination pixels are dicarded

DST_OUT the destination pixels that are not covered by source are kept. 


## Tile Mode
The tileing mode, TileMode, defined in the Shader, specifies how the bitmap drawable is repeated or mirrored in the X and Y directions if the bitmap drawable being used for texture is smaller than the screen. Android provides three different ways to repeat(tile) the bitmap drawable. 

- Repeat: Repeats the bitmap shader's image horizontally and vertically.
- CLAMP: The edge colors will be used to fill the extra space outside of the shader's image bounds
- Mirror: The shader's image is mirroed horizontally and vertically. 
