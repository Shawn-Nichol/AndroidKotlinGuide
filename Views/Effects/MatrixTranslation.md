# Matrix Translation
When the user touches and holds the screen for the spotligh, instead of calculating where the spotlight needs to be drawn, you move the shader matrix; that is, the texture/shader coordinate system, and then draw the texture at the same location in the translated coordinate system. The resulting effect will seem as if you are drawing the spotlight texture at a different location, which is the same as the ShaderMatrix translated location. This is simpler and slightly more efficient. 

