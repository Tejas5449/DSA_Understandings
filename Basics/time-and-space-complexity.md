1. What is Time Complexity?
When we write a program, we want to know:
How does the running time of my program change when the input becomes larger?

Suppose we have:
int[] numbers = {10, 20, 30, 40, 50};

If we want to find 30, we could check:
for (int i = 0; i < numbers.length; i++) {
    if (numbers[i] == 30) {
        System.out.println("Found");
    }
}

With 5 elements, this is very fast.

But imagine:
5 elements
   ↓
100 elements
   ↓
10,000 elements
   ↓
1,000,000 elements

The important question becomes:
How does the amount of work grow as the input grows?

That's what time complexity helps us describe.