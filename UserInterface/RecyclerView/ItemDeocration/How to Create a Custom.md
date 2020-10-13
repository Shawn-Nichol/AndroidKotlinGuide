# How to Create a Custom Line Decoration
CustomItemDecoration allows you to set the spacing of the lineDecortor and use different colors depending on parameters, you can also display an image over the recycler view when a certain position in the RecyclerView is hit. 


## 1) Create a new Class
Create a new class that extends RecyclerView.ItemDecoration(), pass the context and override the following methods onDraw() and getItemOffSets() and onDrawOver() to allow configuration of the line and control spacing. 


```
class CustomItemDecoration(context: Context, private val redDivider: Drawable) : RecyclerView.ItemDecoration() {

  // If you aren't passing the drawable in the constructor, make sure include the not null operator. 
  private val blackDivider: Drawable = ContextCompat.getDrawable(context, R.drawable.rv_divider_black)!!

  override fun getItemOffsets(...) {
  
  }

  override fun onDraw(...) {
    ...
  }
  
  override fun onDrawOver(...) {
     ...
  }


```


## 2) getItemOffSet    
Retrieve an offsets for the given item. Each field of outRect specifies the number of pixels
that the item view should be inset by. The default, sets the view to 0. if you need to access Adapter
for additional data you can call RecyclerView.getChildAdapterPosition(view) to get the adapter position of the view.

rect: Rect to receive the output.
View: The child view to decorate.
parent: RecyclerView: RecyclerView this ItemDecoration is decoding.
state: RecyclerView.State: The current state of RecyclerView.
     

```
override fun getItemOffsets( rect: Rect, view: View, parent: RecyclerView, s: RecyclerView.State) {
  // Get the adapter positioon of the view, if there is no RV do nothing.
  val position = parent.getChildAdapterPosition(view)
    .let { 
      if(it == RecyclerView.NO_POSITION) return
      else it
    }

  parent.adapter?.let {adapter -> 
    /*
     * Rect holds the integer coordiantes ofr a rectangle. The rectangle is represented by the coordiantes of its 4 edges. These fields can be accessed directly.
     * Use width() and height() to retrieve the rectangle's width
     *
     * Note that the right and bottom coordiantes are exclusive. This means a Rect being drawn on bottom and right will move into the item on tright or bottom. 
     *
     * This rect holds space for padding. 
     */
    rect.bottom = when {
      position == adapter.itemCount -1 -> 0 // no Padding
      position < 2 -> 0 // no padding
      position % 2 != 0 -> 0 // no padding
      // Provides padding fore the height so the divider is evenly spaced between the two items in the recyclerview. 
      else -> redDivider.intrinsicHeight
    }  
  }
}
```
      
## 3) OnDraw()
Draw an appropriate decoration into the canvas supplied to the RecyclerView. Any content drawn by this method will be
drawn before the item views are drawn and will thus appear undereath the views.

c: Canvas to draw into
parent: RecyclerView this itemDeocration is drawing.
state: the current state of the RecyclerView.
     
```     
override fun onDraw(canvas: Canvas, parent: RecyclerView, state: RecyclerView.State) {
  // The rcyclerView item decoratis drawing
  parent.child
    // For each child item , becuase the top and bottom of every view is different. 
    .foreach { view -> 
      // get the adapter positio of the view. 
      val position = parent.getChildAdapterPosition(view)
        .let {
          if(it == ReccyerView.NO_POSITION) return
          else it
      }

      // Define the edges for Rect
      val left = 0
      // Bottom of the current view in the RecyclerView
      val top = view.bottom
      // Right doesn't adjust the view position of the next item in the RecyclerView.
      val right = parent.width
      // Bottom view doesn't adjust the start position of the next item in the RecyclerView. 
      val bottom = top + redDivider.instrinsicHeight

      if(position % 2 == 0) {
        // Specify a bounding rectangle for the drawable. This is where the drawable will be drawn when its draw() method is called
        blackDivider.bounds = Rect(left, top, right, bottom)
        // Draw in the rect bounds respectin optional effects such as alpha and color filter
        blackDivider.draw(canvas)
      } else {
        redDivider.bounds = Rect(left, top, right, bottom)
        redDivider.draw(canvas)
      }
}
```
    
## 4) OnDrawOver()    
Draw any approperaite decoration into the canvas supplied at the RecyclerView. Any content drawn by this method 
will be drawn after the item views are drawn and will thus appear over the views. 

c: Canvas to draw into
parent: RecyclerView this the itemDeocration its drawing into
state: the current state of RecyclerView.
     
override fun onDrawOver(c: Canvas, parent: RecyclerView, state: RecyclerView.State) {
    super.onDrawOver(c, parent, state)

    // The recyclerView this item decoration is drawing to
    parent.childern
      // This is done for every view becuase every view is different
      .forEach { view -> 
        val position = parent.getChildAdapterPosition(view)
        .let { 
          if(iit == RecyclerView.NO_POSITION) return
          else it
        }

        if(position > 20) {
        person.bounds = Rect(0, 0, parent.width, parent.height)
        person.draw(c)
      }
  }
}
```


## 5) Apply to RecyclerView
Add the customItemDecoration to the RecyclerView. 
```
val rv = findViewById<RecyclerView>(R.id.recycler_view)
rv.applly {
  ...
  val itemDecoration = CustomItemDecoration(context, context.getDrawable(R.drawable.ic_my_line)!!)
  addItemDecoration(itemDecoration)
}
```
