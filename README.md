# Arrays

### Introduction

While you may be familiar with arrays, chances are you have not considered what happens our computer either manipulates an array by adding or removing elements, or retrieves information from an array.  In this lesson, we'll consider what happens when we retrieve or manipulate data in an array. 

### Arrays - under the hood

When we initialize an array in a programming language, the language 
allocates space in memory for your array, and then points that starting variable to that address in memory.  Then for each element the program assigns a fixed amount of memory for each element.  

![](https://docs.oracle.com/javase/tutorial/figures/java/objects-tenElementArray.gif)

So let's say my array say starts at memory address 100.  And assume that the programming language allocates 8 bits for each element in the array.  And that it allocates enough space for ten elements evenly spaced in memory.

Now, let's try to think through how a computer program retrieves an element at a specific index.

```javascript 
let myarr = ['a']

my_arr[0] = 'a'

```

Well, we first initialize the array and assign the letter 'a' as the first element, the programming language associates the letter "a" to a specific space in memory.  In our example, address 100.  So then, when when we call `my_arr[0]` all the program has to do is go to address 100, and retreive the element.

So now, do you think happens if we call `my_arr[3]`, to return what is in that slot.  If the `my_arr` begins at address 100, and we allocate eight bits of space for each, what address does the program go to to retreive the element at index 3?

Is there a formula that we can come up with for retrieval?  Yes, it's algebra.

```javascript
my_arr[3] 

100 + 3*8 = 124
```

![](https://upload.wikimedia.org/wikipedia/commons/8/89/Mail_boxes.jpg)



So our programming language knows that if eight bits are allocated to each element, and then to retrieve an element at a specific index, the program simply visits an address by using the following formula: 

* memoryLocationOfElement = arrayStartAddress + indexNumber * bit_allocation 

### Manipulating array elements

Now that we talked about retrieving elements from an array, let's talk about removing elements from an array.

```javascript
let arr = [1, 24, 48, 9]
arr.pop  
// 9

arr 
// [1, 2, 24, 48]
```
 
Performing an operation like pop, is fairly simple. Again let's assume that our array begins at memory address 100.

| memory address  |100  | 108  | 116 | 124 | 
| ----            |:---:| ----:|----:|----:|
| arr             |  1  |  24  |  48 | 9
| arr.pop         |  1  |  24  |  48 | X

So removing from the end of the array, is not so bad.  But removing an element from the beginning involves a lot more.  

| memory address  |100  | 108  | 116 | 124 | 
| ----            |:---:| ----:|----:|----:|
| arr             |  1  |  24  |  48 | 9
| arr.shift     | 24   |  48  | 9  | X


Looking at the chart above, shifting involves moving every remaining element to a new space in memory.  The cost is equal to the number of elements in the array.  So the time complexity of shifting is big O(n).  Note that to add elements to the beginning of the array also will cost big O (n) as every subsequent element would have to move to different spot in memory.

| memory address  |100  | 108  | 116 | 124 | 132
| ----            |:---:| ----:|----:|----:|----:|
| arr             |  1  |  24  |  48 | 9   |
| arr.unshift(5)  |  5   | 1   | 24  | 48  | 9

So unshifting is big O(n)and shifting is bigO(n).  While popping and finding elements take the same amount of time regardless the size of the array.  That is, the time complexity is big O (1), meaning that the cost of the operation does not depend on the number of elements in the array.

### Summary 
We saw in this section that some of the strengths and weaknesses of using an array.  Retrieving elements and add elements to the end of the array has a time complexity of big O(1), while adding or removing elements at the beginning of an array is big O(n).  But do not despair, there is alternative collection that is less costly for adding and removing elements from the beginning and that is a linked list.  We will learn about it in the next section.
