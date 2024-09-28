---
title: "Bit Manipulation"
permalink: /bitmanipulation
---

# Bit Manipulation

Operations involving bits are 

1. Get: get the bit at a position
2. Set: change a bit to 1
3. Clear: change a bit to 0
4. Update: update a bit to 0 or 1

### Bit Mask

Bit Masking is the process of manipulating the binary representation of a number. 
We use a number called the bit mask to do these operations.
A **Bit Mask** is simply the data used for bitwise operations.

**Q:** **Get** the bit at position i of n.
```
1. Take Bit Mask: 1<<i
2. Perform AND with n.
3. If the obtained number is non-zero, the bit at position i is 1.
4. Else, the bit is 0.
```

*How It Works*: 
```
Let n = abcd, i = 2
bitmask =  1<<2 = 0100
    abcd  &
    0100
    ----
    0b00 == 0 ? 0 : 1;
```

**Q:** **Set** the bit at position i of n.
```
1. Take Bit Mask: 1<<i 
2. Perform OR with n.
```
*How It Works*: 
```
Let n = abcd, i = 2
bitmask =  1<<2 = 0100
    abcd  |
    0100
    ----
    a1cd
```

**Q:** **Clear** the bit at position i of n.
```
1. Take Bit Mask: 1<<i 
2. Perform NOT(Bit Mask) AND n.
```
*How It Works*:
```
Let n = abcd, i = 2
bitmask =  1<<2 = 0100
~bitmask = 1011
    abcd  &
    1011
    ----
    a0cd
```

**Q:** **Update** the bit at position i of n to b.
```
1. If b is 0, do Clear Operation (AND + NOT)
2. If b is 1, do Set Operation (OR)
```

## Problems

### 1. Power of 2. [(LeetCode)](https://leetcode.com/problems/power-of-two/)

Check whether a number is a power of 2.
```
If n & n - 1 == 0, the number is a power of 2 (assuming n is positive).
```
*How It Works*:
```
A power of 2 has only one set bit
0001, 0010, 0100, 1000 etc.
Let
           n = 1000...
       n - 1 = 0111... (all bits set, except the MSB)
   n & n - 1 = 0000...
```

### 2. Count 1s

Count the number of set bits in a number.

**Brian Kerninghan's Algorithm**
```
count = 0
while n > 0:
    n = n & n - 1
    count += 1
return count
```
*How It Works:*
```
We can observe that (n & n - 1) removes the leftmost set bit from n.
For example
     n = ...10100
   n-1 = ...10011
   --------------
n&(n-1)= ...10000

Thus, each n & (n - 1) removes a single set bit from the number.
We just have to count how many times we can do this, before n becomes 0 (no set bits remaining).
```

### 3. Power Set [(LeetCode)](https://leetcode.com/problems/subsets/)

Generate all possible subsets of a set.

**Using Disjoint Set:**
```
initialize a list 'powerset'
for i from 0 to 2^n - 1:
    initialize current subset
    for j from 0 to n - 1:
        if the jth bit is set:
            add j to the current subset
    add current subset to the powerset
return powerset
```
* We can use (1 << n) to represent 2^n.

**Java Code**
```java
void getPowerSet(List<Integer> arr, List<List<Integer>> powerSet, int n) {
    for (int i = 0; i < (1 << n); i++) {
        List<Integer> subset = new ArrayList<>();
        for (int j = 0; j < n; j++)
            // Check if the jth bit in i is set
            if ((i & (1 << j)) != 0) 
                subset.add(arr.get(j));
        powerSet.add(subset);
    }        
}
``` 
*How it works:*
```
Let arr = {1, 2, 3}
The each subset and their representation using disjoint sets:
[] = {0, 0, 0}
[3] = {0, 0, 1}
[2] = {0, 1, 0}
[2, 3] = {0, 1, 1}
[1] = {1, 0, 0}
[1, 3] = {1, 0, 1}
[1, 2] = {1, 1, 0}
[1, 2, 3] = {1, 1, 1}

This is the binary representation of numbers from 0 to 2^n - 1
where n is the size of the set.
```

### 4. Find the unique number [(LeetCode)](https://leetcode.com/problems/single-number/)

Find the only number in an array which occurs twice.

**Using XOR**
```
xor_sum = 0
for num in array:
    xor_sum = xor_sum ^ num
return xor_sum
```
*How It Works:*
```
x ^ y = 0 whenever x = y
Hence, calculate cumulated XOR with each element.
All the numbers that occurs twice will be cancelled.
```

### 5. Find the 2 numbers that are unique [(LeetCode)](https://leetcode.com/problems/single-number-iii/)

```java
int xor = 0;
for (int num: nums)
    xor ^= num;
xor &= -xor;

int[] ans = new int[2];
int x = Integer.lowestOneBit(xor);
for (int num: nums)
    if ((num | x) == num) ans[0] ^= num;
    else ans[1] ^= num;
return ans;
```
*How It Works*:
```
The cumulated XOR will eliminate all duplicates
All that remains is a ^ b where a and b are the unique numbers
In the output, if a bit is 1, that means, a and b differ at that position.
There must be atleast one set bit, since the 2 numbers are different.
Choose the rightmost set bit as our bit.

If we only take the xor of all numbers with this bit set, we will get one of our numbers.
This is because the duplicated numbers get eliminated, and the second number we want is not in this group.
Similarly, XOR of all the remaining numbers will give us the second number.
```

### 6. Find the unique number 2 [(LeetCode)](https://leetcode.com/problems/single-number-ii/)

Find the only unique number in an array, where every other number occurs **thrice**.

```java
int n = nums.length;
int[] setBits = new int[32];
for (int num: nums) {
    int pos = 0;
    for (int i = 0; i < 32; i++) {
        if ((num & (1 << i)) != 0) 
            setBits[i]++;
    }
}
int result = 0;
for (int i = 0; i < 32; i++) {
    if (setBits[i] % 3 != 0) {
        result |= (1 << i);
    }
}
return result;
```
*How it Works:*
```
Let the array be {1, 2, 3, 1, 2, 3, 4}
Their binary representation is 
001
010
011
001
010
011
100
---
133 <- Sum of bits at each position
Since each number except one occurs thrice, 
the sum of bits at a position modulo 3 would be the bit at that position of our desired number.
This is because each number either contributes 0, or 3 to a position, except the unique number. 
The unique number may contribute 1, if the bit is set, or 0 otherwise.

133 -> 100 = 4

Thus, for each position, calculate the sum of bits % 3
Then construct the number from the stored values.
```


