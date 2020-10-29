## Glossary
Adapter: A subclass of RecyclerView.Adapter responsible for providing views that represent items in a data set. 

Position: The position of a data item within the Adapter

Index: The index of an attached child view as used in a call  to ViewGroup.getChildAt(int)Contrast with Position.

Binding: The process of preparing a child view to display data corresponding to a positoin within the adpter

Recycler(View: A view previously used to display data for a specified adapter position maay be placed in a cache for later reuse to dsiplay the same type of data again later. This can drastically improove performance by skipping initial layout inflattion of construction. 

Scrap(view): A child view that has entered into a temporarily  detached state during layout. Scrap views may be reused without becoming fully detached form the parent RecyclerView, either unmodified if no rebinding is require or modified by the adpater if the view was considered dirty. 

Dirty: A Achild view that must be rebound by the adapter before being dispplayed. 
