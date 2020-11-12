## What is a list?
A list can save pieces of data in strict order, becuase the list is dynamic you can add and remove items. 

## Array vs List
Arrays are not dynamic, becuase of this we know the place of element. 

Lists are dynamic, we have the advantage of changing it at runtime, to read an element you have to loop through the whole listin the worste case. 

## How a list saves data. 
Each item in a list not only contians the data it should hold, but also a pointer to the next item of the list. This way, it doesn't matter where the single items are saved in memory. 

## Adding items
When adding an item, we must loop over the list till the last item we want to add then asign pointers. 

## Removing item
Remvoign cna be tricky becuase the pointers need to be rearragned if it's  not the last itme. 
