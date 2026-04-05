# Реализация стека и очереди

## Реализация стека

### Код реализации

```python
class Stack:
    def __init__(self):
        # список для хранения элементов
        self.items = []

    def push(self, value):
        # добавление элемента в стек
        self.items.append(value)

    def pop(self):
        # удаление верхнего элемента
        if self.is_empty():
            print("Стек пуст")
            return None
        return self.items.pop()

    def peek(self):
        # просмотр верхнего элемента без удаления
        if self.is_empty():
            print("Стек пуст")
            return None
        return self.items[-1]

    def is_empty(self):
        # проверка, пуст ли стек
        return len(self.items) == 0

    def size(self):
        # количество элементов
        return len(self.items)
```

Пример использования

```python
stack = Stack()

stack.push(10)
stack.push(20)
stack.push(30)

print(stack.peek())  # 30
print(stack.pop())   # 30
print(stack.size())  # 2
```

## Реализация очереди

Код реализации

```python
class Queue:
    def __init__(self):
        # список для хранения элементов
        self.items = []

    def enqueue(self, value):
        # добавление элемента в очередь
        self.items.append(value)

    def dequeue(self):
        # удаление элемента из начала очереди
        if self.is_empty():
            print("Очередь пуста")
            return None
        return self.items.pop(0)

    def front(self):
        # просмотр первого элемента
        if self.is_empty():
            print("Очередь пуста")
            return None
        return self.items[0]

    def is_empty(self):
        # проверка, пуста ли очередь
        return len(self.items) == 0

    def size(self):
        # количество элементов
        return len(self.items)
```

Пример использования

```python
queue = Queue()

queue.enqueue(10)
queue.enqueue(20)
queue.enqueue(30)

print(queue.front())   # 10
print(queue.dequeue()) # 10
print(queue.size())    # 2
```
