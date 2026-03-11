# Dojo quiz

Ниже приведены фрагменты кода на Python.

Для каждого примера попробуйте определить **асимптотическую сложность по времени**.

---

### Пример 1

```python
def example_1(arr):
    total = 0
    for x in arr:
        total += x
    return total
```

### Пример 2
```python
def example_2(arr):
    n = len(arr)

    for i in range(n):
        for j in range(i):
            print(arr[i], arr[j])
```

### Пример 3

```python
def example_3(n):
    i = 1
    while i < n:
        print(i)
        i *= 2
```

### Пример 4
```python
def example_4(arr):
    n = len(arr)

    for i in range(n):
        for j in range(n):
            if i == j:
                print(arr[i])
```

### Пример 5
```python
def example_5(arr):
    s = set()

    for x in arr:
        if x in s:
            return True
        s.add(x)

    return False
```

### Пример 6
```python
def example_6(arr):
    n = len(arr)

    for i in range(n):
        j = 1
        while j < n:
            j *= 2
```

### Пример 7
```python
def example_7(arr):
    n = len(arr)

    for i in range(n):
        for j in range(i * i):
            print(i, j)
```

### Пример 8
```python
def example_8(n):
    if n <= 1:
        return 1

    return example_8(n-1) + example_8(n-1)
```

### Пример 9
```python
def example_9(arr):
    n = len(arr)

    i = 0
    while i < n:
        for j in range(n):
            print(arr[j])
        i *= 2 if i > 0 else 1
        i += 1
```

### Пример 10
```python
def example_10(arr):
    n = len(arr)

    for i in range(n):
        j = i
        while j > 0:
            j //= 2
```
