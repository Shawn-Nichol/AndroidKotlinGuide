## What is a map?
A map is an abstract data type (an interface) that saves its data in key-value pairs. Each key must be unique. 

## What is a Hashmap?
- A Hasphmap is a data structure, it is concrete implementation of a map. So a Hashmap defines how eh operations of a map are implemented.
- There is not a single way for a map to store data, but a HashMap implements such a way. 
- A HashMap compbines the quick information retrieval of  an array with the item insertion of a list. 

## How does it work? 
- When you insert a new item into a HashMap, it calculates the key by using its HashCOde() function
- Then it calculates the index that key will have(remember, internally a HashMap uses an array for quick access). If the array is too small, it'll double its size. 

## What if a index exists?
This called a hash collision. In that case the HashMap needs to solve this. There are several ways to resolve this. 

## Reading a HashMap?
- This is what makes a HashMap so performant. Reading is much faster than from a list. 
- It only needs to calculate hashCoe() of the key we want to read and we already got the index we can access it at. 

## When to use HashMap?
- Use it if you need to save huge amounts of data and need quick lookups
- Don't use it, if you neeed your entries to have order and if you highly rely on memory consumption. Then instead prefer a list. 
