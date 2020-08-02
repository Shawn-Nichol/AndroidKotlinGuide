# Tile Mode
The tiling mode, TileMode, defined in the Shader, specifies how the bitmap drawable is repeated or mirrored in teh X and Y directions if the bitmap drawable being used for texture is smaller than the screen. Android provides three different ways to repeat(tile the bitmap drawable. 

- Repeat: Repeats the bitmap shader's image horizontally and vertically. 
- CLAMP: The edge colors will be used to fill the extra space outside of the shader's image bounds 
- Mirror: the shader's image is mirroed horizontally and vertically. 


## How to intialize
- Declare a private lateinit class variable of type shader
- Private var shader: Shader
- In the init block create a bitmap shader using the constructor, BitmapShader(). pass the texture bitmap and tiling mode. 

```
private lateinit var shader: Shader

init {
  val vitmap = Bitmap.createBitmap(spotlight.width, spotlight.height, Bitmap.Config.ARG_8888)

  shader = BitmapShader(bitmap, Shader.TileMode.REPEAT, Shader.TileMode.REPEAT)
  paint.shader = shader
}
```
