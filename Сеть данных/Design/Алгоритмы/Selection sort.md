# Selection sort
*Need to know:*[[Структуры данных введение]]
*tags:* #maths #binarMaths #python #analysis #dataScience #java
*description:* Сортировка массива выбором. Алгоритм проходиться по всем значениями, находит самое большое (маленькое) и вставляет его в новый отсортированный массив.

---
## Характеристика
**Входные данные:** Массив с однотипными значениями
**Время выполнения:** `O(n^2)`


## Code:
Python:
```py
# Finds the smallest value in an array
def findSmallest(arr):
  # Stores the smallest value
  smallest = arr[0]
  # Stores the index of the smallest value
  smallest_index = 0
  for i in range(1, len(arr)):
    if arr[i] < smallest:
      smallest = arr[i]
      smallest_index = i
  return smallest_index

# Sort array
def selectionSort(arr):
  newArr = []
  for i in range(len(arr)):
      # Finds the smallest element in the array
	  #and adds it to the new array
      smallest = findSmallest(arr)
      newArr.append(arr.pop(smallest))
  return newArr

print selectionSort([5, 3, 6, 2, 10])
```