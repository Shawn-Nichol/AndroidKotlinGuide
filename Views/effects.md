A shader is a type of computer program that was originally used for shading(the productino of appropriate levels of light, darkness, and color witin an image), but which now performs a variety of specialized functions in various fields of computer graphics special effects.

In Android, shader defines the colors of the texture with chich the paint object should draw(other than a bitmap). Android defiens several subclasses of shader for Paint to use, such as BitmapShader, ComposeShader, LinearGradient, RadialGradient, and SweepGradient. 

## Shader
A shader defines teh texture ofr a Paint object. A subclass of Shader is installed in a Paint by calling paint.setShader(shader). After that, any object that is drawn with that Paint will get its colors from the shader. Android provides teh following subclassess of Shader for Paint to use

LinearGradient: draws a linear gradient using two or more given colors

RadialGradient: draws a radial gradient using the given colors, the center, and the radius. The colors are distributed between the center adn edge of the circle. 

SweepingGradient, draws a sweeping gradient around a center point with the specified colors. 

- ComposeShader is a composition of two shaders. ComposeShader and composing modes are beyond

- BitmapShader draws a bitmap drawable as a texture. The bitmap can be repeated or mirrored by setting the TileMode mode. You willl learn more about BitmapShader and TileMode later in this codelab. 

Concept: 
