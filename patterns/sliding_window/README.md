# Sliding Window Pattern

## What is the Sliding Window Pattern?

The Sliding Window pattern is a technique used to process arrays or lists in a sequential manner by maintaining a "window" of elements. This window can grow, shrink, or slide as needed to solve the problem efficiently.

## When to Use Sliding Window?

Use the Sliding Window pattern when you need to:
- Process contiguous subarrays or sublists
- Find maximum/minimum subarrays of a specific size
- Find subarrays that meet certain conditions
- Calculate running averages or sums

## Types of Sliding Window

### 1. Fixed Size Window

In this variation, the window size remains constant throughout the traversal.

![Fixed Size Window](./images/fixed_window.png)

**Use cases:**
- Find maximum/minimum sum subarray of size K
- Calculate moving average of size K
- Find all subarrays of size K with a specific property

### 2. Variable Size Window

In this variation, the window size can grow or shrink based on certain conditions.

![Variable Size Window](./images/variable_window.png)

**Use cases:**
- Find smallest subarray with sum greater than or equal to S
- Find longest substring with K distinct characters
- Find longest substring without repeating characters

## How It Works

1. Start with a window of initial size (often 0 or 1)
2. Expand the window by adding elements from the right
3. When a certain condition is met, contract the window from the left
4. Keep track of the answer during this process

![Sliding Window Animation](./images/sliding_window_animation.gif)

## Code Templates

### Fixed Size Window

#### Python
```python
def fixed_sliding_window(arr, k):
    # Initialize variables
    window_sum = sum(arr[:k])
    result = window_sum
    
    # Slide the window
    for i in range(k, len(arr)):
        # Add the next element and remove the first element of the previous window
        window_sum = window_sum + arr[i] - arr[i-k]
        result = max(result, window_sum)  # or min, depending on the problem
    
    return result
```

#### Java
```java
public int fixedSlidingWindow(int[] arr, int k) {
    // Initialize variables
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    int result = windowSum;
    
    // Slide the window
    for (int i = k; i < arr.length; i++) {
        // Add the next element and remove the first element of the previous window
        windowSum = windowSum + arr[i] - arr[i-k];
        result = Math.max(result, windowSum);  // or Math.min, depending on the problem
    }
    
    return result;
}
```

### Variable Size Window

#### Python
```python
def variable_sliding_window(arr, target):
    window_start = 0
    window_sum = 0
    min_length = float('inf')  # or 0, depending on the problem
    
    for window_end in range(len(arr)):
        window_sum += arr[window_end]  # Add the next element
        
        # Shrink the window as small as possible until the condition is no longer met
        while window_sum >= target:  # Condition depends on the problem
            min_length = min(min_length, window_end - window_start + 1)
            window_sum -= arr[window_start]
            window_start += 1
    
    return min_length if min_length != float('inf') else 0
```

#### Java
```java
public int variableSlidingWindow(int[] arr, int target) {
    int windowStart = 0;
    int windowSum = 0;
    int minLength = Integer.MAX_VALUE;  // or 0, depending on the problem
    
    for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
        windowSum += arr[windowEnd];  // Add the next element
        
        // Shrink the window as small as possible until the condition is no longer met
        while (windowSum >= target) {  // Condition depends on the problem
            minLength = Math.min(minLength, windowEnd - windowStart + 1);
            windowSum -= arr[windowStart];
            windowStart++;
        }
    }
    
    return minLength == Integer.MAX_VALUE ? 0 : minLength;
}
```

## Example Problems

### 1. Maximum Sum Subarray of Size K

**Problem:** Find the maximum sum of any contiguous subarray of size K.

**Solution:**

```python
def max_sum_subarray(arr, k):
    max_sum = 0
    window_sum = 0
    
    for i in range(len(arr)):
        window_sum += arr[i]
        
        if i >= k - 1:
            max_sum = max(max_sum, window_sum)
            window_sum -= arr[i - (k - 1)]
    
    return max_sum
```

### 2. Smallest Subarray with Sum Greater than or Equal to S

**Problem:** Find the length of the smallest contiguous subarray whose sum is greater than or equal to S.

**Solution:**

```python
def smallest_subarray_with_given_sum(arr, s):
    window_sum = 0
    min_length = float('inf')
    window_start = 0
    
    for window_end in range(len(arr)):
        window_sum += arr[window_end]
        
        while window_sum >= s:
            min_length = min(min_length, window_end - window_start + 1)
            window_sum -= arr[window_start]
            window_start += 1
    
    return min_length if min_length != float('inf') else 0
```

### 3. Longest Substring with K Distinct Characters

**Problem:** Find the length of the longest substring with at most K distinct characters.

**Solution:**

```python
def longest_substring_with_k_distinct(s, k):
    window_start = 0
    max_length = 0
    char_frequency = {}
    
    for window_end in range(len(s)):
        right_char = s[window_end]
        if right_char not in char_frequency:
            char_frequency[right_char] = 0
        char_frequency[right_char] += 1
        
        while len(char_frequency) > k:
            left_char = s[window_start]
            char_frequency[left_char] -= 1
            if char_frequency[left_char] == 0:
                del char_frequency[left_char]
            window_start += 1
        
        max_length = max(max_length, window_end - window_start + 1)
    
    return max_length
```

## LeetCode Problems Using Sliding Window

1. [Maximum Subarray (LeetCode #53)](https://leetcode.com/problems/maximum-subarray/)
2. [Minimum Size Subarray Sum (LeetCode #209)](https://leetcode.com/problems/minimum-size-subarray-sum/)
3. [Longest Substring Without Repeating Characters (LeetCode #3)](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
4. [Longest Substring with At Most K Distinct Characters (LeetCode #340)](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
5. [Fruit Into Baskets (LeetCode #904)](https://leetcode.com/problems/fruit-into-baskets/)
6. [Permutation in String (LeetCode #567)](https://leetcode.com/problems/permutation-in-string/)

## Time and Space Complexity

- **Time Complexity**: O(N) where N is the size of the input array/string. In the worst case, each element might be processed twice (once when it enters the window and once when it exits).
- **Space Complexity**: O(1) for fixed-size window problems with simple data. O(K) for problems involving distinct elements, where K is the size of the window or the number of distinct elements.

## Tips and Tricks

1. Always check if the window size is valid before processing
2. Be careful with window boundaries (off-by-one errors)
3. For string problems, use a hash map to track character frequencies
4. For fixed window problems, you can optimize by just adding the new element and removing the oldest one
5. For problems requiring all subarrays/substrings, the time complexity might be higher (e.g., O(N²))
