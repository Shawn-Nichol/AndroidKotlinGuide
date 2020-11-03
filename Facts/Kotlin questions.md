### What is the difference betweeen a data class and normal class?
The compiler will automatically generat getters and setteres for data classes and also the equals() hashCode(), toString() and copy() function.

### What is the difference between val and const val?
Variables marked as const val will have their values assigned at compile time, variables are delcared as val at runtime. 

### What is the (better) equivalent to javas swicth statement in kotlin?\
When expression

### When writing an extension function , will the compiler put hte function inot the class you estended?
No, Extension functions only exist for better readability. They do not modify the class they extend.

### What is the easiest way to implement a singleton?
By creating an object instead of a class. 

### What is the differnece between a coroutine and a thread?
Coroutines run inside of threads and are more lightweight. THey can quickly swithc the thread they are running in. 

### What is Smart cast Feature?
When you check once if an object is an instance of specific class, you don't need to convert it to that class afterwards, you can directly access the properties of the class you checked for. 

### What is an inline function?
Inline functions aren't real functions with their own addres in memory. Instead the compiler copies their code to wherever you call them. 
