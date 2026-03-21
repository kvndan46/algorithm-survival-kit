# Связные списки (Linked Lists) с точки зрения Computer Science

## 1. Определение связного списка

**Связный список (Linked List)** — это структура данных, состоящая из **узлов (nodes)**, где каждый узел содержит:
- **Данные** — хранимое значение
- **Ссылку (ссылку/указатель)** на следующий узел (и, возможно, на предыдущий)

В отличие от массива, узлы **не хранятся в памяти непрерывно** — они могут быть разбросаны по всей памяти, связанные между собой ссылками.

```python
# Простейшая реализация узла односвязного списка
class Node:
    def __init__(self, data):
        self.data = data      # данные
        self.next = None      # ссылка на следующий узел

# Создаем три узла
node1 = Node(10)
node2 = Node(20)
node3 = Node(30)

# Связываем их
node1.next = node2
node2.next = node3

# Теперь node1 → node2 → node3 → None
print(node1.data)        # 10
print(node1.next.data)   # 20
print(node1.next.next.data)  # 30
```

---

## 2. Свойства linkedList

### 2.1. Непрерывность vs Фрагментация

**Главное отличие от массива:** узлы связного списка **не обязаны** храниться в памяти последовательно.

```python
import sys

# Создаем массив и список для сравнения
array = [10, 20, 30]
linked_nodes = [Node(10), Node(20), Node(30)]
linked_nodes[0].next = linked_nodes[1]
linked_nodes[1].next = linked_nodes[2]

print("Массив — адреса элементов:")
for x in array:
    print(f"  {x}: {id(x)}")
# Вывод: адреса близки и идут подряд

print("\nСвязный список — адреса узлов:")
for node in linked_nodes:
    print(f"  {node.data}: {id(node)}")
# Вывод: адреса могут быть разбросаны по памяти
```

### 2.2. Динамический размер

В отличие от статического массива, связный список может **расти и уменьшаться** без необходимости копирования всех элементов.

```python
class LinkedList:
    def __init__(self):
        self.head = None
        self.size = 0

    def append(self, data):
        """Добавление в конец — O(n) для односвязного списка"""
        new_node = Node(data)
        if not self.head:
            self.head = new_node
        else:
            current = self.head
            while current.next:
                current = current.next
            current.next = new_node
        self.size += 1

    def prepend(self, data):
        """Добавление в начало — O(1)"""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
        self.size += 1

# Демонстрация динамического размера
lst = LinkedList()
print(f"Размер: {lst.size}")  # 0

for i in range(10):
    lst.append(i)
print(f"После добавления 10 элементов: {lst.size}")  # 10

lst.prepend(-1)
print(f"После добавления в начало: {lst.size}")  # 11
```

---

## 3. Виды связных списков

### 3.1. Односвязный список (Singly Linked List)

**Структура:** каждый узел хранит ссылку только на следующий узел.


```python
class SinglyNode:
    def __init__(self, data):
        self.data = data
        self.next = None

class SinglyLinkedList:
    def __init__(self):
        self.head = None

    def insert_at_beginning(self, data):
        """O(1)"""
        new_node = SinglyNode(data)
        new_node.next = self.head
        self.head = new_node

    def insert_at_end(self, data):
        """O(n) — нужно пройти до конца"""
        new_node = SinglyNode(data)
        if not self.head:
            self.head = new_node
            return

        current = self.head
        while current.next:
            current = current.next
        current.next = new_node

    def delete_at_beginning(self):
        """O(1)"""
        if self.head:
            self.head = self.head.next

    def delete_by_value(self, value):
        """O(n) — нужно найти и удалить"""
        if not self.head:
            return

        # Если удаляем голову
        if self.head.data == value:
            self.head = self.head.next
            return

        # Ищем удаляемый узел
        current = self.head
        while current.next and current.next.data != value:
            current = current.next

        if current.next:
            current.next = current.next.next

    def find(self, value):
        """O(n) — линейный поиск"""
        current = self.head
        while current:
            if current.data == value:
                return True
            current = current.next
        return False

    def display(self):
        """Вывод всех элементов"""
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))
            current = current.next
        print(" → ".join(elements) + " → None")

# Демонстрация
sll = SinglyLinkedList()
sll.insert_at_beginning(30)
sll.insert_at_beginning(20)
sll.insert_at_beginning(10)
sll.display()  # 10 → 20 → 30 → None

sll.insert_at_end(40)
sll.display()  # 10 → 20 → 30 → 40 → None

sll.delete_by_value(20)
sll.display()  # 10 → 30 → 40 → None
```

**Характеристики односвязного списка:**

| Операция | Сложность | Примечание |
|----------|-----------|------------|
| Вставка в начало | O(1) | Просто меняем head |
| Вставка в конец | O(n) | Нужно пройти до конца |
| Вставка после заданного узла | O(1) | Если есть ссылка на узел |
| Удаление с начала | O(1) | Просто меняем head |
| Удаление по значению | O(n) | Нужно найти и предыдущий узел |
| Поиск | O(n) | Линейный проход |
| Доступ по индексу | O(n) | Нужно пройти до индекса |

**Ограничение:** можно двигаться только **вперед** (однонаправленная навигация).

---

### 3.2. Двусвязный список (Doubly Linked List)

**Структура:** каждый узел хранит ссылки **на следующий** и **на предыдущий** узлы.

```python
class DoublyNode:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None

class DoublyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None      # ссылка на последний узел для удобства
        self.size = 0

    def insert_at_beginning(self, data):
        """O(1)"""
        new_node = DoublyNode(data)
        if not self.head:
            self.head = self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node
        self.size += 1

    def insert_at_end(self, data):
        """O(1) — благодаря ссылке на tail"""
        new_node = DoublyNode(data)
        if not self.tail:
            self.head = self.tail = new_node
        else:
            new_node.prev = self.tail
            self.tail.next = new_node
            self.tail = new_node
        self.size += 1

    def delete_at_beginning(self):
        """O(1)"""
        if not self.head:
            return None

        data = self.head.data
        self.head = self.head.next

        if self.head:
            self.head.prev = None
        else:
            self.tail = None

        self.size -= 1
        return data

    def delete_at_end(self):
        """O(1) — благодаря ссылке на tail"""
        if not self.tail:
            return None

        data = self.tail.data
        self.tail = self.tail.prev

        if self.tail:
            self.tail.next = None
        else:
            self.head = None

        self.size -= 1
        return data

    def delete_by_value(self, value):
        """O(n) — нужно найти узел, но удаление O(1)"""
        current = self.head
        while current:
            if current.data == value:
                # Связываем предыдущий и следующий узлы
                if current.prev:
                    current.prev.next = current.next
                else:
                    self.head = current.next

                if current.next:
                    current.next.prev = current.prev
                else:
                    self.tail = current.prev

                self.size -= 1
                return True
            current = current.next
        return False

    def display_forward(self):
        """Вывод от начала к концу"""
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))
            current = current.next
        print(" ⇄ ".join(elements))

    def display_backward(self):
        """Вывод от конца к началу (уникальная возможность двусвязного списка)"""
        elements = []
        current = self.tail
        while current:
            elements.append(str(current.data))
            current = current.prev
        print(" ⇄ ".join(elements))

# Демонстрация
dll = DoublyLinkedList()
dll.insert_at_end(10)
dll.insert_at_end(20)
dll.insert_at_end(30)
dll.insert_at_beginning(5)

print("Прямой порядок:")
dll.display_forward()    # 5 ⇄ 10 ⇄ 20 ⇄ 30

print("Обратный порядок:")
dll.display_backward()   # 30 ⇄ 20 ⇄ 10 ⇄ 5

dll.delete_by_value(20)
print("После удаления 20:")
dll.display_forward()    # 5 ⇄ 10 ⇄ 30
```

**Преимущества двусвязного списка:**
- Можно двигаться в **обоих направлениях**
- Удаление узла возможно **без знания предыдущего** (если есть ссылка на узел)
- Вставка/удаление с обоих концов — O(1) (при наличии ссылок на head и tail)

**Недостатки:**
- Требует **больше памяти** (дополнительная ссылка `prev`)
- Более сложная реализация (нужно поддерживать обе ссылки)

---

### 3.3. Кольцевой список (Circular Linked List)

**Структура:** последний узел ссылается на первый, образуя **кольцо**. Может быть как односвязным, так и двусвязным.

```python
class CircularNode:
    def __init__(self, data):
        self.data = data
        self.next = None

class CircularLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0

    def insert_at_beginning(self, data):
        """O(n) — нужно обновить ссылку последнего узла"""
        new_node = CircularNode(data)

        if not self.head:
            self.head = new_node
            new_node.next = self.head  # ссылается на себя
        else:
            # Находим последний узел
            current = self.head
            while current.next != self.head:
                current = current.next

            new_node.next = self.head
            current.next = new_node
            self.head = new_node

        self.size += 1

    def insert_at_end(self, data):
        """O(n) — нужно найти последний узел"""
        new_node = CircularNode(data)

        if not self.head:
            self.head = new_node
            new_node.next = self.head
        else:
            current = self.head
            while current.next != self.head:
                current = current.next

            current.next = new_node
            new_node.next = self.head

        self.size += 1

    def display(self, n=None):
        """Вывод элементов. n — сколько элементов показать (для бесконечных списков)"""
        if not self.head:
            print("Empty")
            return

        elements = []
        current = self.head
        count = 0
        max_display = n if n else self.size

        while count < max_display:
            elements.append(str(current.data))
            current = current.next
            count += 1

        print(" → ".join(elements) + " → ... (циклически)")

    def josephus(self, k):
        """Задача Иосифа Флавия — классическое применение кольцевого списка"""
        if not self.head:
            return None

        current = self.head
        while self.size > 1:
            # Делаем k-1 шагов
            for _ in range(k - 1):
                current = current.next

            # Удаляем k-й элемент
            to_remove = current.next
            print(f"Удаляем: {to_remove.data}")

            # Переносим данные из следующего узла в текущий
            current.data = to_remove.data
            current.next = to_remove.next

            self.size -= 1

        return current.data

# Демонстрация
cll = CircularLinkedList()
for i in range(1, 6):
    cll.insert_at_end(i)

print("Кольцевой список:")
cll.display()  # 1 → 2 → 3 → 4 → 5 → ... (циклически)

print("\nКольцевой список (10 элементов, хотя их всего 5):")
cll.display(10)  # Покажет циклическое повторение
```

## 4. Операции и их сложность (сравнение видов)

| Операция | Односвязный | Двусвязный | Кольцевой |
|----------|-------------|------------|-----------|
| **Вставка в начало** | O(1) | O(1) | O(n)* |
| **Вставка в конец** | O(n) | O(1)** | O(n) |
| **Удаление с начала** | O(1) | O(1) | O(n)* |
| **Удаление с конца** | O(n) | O(1)** | O(n) |
| **Вставка после узла** | O(1)*** | O(1)*** | O(1)*** |
| **Удаление узла** | O(n) & | O(1) && | O(n)& |
| **Поиск по значению** | O(n) | O(n) | O(n) |
| **Обход вперед** | O(n) | O(n) | бесконечно |

\* — для кольцевого нужен обход для обновления последнего узла

\*\* — при наличии ссылки на tail (хвост)

\*\*\* — если у нас уже есть ссылка на узел

& — нужно найти предыдущий узел

&& — можно удалить по ссылке на сам узел (благодаря prev)

---

## 5. Связный список в памяти: cache locality

### 5.1. Проблема фрагментации

В отличие от массива, узлы связного списка могут быть **разбросаны по всей памяти**, что приводит к:

- **Плохой cache locality** — процессор не может предзагрузить следующие элементы
- **Частым cache miss** — каждый переход к следующему узлу может требовать загрузки новой cache line

| Характеристика | Массив (Array) | Связный список (Linked List) |
|----------------|----------------|------------------------------|
| **Память** | Непрерывный блок | Фрагментированная |
| **Доступ по индексу** | O(1) — мгновенно | O(n) — нужно пройти |
| **Вставка в начало** | O(n) — сдвиг всех | O(1) — просто меняем head |
| **Вставка в конец** | O(1)* — амортизированно | O(1)** — с tail, иначе O(n) |
| **Вставка в середину** | O(n) — сдвиг | O(1) — если есть ссылка на узел |
| **Удаление из начала** | O(n) — сдвиг | O(1) |
| **Удаление из середины** | O(n) — сдвиг | O(1) — если есть ссылка |
| **Поиск по значению** | O(n) | O(n) |
| **Cache locality** | Отличная | Плохая |
| **Дополнительная память** | Нет | Есть (next, prev) |
| **Размер** | Фиксированный/амортизированный | Динамический |
| **Предсказуемость** | Высокая | Низкая (из-за аллокаций) |

---
## 6. Когда использовать связный список?

### **Используем связный список:**
- Частые вставки и удаления в **начало** или **середину** списка
- Размер данных часто и непредсказуемо меняется
- Нет необходимости в быстром доступе по индексу
- Нужна структура с **двунаправленной навигацией** (история браузера)
- Нужно **часто удалять** элементы из середины (и есть ссылка на узел)

### **Избегать связного списка:**
- Нужен частый доступ по индексу (`list[i]`)
- Важна cache locality (последовательный обход больших объемов данных)
- Требуется минимальное потребление памяти
- Данные хранятся в кэше процессора (массивы выигрывают)