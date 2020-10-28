# Concept PoreterDuff.Mode

The PorterDuff.Mode class provides several Alpha compositing and blending modes. Alpha compositing is the process of compositing a source image with an image to create the appearance of partial or full transparaency. Transparency is deinfed by the alpha channel. The alpha channel represents the degree of transparency of a color. 

- DST: The source pixels are dicarded, leaving the destination intact.
- DST_ATOP: The destination pixesl that are not covered by source pixels are discarded. 
- DST_IN: The destination pixels that cover source pixels, are kept and the remaining source and destination pixels are discarded

- DST_OUT: THe destination pixels that are not covered by the source are kept. 
```
paint.xfermode = PoterterDuffXfermode(PorterDuff.Mode.DST_OUT)
canvas.drawBitmap(spotlight, 0.0f, 0.0f, paint)
```

