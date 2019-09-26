
<center>

# Курс "Основы Python для анализа данных"

## Артамонов Игорь Михайлович
## Факультет "Прикладная математика" МАИ

### Занятие № 3.  Объектно ориентированное и функциональное программирование

</center>


## Общение / вопросы по курсу

Платформа для групповой работы Atlassian Confluence факультета "Прикладная математика"

https://mai.moscow/display/PYTML

* <b>Занятие 3. Объектно ориентированное и функциональное программирование</b>
       * Объектно ориентированное программирование
           * Общие понятия
           * ООП в Python
       * Функциональное программирование
           * Общие понятия
           * ФП в Python

## virtualenv + Jupyter notebook

```
<Ctrl> + <Alt> + T - новое окно терминала
```

```
$ conda -V

$ conda update conda

$ conda search "^python$"

$ conda create -n yourenvname python=x.x anaconda

$ source activate yourenvname

$ jupyter notebook

$ conda install -n yourenvname [package]
```

# Общее

* Python позволяет писать в рамках большинства подходов к программированию. Часто говорят, что он поддерживает несколько __парадигм__ программирования:
    - императивное
    - структурное
    - объектно-ориентированное
    - функциональное
* При наличии дополнительных пакетов (Kanren + SymPy) поддерживает даже логическое программирование
* Такие языки называют многопарадигменными (_multi-paradigm programming language_)

### Когда недостаточно императивного программирования:

* много (в том числе - очень) кода
* можно выделить структуру задачи через функции или объекты
* использование внешних библиотек, использующих определенный подход (например, почти все интерфейсные
  библиотеки используют объектно-ориентированный подход
* рефакторинг кода для улучшения его структуры
* хорошее понимание одной из парадигм и умение "раскладывать" в неё задачи

## <font color=blue>ВАЖНО!</font>

* наличие инструментов и использование парадигмы - это __разные__ вещи
* правильное понимание и использование парадигмы часто __важнее__, чем наличие инструментов
* многие инструменты могут успешно использоваться __вне__ "чистого" объектно-ориентированного или функционального кода


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import scipy as sc
%matplotlib inline
```

# Объектно-ориентированное программирование

## Основные принципы

* способ __структурирования__ программы
* поведение и свойства (чего-то) помещены в индивидуальные __объекты__
* объект является представителем __класса__

__Класс__ - определяет поведение __группы объектов__

__Объект__ (__пример__ класса) - содержит реальные значения, относящиеся непосредственно к нему

```python
class my_class(superclass(es)):
    
    def __init__(self):
        my_list = []
        my_dict = {1:1}
        n = 0
        pass
    
    def method1(self, *args):
        pass
    
    def method2(self, *args)
        return x
    
    def __len__(self):
        return n
```

self - обращение к переменной объекта
var != self.var


```python
class Gender:
    def __init__(self, gender):
        self.gender = gender

    def __str__(self):
        return self.gender
    
    def get():
        return self.gender
    
    def set(gender):
        self.gender = gender

```


```python
class Human:
    
    population = 0
    
    def __init__(self, name, age, gender):
        self.name = name
        self.age = age
        self.gender = gender
        Human.population += 1
    
    def __str__(self):
        return 'Имя: {}, возраст: {}, пол: {}'.format(self.name, self.age, self.gender)
    
    def get_name(self):
        return self.name

    @classmethod
    def how_many(cls):
        return  Human.population

```


```python
Human.how_many()
```


```python
vasya = Human('Вася', 30, Gender('М'))
print(type(vasya), vasya)
```


```python
Human('Петя', 32, Gender('М'))
```


```python
class Man(Human):
    def __init__(self, name, age, gender):
        Human.__init__(self, name, age, Gender('M'))
    
    def get_name(self):
        return 'Меня зовут ' + Human.get_name(self)
```


```python
print(Human.how_many())
```


```python
vasya = Man('Вася', 30, Gender('М'))
```


```python
print(vasya)
```


```python
print(vasya), type(vasya)
```


```python
isinstance(vasya, object)
```


```python
isinstance(vasya, Man)
```


```python
isinstance(vasya, list)
```


```python
isinstance([1,2,3], list)
```

## <font color=red>ЗАДАНИЕ</font>

Реализуйте класс клеток (cell) со следующими функциями:<br>
* при создании у клетки задается имя (строка)
* hello - клетка печатает "Клетка:" плюс свое имя
* die - клетка погибает
* divide - возвращяет tuple из двух новых клеток, к имени старой клетки добавлются 0 и 1
    старая клетка погибает
* Надо сохранять количество клеток в переменной класса


```python
class cell:
    
    amount = 0
    
    def __init__(self, name : str):
        self.name = name
        cell.amount += 1
    
    def hello(self):
        print(f'Клетка {self.name}')
    
    def __del__(self):
        cell.amount -= 1
    
    def divide(self):
        name = self.name
        return (cell(name + '0'), cell(name + '1'))
```


```python
kek = cell('kappa')
kek.hello()
kek.divide()
```


```python
print(cell.amount)
```


```python
del kek
```


```python
cell.amount
```

#### Чего в Python нет:
    * Перегрузки функций

#### Что есть:
    * Перегрузка операторов
        * Арифметических
        * Сравнения
        * Как бинарных (+,-), так и унарных (-)
    * Определение "стандартных" функций (len, str)


```python
class MyVector: 
    def __init__(self, a, b): 
        self.a = a 
        self.b = b 
    
    def __len__(self):
        return math.sqrt(self.a**2 + self.b**2)

    # adding two objects  
    def __add__(self, other): 
        return MyVector(self.a + other.a, self.b + other.b )
  
    def __neg__(self): 
        return MyVector(-self.a, -self.b)
  
    def __str__(self): 
        return f"({self.a}, {self.b})"
    
    def __lt__(self, other): 
        return self.__len__() < other.__len__()
        
    def __eq__(self, other):
        return self.a == other.a and self.b == other.b

```


```python
a = MyVector(2,3)
b = MyVector(2,3)
c = MyVector(3,5)

print(a, b, c)
```


```python
a + b
```


```python
z = b + c + c
```


```python
print(z)
```


```python
-a
```

# Функциональное программирование

## Основные принципы

__"Чистое" функциональное программирование__<br>

* Функциональное программирование основано на испольовании функций и их композиций
* Переменные отсутствуют, только константы
* Используются только неизменяемые структуры данных
* Нет циклов, нелокальных переходов и исключений
* Нет побочных эффектов. Для операций с побочными эффектами используются монады

В начале оно кажется очень сложным и непонятным ...

### Чистота функций
* функция возвращает одинаковый результат при одинаковых значениях аргументов
* у функции отсутствуют побочные эффекты -> не зависит от контекста



```python
def pure_func(x):
    return 2 * 3.1415926 * x 

y = 2
 
def impure_func(x):
    y = y + 2 * 3.1415926 * x
    return y
```

### Следствия
* могут выполняться (оцениваться) в любом порядке
* удобны для параллельного выполнения<br>

### Вызов чистой функции всегда может быть заменен на результат (ссылочная прозрачность)
* проще  тестирование (функция "изолирована")
* читаемость (не надо анализировать другие функции)
* возможность анализа и валидации кода

### Первоклассные функции

* Функции имеют те же свойства, что и остальные значения
* Им пожно присвоить имя или присвоить их объекту
* Их можно передать, как аргумент функции
* Их можно возвращать

## <font color=red>ЗАДАНИЕ</font>

Реализуйте класс, принимающий имя функции в виде строки и функцию, связанную с этой строкой. <br>
При выполнении метода __execute__ с кодом и переменной или значением выполняет функцию, связанную с кодом.<br>
Если кода нет, то методы генерирует прерывание __KeyError__


```python
class FDict():
    def __init__(self):
        self.kek = {}

    # добавляет функцию с кодом
    def add(self, code, func):
        self.kek[code] = func
    
    # удаляет функцию по коду
    def remove(self, code):
        if code not in self.kek:
            raise KeyError('Саня, ты в порядке?')
        self.kek.pop(code)

    # выполняет функцию с аргументом
    def execute(self, code, arg):
        if code not in self.kek:
            raise KeyError('Саня, ты в порядке?')
        return self.kek[code](arg)
    
    # возвращает функцию по коду
    def get_func(self, code):
        if code not in self.kek:
            raise KeyError('Саня, ты в порядке?')
        return self.kek[code]
    
    # получает 2*N аргументов (код, значение, код, значение ..) и возвращает кортеж из значений
    def execute_many(self, *args):
        return tuple(self.kek[i](j) for i, j in zip(args[::2], args[1::2]) if i in self.kek)
```

Проверьте функционирование Вашего кода. Проверьте работу с отсутствующим кодом и добавьте обработку исключения.


```python
import math
from math import sin, cos

x = math.pi / 3

func_dict = FDict()
func_dict.add('sin', math.sin)
func_dict.add('cos', math.cos)

y1 = func_dict.execute('sin', x)
y2 = func_dict.execute('cos', x)

print("1. sin({0:.4f})={1:.4f}, cos({0:.4f})={2:.4f}".format(x, y1, y2))

y1, y2 = func_dict.execute_many('sin', x*2, 'cos', x*3)
print("2. sin({0:4f})={1:4f}, cos({0:4f})={2:4f}".format(x, y1, y2))
```

### Лямбда-функции (анонимные функции)


```python
f = lambda x, y: sin(x**2) + cos(y**2)

def func(x,y):
    return sin(x**2) + cos(y**2)

f(math.pi/2, math.pi/2)
```

## <font color=green>ВСПОМИНАЕМ</font> генераторы списков, словарей и просто генераторы


```python
import math

even_square_roots = [(lambda x: math.sqrt(x))(x) for x in range(37) if x % 2 == 0]
even_square_roots

```

## <font color=red>ЗАДАНИЕ</font>

1. Преобразуйте этот код в формирование словаря, в котором число - ключ, а корень из него - значение


```python
## Ваш код
{i : i**2 for i in range(37)}
```

2. Преобразуйте этот код в формирование генератора и пройтиде по нему с помощью цикла for


```python
## Ваш код

```

### Функции для работы с итерируемыми

```python
    map()
    filter()
    reduce()
```

__map__ - выполняет функцию над всеми элементами итерируемого


```python
squared =  map(lambda x: x**2, [1,2,3])
squared
```


```python
type(squared)
```


```python
x = (i for i in range(10))
type(x)
```


```python
isinstance(squared, map)
```

__<font color=blue>??</font>__ Какой тип возвращает __map__? Почему?

## <font color=red>ЗАДАНИЕ</font>

Найдите длину самой длинной строки в списке с помощью __map__


```python
l = ['abc', 'xyz', 'ab', 'qwer', 'trench', 'm', '17', 'www.mai.ru', 'd', 'plot']
```


```python
max(map(lambda x: (len(x), x), l))
```

__filter__ - возвращает те значения из итерируемого, для которых верна функция


```python
even = list(filter(lambda x:x % 2 == 0, range(20)))
even
```

## <font color=red>ЗАДАНИЕ</font>

Найдите все строки, в которых присутствует буква __'a'__


```python
# Ваш код
list(filter(lambda x: 'a' in x,l))
```

__reduce()__ - сворачивает итерируемое в одно значение<br>
В отличие от map и filter у функции 2 аргумента


```python
from functools import reduce
```


```python
values = list(range(10))
summed = reduce( lambda x,y: x+y, values)
summed
```

## <font color=red>ЗАДАНИЕ</font>

Верните самую длинную строку из списка с помощью reduce<br><br>
__<font color=green>Уложившимся в 1 строку с lambda-функцией - 1 балл</font>__


```python
reduce(lambda x, y: x if len(x) > len(y) else y, l)
```

### Ещё полезности из __functools__

__functools__ - работа с функциями высшего порядка, то есть функциями, которые работают с функциями
или возвращают другие функции


```python
from functools import partial
```


```python
def f(a, b, c):
    return a + b + c

f(1, 2, 3)
```


```python
part_f = partial(f, 100)
part_f(4, 5)
```


```python
part_f = partial(f, b=10)
part_f(a=4, c=5)
```

## <font color=red>ДОМАШНЕЕ ЗАДАНИЕ</font>

Сделайте класс бинарного дерева, который наполняется строками. <br>
Все функции добавления, поиска, удаления, длины должны быть рекурсивными<br><br>
<font color=red>__deadline - 18:00 27.09__</font>


```python
from difflib import ndiff
```


```python
class Bst:
    class Node:
        def __init__(self, value):
            self.left = None
            self.right = None
            self.value = value

    def __init__(self):
        self.root = None
        self.size = 0
        self.values = []

    def add(self, value):
        if self.root is None:
            self.root = self.Node(value)
        else:
            self.root = self.insert_node(self.root, value)
        self.size += 1
        self.values.append(value)

    # Добавление строки к дереву выглядит странно
    def __add__(self, s):
        '''
        Переопределение +
        '''
        add(self, s)

    def insert_node(self, cur_node, value):
        if cur_node is None:
            return self.Node(value)
        elif cur_node.value <= value:
            cur_node.right = self.insert_node(cur_node.right, value)
        else:
            cur_node.left = self.insert_node(cur_node.left, value)
        return cur_node

    def remove(self, value):
        if not self.isin(value):
            return 1
        self.root = self.remove_node(self.root, value)
        self.size -= 1
        return 0

    def isin(self, value):
        return self._isin(self.root, value)
    
    def _isin(self, cur_node, value):
        if cur_node is None:
            return False
        elif cur_node.value < value:
            return self._isin(cur_node.right, value)
        elif cur_node.value > value:
            return self._isin(cur_node.left, value)
        return True

    def remove_node(self, cur_node, value):
        if cur_node is None:
            return None
        elif cur_node.value < value:
            cur_node.right = self.remove_node(cur_node.right, value)
        elif cur_node.value > value:
            cur_node.left = self.remove_node(cur_node.left, value)
        else:
            if cur_node.left and cur_node.right:
                min_node = self.find_min(cur_node.right)
                cur_node.value = min_node.value
                cur_node.right = self.remove_node(cur_node.right, min_node.value)
            elif cur_node.left:
                cur_node.value = cur_node.left.value
                cur_node.right = cur_node.left.right
                cur_node.left = cur_node.left.left
            elif cur_node.right:
                cur_node.value = cur_node.right.value
                cur_node.left = cur_node.right.left
                cur_node.right = cur_node.right.right
            else:
                del cur_node
                return None
            return cur_node

    @staticmethod
    def find_min(cur_node):
        while cur_node.left:
            cur_node = cur_node.left
        return cur_node

    def __len__(self):
        return self.size
    
    @staticmethod
    def dist(value1, value2):
        return sum([1 for i in ndiff(value1, value2) if '+' in i or '-' in i])

    def get(self, value):
        nearest = self.search_node(self.root, value)
        return nearest, self.dist(nearest, value)

    def search_node(self, cur_node, value):
        prev_word = ''
        while cur_node is not None:
            if cur_node.value == value:
                return value
            elif cur_node.value < value:
                prev_word = cur_node.value
                cur_node = cur_node.right
            else:
                prev_word = cur_node.value
                cur_node = cur_node.left
        return prev_word

    def __str__(self):
        return self.print_tree(self.root, 0).rstrip()
    
    def print_tree(self, cur_node, level):
        if cur_node:
            line1 = self.print_tree(cur_node.right, level + 1)
            line2 = '{:>{width}}\n'.format(cur_node.value, width=level * 7 + 1)
            line3 = self.print_tree(cur_node.left, level + 1)
            return ''.join([line1, line2, line3])
        return ''

    def to_list(self):
        '''
        возвращает все строки в виде упорядоченного списка
        '''
        return self.values
```


```python
from random import randint, sample
from string import ascii_letters

tree = Bst()

for i in (''.join(sample(ascii_letters, randint(3, 8))) for i in range(1000)):
    tree.add(i)

print('len is ', len(tree))
```


```python
t = tree.get('kappa')
print(t)
tree.remove(t[0])
tree.isin(t[0])
```


```python
class StrBinTree:
    '''
    Класс бинарного дерева с функциями поиска и выдачи расстояния до ближайшей сохраненной строки:
     '''
    def __init__(self):
        pass
    
    def add(self, s):
        '''
        добавляет строку в дерево
        '''
        pass
    
    # Каков смысл этой функции?
    def __add__(self, s):
        '''
        ваше описание
        '''
        add(self, s)
    
    def isin(self, str):
        '''
        возвращает True, если есть точно такая же строка, или False, если её нет
        '''
        pass
        
    def remove(self, str):
        '''
        удаляет строку. Удаление производится только в случае точного совпадения
        строки с указанной. Возвращает 0, если удаление выполнено, и 1 в остальных случаях
        '''
        pass
        
    def get(self, str):
        '''
        возвращает ближайшую с лексической точки зрения строку и расстояние строки-аргумента
        до нее
        '''
        pass

    def __len__(self):
        '''
        возвращает количество строк в дереве
        '''
        pass
    
    def to_list(self):
        '''
        возвращает все строки в виде упорядоченного списка
        '''
        pass
```


```python
help(StrBinTree)
```

ДЗ посылать по адресу __mai-ml AT mail.ru__

## Экзаменационные вопросы:

* Объектно-оринетированное программирование. Основные положения
* Классы и объекты в Python
* Функциональное программирование. Основные положения
* Функциональное программирование в Python
* Генераторы списков, словарей и просто генераторы
* Функции отображения: map, filter, reduce

### К следующему занятию п(р)очитать:


* что такое объектно ориентированное програмирование
* что такое функциональное программирование
