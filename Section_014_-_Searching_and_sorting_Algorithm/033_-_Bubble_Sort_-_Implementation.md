Here are implementations of Bubble Sort in common programming languages.

## Python Implementation

```python
def bubble_sort(arr):
    n = len(arr)
    # Traverse through all array elements
    for i in range(n):
        # Flag to optimize: if no swaps, array is sorted
        swapped = False
        # Last i elements are already in place
        for j in range(0, n - i - 1):
            # Compare adjacent elements
            if arr[j] > arr[j + 1]:
                # Swap if they are in wrong order
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        # If no swaps occurred, array is sorted
        if not swapped:
            break
    return arr

# Example usage
sample_list = [64, 34, 25, 12, 22, 11, 90]
sorted_list = bubble_sort(sample_list)
print("Sorted array:", sorted_list)
```

## C Implementation

```c
#include <stdio.h>
#include <stdbool.h>

void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        // If no swaps, array is sorted
        if (!swapped)
            break;
    }
}

int main() {
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(arr) / sizeof(arr[0]);
    bubbleSort(arr, n);
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");
    return 0;
}
```

## Java Implementation

```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        boolean swapped;
        for (int i = 0; i < n - 1; i++) {
            swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            // If no swaps, array is sorted
            if (!swapped) break;
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};
        bubbleSort(arr);
        System.out.print("Sorted array: ");
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

## JavaScript Implementation

```javascript
function bubbleSort(arr) {
    let n = arr.length;
    let swapped;
    for (let i = 0; i < n - 1; i++) {
        swapped = false;
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
                swapped = true;
            }
        }
        // If no swaps, array is sorted
        if (!swapped) break;
    }
    return arr;
}

// Example usage
let sampleArray = [64, 34, 25, 12, 22, 11, 90];
console.log("Sorted array:", bubbleSort(sampleArray));
```

All implementations follow the optimized version with a `swapped` flag to detect an already sorted array. The algorithm sorts in-place, so the original array is modified.