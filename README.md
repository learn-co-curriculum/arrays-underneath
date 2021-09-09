# Day 3: Underneath Arrays

## Learning Goals

- Explain how programming languages implement arrays
- Identify the runtime complexity of common array methods in Big O notation

## Introduction

While you may be familiar with arrays, chances are you have not considered what
happens when our computer either manipulates an array by adding or removing
elements, or retrieves information from an array. In this lesson, we'll discuss
what happens when we retrieve or manipulate data in an array.

Understanding how familiar data structures work under the hood will give you a
better sense of what's happening at a low level, as well as some clues as to how
to determine the Big O of algorithms that use these data structures.

## Arrays Under the Hood

When we initialize an array in a programming language, the language allocates
space in memory for your array, and then points that starting variable to that
address in memory. Then the program assigns a fixed amount of memory for each  
element.

![ten element array](https://s3.amazonaws.com/learn-verified/objects-tenElementArray.gif)

Let's say my array say starts at memory address 100. Assume that the programming
language allocates eight bits of memory for each element in the array, and that
it allocates enough space for ten elements evenly spaced in memory.

Now, let's try to think through how a computer program retrieves an element at a
specific index.

```javascript
let myAarr = ["a"];

myAarr[0] = "a";
```

When we initialize the array and assign the letter `"a"` as the first element,
the programming language assigns the letter `"a"` to a specific space in memory.
In our example, address 100. So then, when we call `myArr[0]` all the program
has to do is go to address 100, and retrieve the element.

So now, what do you think happens if we call `myArr[3]`, to return what is in
that slot. If the `myArr` begins at address 100, and we allocate eight bits of
space for each, what address does the program go to to retrieve the element at
index 3?

Is there a formula that we can come up with for retrieval? Yep!

```txt
100 + 3 * 8 = 124
```

Our programming language knows that if eight bits are allocated to each element,
and then to retrieve an element at a specific index, the program simply visits
an address by using the following formula:

```txt
memoryLocationOfElement = arrayStartAddress + indexNumber * bitAllocation
```

### Manipulating Array Elements

Now that we talked about retrieving elements from an array, let's talk about
removing elements from an array.

```javascript
let arr = [1, 24, 48, 9];
arr.pop;
// 9

arr;
// [1, 24, 48]
```

Performing an operation like `pop` is fairly simple. Let's assume that our array
begins at memory address 100:

| memory address | 100 | 108 | 116 | 124 |
| -------------- | --- | --- | --- | --- |
| `arr`          | 1   | 24  | 48  | 9   |
| `arr.pop`      | 1   | 24  | 48  | X   |

Removing from the end of the array is not so bad. But removing an element from
the beginning involves a lot more:

| memory address | 100 | 108 | 116 | 124 |
| -------------- | --- | --- | --- | --- |
| `arr`          | 1   | 24  | 48  | 9   |
| `arr.shift`    | 24  | 48  | 9   | X   |

Looking at the chart above, shifting involves moving **every** remaining element
to a new space in memory. The cost is equal to the number of elements in the
array. So the time complexity of shifting is Big O(n). Adding elements to the
beginning of the array also will cost Big O(n), as every subsequent element would
have to move to different spot in memory:

| memory address   | 100 | 108 | 116 | 124 | 132 |
| ---------------- | --- | --- | --- | --- | --- |
| `arr`            | 1   | 24  | 48  | 9   | X   |
| `arr.unshift(5)` | 5   | 1   | 24  | 48  | 9   |

So `unshift` is Big O(n) and `shift` is Big O(n). However, `pop` and accessing
elements using their index positions take the same amount of time regardless the
size of the array. That is, the time complexity is Big O(1), meaning that the
cost of the operation does not depend on the number of elements in the array.

### Array Size

Remember that to retrieve information from an array, we simply need to apply the
formula `startingAddress + index * bitAllocation` and go to the corresponding
address. One problem that occurs with having all of these contiguous elements
is that we must allocate a specific amount of space — say enough space for six
elements. What happens when we want to add an seventh element?

| memory address | 100 | 108 | 116 | 124 | 132 | 140      |
| -------------- | --- | --- | --- | --- | --- | -------- |
| `arr`          | 1   | 24  | 48  | 9   | 32  | song.mp3 |
| `arr.push(5)`  | 1   | 24  | 48  | 9   | 32  | song.mp3 |

Do you see our problem? We want to push another element, but something else is
on those eight bits. If we move our new element to a different location, our
formula for retrieving elements no longer works. Instead, we copy our array into
a new location in memory where there is enough space. However, notice that the
cost of doing this is Big O(n), as we must incur a cost for each element we copy
over.

| new memory address | 300 | 308 | 316 | 324 | 332 | 340 |
| ------------------ | --- | --- | --- | --- | --- | --- |
| `arr`              | 1   | 24  | 48  | 9   | 32  | X   |
| `arr.push(5)`      | 1   | 24  | 48  | 9   | 32  | 5   |

### Summary

We saw in this section that some of the strengths and weaknesses of using an
array. Retrieving elements by index and adding elements to the end of the array
has a time complexity of Big O(1), while adding or removing elements at the
beginning of an array is Big O(n). We also saw that because operations in our
array rely on using neighboring locations in memory, we can run out of space.

Here's a summary of the Big O of common array methods:

| Method                | Big O |
| --------------------- | ----- |
| Access                | O(1)  |
| Search                | O(n)  |
| Insertion (end)       | O(1)  |
| Insertion (beginning) | O(n)  |
| Deletion (end)        | O(1)  |
| Deletion (beginning)  | O(n)  |

For problems that rely on adding or removing elements from the beginning, you're
better off using a linked list, as we saw in a previous lesson. Arrays are quick
for accessing elements when you know their index position, and for adding and
removing items from the end.
