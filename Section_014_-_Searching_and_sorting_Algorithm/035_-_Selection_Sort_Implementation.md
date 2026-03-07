Here are implementations of the Selection Sort algorithm in various programming languages, categorized by programming paradigm and style for easier reference.

### Python and Ruby

```python
def selectionSort(arr):
    for i in range(len(arr) - 1):
        minIndex = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[minIndex]:
                minIndex = j
        if i != minIndex:
            arr[i], arr[minIndex] = arr[minIndex], arr[i]
    return arr
```


```ruby
def selectionsort(list)
  0.upto(list.size-2) do |start|
    min = start
    (start+1).upto(list.size-1) do |i|
      min = i if list[i] < list[min]
    end
    list[start], list[min] = list[min], list[start]
  end
  list
end
```


### C Family

```c
void selectionsort(int anzahl, int daten[]) {
    int i, k, t, min;
    for(i = 0; i < anzahl-1; i++) {
        min = i;
        for(k = i+1; k < anzahl; k++) {
            if(daten[k] < daten[min])
                min = k;
        }
        t = daten[min];
        daten[min] = daten[i];
        daten[i] = t;
    }
}
```


```cpp
void selectionsort(Comparable* A[], int n) {
  for (int i = 0; i < n-1; i++) {
    int bigindex = 0;
    for (int j = 1; j < n-i; j++)
      if (*A[j] > *A[bigindex])
        bigindex = j;
    swap(A, bigindex, n-i-1);
  }
}
```


```csharp
static void SelectionSort(ref List<int> feld) {
    for (int i = 0; i < feld.Count; i++) {
        int min = i;
        for (int j = i + 1; j < feld.Count; j++) {
            if (feld[j] < feld[min]) {    
                min = j;
            }
        }
        int tmp = feld[min];
        feld[min] = feld[i];
        feld[i] = tmp;
    }
}
```


```java
public static void selectionSort(int[] haystack) {
    for (int i = 0; i < haystack.length - 1; i++) {
        for (int j = i + 1; j < haystack.length; j++) {
            if (haystack[i] > haystack[j]) {
                int temp = haystack[i];
                haystack[i] = haystack[j];
                haystack[j] = temp;
            }
        }
    }
}
```


```go
func selectionSort(arr []int) []int {
    length := len(arr)
    for i := 0; i < length-1; i++ {
        min := i
        for j := i + 1; j < length; j++ {
            if arr[min] > arr[j] {
                min = j
            }
        }
        arr[i], arr[min] = arr[min], arr[i]
    }
    return arr
}
```


### Web Languages

```php
function selectionSort($array) {
    for ($i=0; $i<count($array); $i++) {
        $minpos=$i;
        for ($j=$i+1; $j<count($array); $j++) {
            if ($array[$j] < $array[$minpos]) {
                $minpos=$j;
            }
        }
        $tmp=$array[$minpos];
        $array[$minpos]=$array[$i];
        $array[$i]=$tmp;
    }
    return $array;
}
```


```javascript
// JavaScript implementation typically follows the same pattern
function selectionSort(arr) {
    for (let i = 0; i < arr.length - 1; i++) {
        let min = i;
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[min]) {
                min = j;
            }
        }
        if (min !== i) {
            [arr[i], arr[min]] = [arr[min], arr[i]];
        }
    }
    return arr;
}
```

### Pascal and Modula-2

```pascal
Procedure SelectionSort(Var Feld: Array Of LongInt);
Var
    i, j, Temp, Min: LongInt;
Begin
    For i := Low(Feld) To Pred(High(Feld)) Do
    Begin
        Min := i;
        For j := i+1 To High(Feld) Do
            If Feld[j] < Feld[Min] Then
                Min := j;
        Temp := Feld[Min];
        Feld[Min] := Feld[i];
        Feld[i] := Temp;
    End;
End;
```


```modula2
PROCEDURE SelectSort* ( VAR a : ARRAY OF INTEGER; n : INTEGER );
    VAR
        i, j, min, temp : INTEGER;
    BEGIN
        FOR i := 0 TO n-2 DO
            min := i;
            FOR j := i+1 TO n-1 DO
                IF a[j] < a[min] THEN min := j END
            END;
            IF min # i THEN
                temp := a[i]; a[i] := a[min]; a[min] := temp;
            END
        END
    END SelectSort;
```


### Functional Languages

```haskell
import Data.List (delete)

sSort :: (Ord a) => [a] -> [a]
sSort [] = []
sSort xs = m : sSort (delete m xs)
  where
    m = minimum xs
```


```scheme
(define selection_sort
  (lambda (l)
    (if (empty? l) 
        '()
        (let ((m (apply min l)))
          (cons m (selection_sort (remove m l)))
         ))))
```


### VBA for Excel

```vba
Sub SelectionSort()
    Dim Lowest As Integer, Temp As Integer, I As Integer, J As Integer
    Dim Feld As Variant
    Feld = Range("A1:A" & Range("A" & Rows.Count).End(xlUp).Row).Value
    
    For I = 1 To UBound(Feld) - 1
        Lowest = I
        For J = I + 1 To UBound(Feld)
            If Feld(J, 1) < Feld(Lowest, 1) Then
                Lowest = J
            End If
        Next J
        If Lowest <> I Then
            Temp = Feld(I, 1)
            Feld(I, 1) = Feld(Lowest, 1)
            Feld(Lowest, 1) = Temp
        End If
    Next I
    
    Range("B1").Resize(UBound(Feld, 1), 1).Value = Feld
End Sub
```


These implementations all follow the same core algorithm: finding the minimum (or maximum) element in the unsorted portion and swapping it into the correct position. The main differences lie in syntax conventions and language-specific features like list comprehensions in Python or functional patterns in Haskell.