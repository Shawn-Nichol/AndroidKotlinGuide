# What is DiffUtil
Diffutil is a utility class that calculates teh difference between two lists and ouputs a list of update operations that converts the first list into the second one. 

If can be used to calculate updates for a Recyclerview adapter. See ListAdapter and AsynclistDiffer which simply the use of DiffUtil on  a background thread. 

Diffutil uses Eugene W Myer's differnce algorith to calculate tthe minimal number of updates to ocnvert one list into another. Myer's algorith does not handle items that are moved so DiffUtil runs a seond pass on the result to dettect items that were moved. 

Note that DiffUtil, ListAdapter and AsynListDiffer requier the list to not mutate whil ein use. This generally means that both the lists themselvsee and thier elements  should not be modified idrectly. Instead, new lists should be provided any time content changes. It's common for lists passed to DifFUtil sto share elements that have not mutated, so it is not strictly required to reload all data to use DiffUtil

If the list are large, this operation may take significant time so youa re advised to run this on  backgroun thread, get eh DiffUtil.DiffResult then apply tit on the RecyclerView on the main Thread. 

The algorith is optimized for space and uses O(N) space to find the minmal number of addition and removal operations between the two lists. It has O(N + D^2) expected time perfromance where D is the length of the edit script. 

If move detection is enabled, it takes an additional O(MN) time wher M is the totatl number of added items and N is the total number or removed items. If your lists are already sorted by the same constrant, you can disable move detection to improve performance. 

## List Adapter
RecyclerView.ADapter base class for presenting List data in a RecyclerView, including computing diffs between lists on a background thread. 

This calss is a convenience wrapper around AsynListDiffer that impelemtns Adapter commoon default behavior for item acces and counting. 

While using a LiveData<List> is an easy way provide data to the adapter, it isn't required you ca use submitList(list when new lists are avaiable. 
