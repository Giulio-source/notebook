# Linear Search

**Complexity: O(N)** 

Loop through an array from the linearly from start to end

```js
/**
 * Linear search an array for a target value.
 * 
 * @param arr The array to search.
 * @param target The value to find in the array.
 * 
 * @returns True if the target is found in the array, false otherwise.
 */
export default function linear_search(
    arr: number[],
    target: number,
): boolean {
    for (let index = 0; index < arr.length; index++) {
        if (arr[index] === target) {
            return true;
        }
    }
    return false;
}
```

# Binary Search

**Complexity: O(logN)**

⚠️ This only works on ordered arrays ⚠️

Keep evaluating the middle element of an array and discarding one half.

```js
/**
 * Returns true if the target is found in the array, false otherwise.
 * @param arr The array to search.
 * @param target The value to find in the array.
 */
export default function bs_list(arr: number[], target: number): boolean {
    let min = 0; // The left index of the search interval.
    let max = arr.length - 1; // The right index of the search interval.

    while (min <= max) {
        const mid = Math.floor((min + max) / 2); // The middle index of the search interval.

        if (arr[mid] < target) { // If the middle value is less than the target, search to the right of mid.
            min = mid + 1;
        } else if (arr[mid] > target) { // If the middle value is greater than the target, search to the left of mid.
            max = mid - 1;
        } else { // If the middle value is equal to the target, the target is found and can be returned.
            return true;
        }
    }

    return false; // If the target is not found, return false.
}
```

# Bubble sort

**Complexity: O(N^2)**

```js
/**
 * Bubble sort an array of numbers.
 *
 * @param arr The array to sort.
 */
export default function bubble_sort(arr: number[]): void {
    // The index of the last unsorted element in the array.
    let unsortedUntilIndex = arr.length - 1;
    // A flag indicating whether the array is sorted or not.
    let isSorted = false;

    // While the array is not sorted...
    while (isSorted === false) {
        // Reset the sorted flag.
        isSorted = true;
        // Loop through all unsorted elements in the array...
        for (let i = 0; i < unsortedUntilIndex; i++) {
            // If the current element is greater than the next element...
            if (arr[i] > arr[i + 1]) {
                // The array is not sorted.
                isSorted = false;
                // Swap the current element with the next element.
                const temp = arr[i];
                arr[i] = arr[i + 1];
                arr[i + 1] = temp;
            }
        }
        // Decrease the index of the last unsorted element.
        unsortedUntilIndex--;
    }
}
```