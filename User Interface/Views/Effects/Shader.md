# Shader
A shader defines the texture of a Paint object. A subclass of Shader is installed in Paint by calling paint.setShader(shader). After that, any object that is drawn with that Paint will get its colors from the shader. Android provides the following subclasses of Shader for Paint to use.


A shader is a type of cmputer program that was originally used for shading( the production of appropriate levels of light, darkness, and color within an image), but which now performs a variety of specialized functions in various filedds of computer graphics special effects. In Android, shader defines the colors of the texture with which the paint object should draw(other than a bitmap). Android defines several subclasses of shader for Paint to use, such as BitmapShader, ComposeShader, LinearGradient, Radial Gradient and SweepGradient. 

## LinearGradient
Draws a linear gradient using two or more given colors

## RadialGradient
Draws a radial gradient using the given colors, the center, and the radius. The colors are distributed between the center and edge of the circle. 

## SweepingGradient
Draws a sweeping gradient around a center point with the specified colors.
- ComposeShader is a composition of two shaders. ComposeShader and composing modes are beyond
- BitmapShader draws a bitmap drawable as a texture. The bitmap can be repeated or mirrored by setting the TileMode. 


Create the bitmap in the init block
initialize the canvas objec with the new bitmap
create and intialize a Pain object
```
var bitmap: Bitmap

init {
  bitmap = Bitmap.createBitmap(spotlight.width, spotlight.height, Bitmap.Config.ARG_8888)
  canvas = Canvas(bitmap)
  val shaderPainnt = Paint(Paint.ANTI_ALIAS_FLAG)
}
```
