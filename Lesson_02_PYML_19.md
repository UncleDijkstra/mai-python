
<center>

# Курс "Основы Python для анализа данных"

## Артамонов Игорь Михайлович
## Факультет "Прикладная математика" МАИ

### Занятие № 2.  Основные типы данных. Обработка исключений.

</center>


<center>
    
### Студент: Аникин Д. А.

</center>

## Общение / вопросы по курсу

Платформа для групповой работы Atlassian Confluence факультета "Прикладная математика"

https://mai.moscow/display/PYTML

* <b>Занятие 2. Основные типы данных. Обработка исключений.</b>
     * Последовательности: строка, список, кортеж, множество
     * Словарь
     * Стеки, очередь, дерево
     * Случайные величины
     * Исключения и их обработка

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

# ВАЖНО!

* курс построен как "__слойка__"
* плохое освоение модуля __сильно__ затрудняет освоение следующего модуля
* возвратов и повторов __мало__ ...

# Общее

* Python имеет много __встроенных__ сложных структур данных
* Наиболее часто используемыми являются:
   - строка
   - список
   - кортеж
   - словарь
   - множество
* В большинстве случаев, код для встроенных структур написан на языке низкого уровня и хорошо оптимизирован
* __ВЫВОД № 1__: если Вам требуется какая-то структура, то:
   - поищите уже реализованную структуру (стек, очередь, ...)
   - сводите её к одной из типовых
   - старайтесь максимально использовать встроенные функции типовых структур
* __ВЫВОД № 2__:
   - имеет смысл __полностью__ просматривать руководство по нужному классу


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import scipy as sc
%matplotlib inline
```

## Основные сложные структуры данных

* связанные списки (linked list) - односторонние и двусторонние
* стек (stack)
* очередь (queue)
* хэш-таблица (hash table)
* двоичное дерево (binary tree)

# Рекурсия

Рекурсия - обращение функции __к самой себе__


```python
def factorial(n):
    if n == 1:
        return 1
    else:
        return n * factorial(n-1)
```


```python
factorial(10)
```

### Сохранение значения


```python
def sum_accum(current, accum, n):
    # Финальное состояние
    if current == n:
        return accum

    # Рекурсия
    else:
        return sum_accum(current + 1, accum + current, n)
```


```python
sum_accum(0, 0, 5)
```

### Когда рекурсия полезна?

* большая читаемость кода
* работа с неизменяемыми значениями
* работа с "естественно" рекурсивными структурами (деревья, графы)

### Ограничения

* рекурсия менее эффективная, чем итрации
* ограничена глубина стека


```python
import sys
sys.getrecursionlimit()
```

### <font color=red>Задание</font>

Вычислите рекурсивно __$e^x$__ по $n$ первым элементам ряда:

$$e(x) = \sum_{i=0}^{n} \frac{x^i}{i!}$$


```python
import math

def my_exp(x, n):
    if n == 0:
        return 1
    return my_exp(x, n - 1) + x**n/factorial(n)
```


```python
print("{:.8f}".format(my_exp(1, 50)))
```

## Строка

* могут ограничиваться либо одинарными __'< str >'__ , либо двойными __"< str >"__, либо тройными двойными ___"""< multiline str >"""___ кавычками
* в целом, аналогична списку из символов
* может быть преобразована в список командой __list()__
* существуют команды, типичные для обработки строк в других языках


```python
s = 'Ехал Грека через реку, видит грека - в реке рак'
```


```python
'abc' == "abc"
```


```python
'abc' > "rbc"
```


```python
long_str = """
this is
a very
long 
string
"""
```


```python
long_str, len(long_str)
```


```python
s = 'Ехал Грека через реку, видит грека - в реке рак'
```


```python
s.startswith( 'река' ), s.startswith( 'Ехал' )
```


```python
print(s.startswith( 'река', 6))
```


```python
print(s.startswith( 'река', 6, 9))
```


```python
print(s.endswith( 'рак'))
```


```python
s.count('рек')
```


```python
s.find('через')
```


```python
s_enc = s.encode('cp1252', errors='ignore')
s_enc
```


```python
s = "ascii"
```


```python
s.join(','), s.join(',,,,,')
```


```python
long_str.split('\n')
```

### Специальные символы

* \n - перевод строки
* \r - возврат каретки
* \t - табуляция
* \xNN - символ в 16-ричной нотации
* \ - символ экранирования (\\n)


```python
print('a' + '\n' + 'b')
```


```python
print('a' + '\r' + 'b')
```


```python
print('a' + '\t' + 'b')
```


```python
print('a' + '\\n' + 'b')
```

### Индексирование строк и списков

* индекс размещается в квадратных скобках __\[ \]__
* начинается с 0
* отрицательный индекс __n__ означает __len(s) - n__ => -1 __исключает__ последний символ
* можно указывать диапазон через двоеточие __:__
* отсутствующий индекс перед двоеточиием означает __"с начала"__, после - __"до последнего символа включительно"__
* можно добавить второе двоеточие, которое означает __"шаг"__


```python
s = 'Большая длинная тестовая строка'
```


```python
len(s)
```


```python
s[31]
```


```python
s[16:-1], s[16:len(s)], s[-1]
```


```python
s = '0123456789'
s[0:-1:2]
```

### <font color=red>Задание</font>

* пользуясь __только__ индексом, выведите строку в обратном порядке


```python
s[::-1]
```

## Операции над строками


```python
s = 'abc' + ' - ' + 'cde'
s
```


```python
s = (s + '? ') * 3
s
```


```python
to_find = 'abc'
if to_find in s:
    s1 = 'Нашли <' + to_find + '> в строке <' + s + '>'
    print(s1)
```

### Преобразование регистра


```python
s1.upper(), s1.lower()
```


```python
s1.find('abc')
```


```python
s1.find('abs')
```


```python
s1.count('abc'), s1[8:].count('abc')
```


```python
s = '  qwerty  '
s_left = '<' + s.lstrip() + '>'
s_right = '<' + s.rstrip() + '>'
s_both = '<' + s.strip() + '>'
print(s_left, s_right, s_both)
```


```python
s1.replace('abc', 'ABC')
```


```python
s1
```


```python
s1.split('?')
```

### Форматирование

* __% форматирование__
    * %d - целые
    * %s - строки
    * %f - с плавающей точкой


```python
print('Это строка <%s>, это целое число <%05d>, а это число с плавающей точкой <%03d>' % ('ABC', 234, 3.1415926))
```


```python
print(s_fmt1)
```

* Форматирование классом __formatter__


```python
fmt = 'Это строка <{0:s}>, это целое число <{1:05d}>, это число с плавающей точкой <{2:3.4f}>, а это снова строка {0}'
print(fmt.format('ABC', 234, 3.1415926) )
```


```python
fmt = 'Это строка <{}>, это целое число <{}>, это число с плавающей точкой <{}>'
print(fmt.format('ABC', 234, 3.1415926) )
```


```python
fmt = 'Это строка <{}>, это целое число <{}>, это число с плавающей точкой <{}>, а это снова строка {0}'
print(fmt.format('ABC', 234, 3.1415926) )
```


```python
a = 123
b = 'I\'m a strting'
c = 3.1415926

```


```python
s = f"{a} {b} {c}"
print(s)
```


```python
s = f"{a:05d} {b} {c:.2f}"
print(s)
```

### Для самостоятельного изучения:


* регулярные выражения (regular expressions)


```python
import re
```

# Список


```python
# Пустой список
l = []

# Список целых чисел
l = [1, 2, 3]

# список со различными типами данных
l = ['abc', 'cde', 3.14159, 2.71828, 1]
```

### <font color=red>Задание:</font>
* выведите для каждого элемента списка:
        - порядковый номер (начиная с нуля)
        - значения
        - тип значения


```python
# Ваш код здесь

for i, e in enumerate(l):
    print(i, e, type(e))
```

__list__ - __ключевое__ слово, которое  (формально) может использоваться, как имя переменной.<br>
Однако, это __сильно__ не рекомендуется делать, так как это приводит к побочным эффектам


```python
l = list( np.array([1,2,3,4]))
l
```


```python
list = [1,2,3,4]
```


```python
list
```


```python
l = list( np.array([1,2,3,4]))
l
```


```python
r = range(10)
s = str(r)
l = list(r)
```


```python
r, s, l
```


```python
s = str(l)
s
```

* Сравнение списков аналогично сравнению строк и выполняется поэлементно (у строк - посимвольно)


```python
[1, 2, 3] < [1, 3, 4], 'abc' > 'cba'
```

## Индексация списка

Совпадает с индексацией строк:
* индекс размещается в квадратных скобках __\[ \]__
* начинается с 0
* отрицательный индекс __n__ означает __len(s) - n__ => -1 __исключает__ последний символ
* можно указывать диапазон через двоеточие __:__
* отсутствующий индекс перед двоеточиием означает __"с начала"__, после - __"до последнего символа включительно"__
* можно добавить второе двоеточие, которое означает __"шаг"__

### Изменение и добавление элементов


```python
l1 = list( i*2 for i in 'abcdefghijk')
l1
```


```python
l2 = list( i for i in range(10))
```


```python
l1 + l2[2:4] + l2[3:7]
```


```python
l = l1
l.append(l2[2:4])
l
```


```python
l = l1
l.extend(l2[2:4])
l
```


```python
l1
```

### <font color=blue>Нужно запомнить</font>

Списки - <b>изменяемый</b> тип данных

### Удаление элементов


```python
l.clear()
l.extend([1,2,3,4,5])
l
```


```python
l.insert(5, 6)
l
```


```python
l.sort(reverse=True)
l3 = l.copy()
```


```python
l.remove(2)
l
```


```python
l.pop()
l
```

### <font color=red>Опишите, что делает этот код</font>


```python
txt = 'а роза упала на лапу азора'
words = txt.split()

l = list()
for word in words:
    l.append((len(word), word))
print(l)

l.sort(reverse=True)
print(l)

res = list()
for length, word in l:
    res.append(word)

print(res)
```

1) Разбивает предложение на токены  
2) Считает длины каждого токена  
3) Сортирует кортежи с токенами и длинами в обратном порядке  
4) Собирает токены в список

### Методы списков

```python
append() Добавляет элемент в конец списка
extend() Добавляет все элементы списка к другому списку
insert() Вставляет элемент на место, указнное индексом
remove() Удаляет элемент из списка
pop() Удаляет элемент из списка и возвращает его, как параметр
clear() Очищает список (удаляет все элементы)
index() Возвращает индекс первого встречающегося элемента
count() Возвращает количество вхождений данного элемента в списке
sort() Сортирует элементы списка в возрастающем порядке
reverse() Изменяет порядок элементов в списке на обратный
copy() Копирует список
```

```python
all() Возвращает True если все элементы списка верны или если список пустой
any() Возвращает True если хотя бы один элементы списка верены. если списко пустой, возвращает False
enumerate() Возвращает объект enumerate, содержащий индекс и значение всех элементов списка в виде кортежей
len() Возвращает длину (количество элементов в) списке
list() Преобразует любой итерируемый объект (iterable) в список (кортеж, строку, множество, словарь)
max() Возвращает максимальный элемент в списке
min() Возвращает минимальный элемент в списке
sorted() Возвращает новый отсортированный список (не сортирует сам список!)
sum() Возвращает сумму элементов в списке
```

### <font color=red>Задание</font>

* Постройте частотную характеристику по словам в тексте Гамлета с использованием списков
    * Используйте split для разбивки по пробелам
    * удалите возвраты строк и символы, отличающиеся от букв
    * выведите 10 наиболее часто встречающихся слов
    * используйте zip для слияния списков в один
* Посчитайте, сколько вообще слов в Гамлете

#### <font color=blue>Порядок решения:</font><br>
    ???


```python
import urllib

hamlet_url = "http://erdani.com/tdpl/hamlet.txt"
f = urllib.request.urlopen(hamlet_url)
hamlet_text = f.read().decode('utf-8')
```


```python
text = [i.strip().lower() for i in hamlet_text.split() if i.strip().isalpha()]
```


```python
len(text)
```

Для удобства воспользуемся counter'ом


```python
from collections import Counter
freq = Counter(text)
freq.most_common(10)
```

### Генераторы списков (list comprehensions)


```python
l = [i**2 for i in range(10)]
l
```


```python
l_even = [i*j for i in range(10) for  j in range(6) if i*j % 2 == 0]
l_even
```


```python
list1 = ['1', '2', '3', '4', '5']
str1 = ' '.join(list1)
str1
```


```python
list2 = list(range(6))
str2 = ''.join(list2)
```

### <font color=red>Задания:</font>

С помощью генераторов списка:
* Исправьте предыдущий пример, чтобы получить строку
* просуммируйте числа от 1 до 100


```python
''.join(str(i) for i in list2)
```


```python
sum([i for i in range(1, 101)])
```

# Кортеж

Кортеж (tuple) отличается от списка тем, что он __неизменяемый__ (immutable)


```python
t = 'a', 'b', 'c', 'd', 'e'
t
```


```python
t = ('a', 'b', 'c', 'd', 'e')
t
```


```python
t = tuple('Some string')
t
```


```python
url = 'www.mai.ru'
a,b,c = url.split('.')
```


```python
a, b, c
```


```python
a, b = b, a
```


```python
url_parts = url.split('.')
url_parts, type(url_parts)
```


```python
d = {i:c for i, c in enumerate('abcdefg')}
d
```


```python
items = d.items()
items
```


```python
sorted(items)
```

### <font color=green>Полезно запомнить</font>


```python
[1, 2, 3] < (1, 3, 4)
```


```python
tuple([1, 2, 3]) > (1, 3, 4), [1, 2, 3] > list((1, 3, 4))
```


```python
gen = (i for i in 'Как-то вот так генерируем')
```


```python
gen, type( list( gen))
```

# Словарь


```python
d = {1:'a', 2:'b', 3:'c', 4:'d', 5:'e'}
d
```


```python
d[1]='oops'
d
```


```python
d.keys(), d.values(), d.items()
```


```python
sum( list( d.values()))
```


```python
list(d.items())
```


```python
for i in d:
    print( i, d[i])

```


```python
for i, k in enumerate(d):
    print( i, k, d[k])
```


```python
for i, k in enumerate( range(10)):
    print(i, k)
```


```python
d.pop(2)
```


```python
d[8] = 'oops!'
d
```


```python
del d[3]
d
```

hashable!!!

### Генераторы словарей (dictionary comprehensions)


```python
d = {i:i**2 for i in range(11)}
d
```


```python
d = (i for i in range(11))
d
```


```python
list(d)
```


```python
# в чем ошибка?
d = [i:i**2 for i in range(11)]
d
```

### <font color=red>Задание</font>

* Перепишите код с частотной характеристикой слов в тексте Гамлета с использованием словаря

См.Counter

## Списки и словари в аргументах функции

#### Скорректируйте функции так, чтобы они выдавали аргумент функции и его тип


```python
def login(* usernamepassword):
    # Get username in the list.
    username = usernamepassword[0]
    # Get password in the list.
    password = usernamepassword[1]

    if(username=='hello') and (password == 'hello'):
        print("Login Success!")
    else:
        print("Login Failed!")

login('hello','hello')
```


```python
def loginWithDictArguments(**upDict):
    # Get all keys in the dictionary.
    keys = upDict.keys();

    username='';
    password='';
    email='';

    # Loop in the dict keys.
    for key in keys:
        if(key=='username'):
            username=upDict[key]
        
        if(key=='password'):
            password=upDict[key]

        if(key=='email'):
            email=upDict[key]

    if(username=='hello') and (password=='hello'):
        print('Login Success. Your email is ' + email)
    else:
        print('Login fail. Your email is ' + email)     

loginWithDictArguments(username='hello', password='hello', email='vasya@pupkin.ru') 
```

## Множество


```python
a = set((0,1,2,3,4,5,6,7,8,9,3,4,5,6,7,8,0))
b = set(['a', 'b', 'c', 'd', 1, 2, 3, 4])
```


```python
a,b
```


```python
print('1=', a.intersection(b), '\n2=', b.intersection(a))
```


```python
print('1=', a.difference(b), '\n2=', b.difference(a))
```


```python
print('1=', a.symmetric_difference(b), '\n2=', b.symmetric_difference(a))
```


```python
print(a.union(b))
```

### <font color=red>Задание</font>

* Найдите количество уникальных слов в Гамлете с помощью множества


```python
len(set(text))
```

### Стек


```python
stack = [1, 2, 3, 4, 5]
```


```python
stack.append(6)
```


```python
stack.append(7)
```


```python
stack
```


```python
stack.pop()
```


```python
stack.pop()
```


```python
stack
```

# Collections


```python
import collections
```

### Очередь


```python
queue = collections.deque()
```


```python
from collections import deque, ChainMap, OrderedDict, Counter
```


```python
queue = deque()
```

```python
    append(x) - добавить элемент справа
    appendleft(x) - добавить элемент слева
    extend(iterable) - расширить списком справа
    extendleft(iterable) - расширить списком слева
    pop() - извлечь элемент справа
    popleft() - извлечь элемент слева
```


```python
import random 

queue = deque()
for i in range(50):
    x = random.randint(0,1000)
    size = len(queue)
    if size == 0:
        queue.append(x)
    elif x < queue[0]:
        queue.appendleft(x)
    elif x >  queue[size-1]:
        queue.append(x)
```


```python
 queue
```

### ChainMap

Объединяет словари без создания нового объекта "словарь". "Вохдящие" в цепочку словари используются, как ссылки


```python
chain = ChainMap()
```


```python
d1 = {'вишня': 5, 'яблоко': 1, 'персик': 8, 'груша': 2, 'тыква':7,  'баклажан':11}
d2 = {'мороженное': 27, 'яблоко': 2, 'торт': 102, 'маффин': 13, 'ром-баба':32,  'пирожок с капустой':33}
```


```python
chain = ChainMap(d1, d2)
```


```python
chain['вишня'], chain['торт'], chain['яблоко']
```


```python
len(d1), len(d2), len(chain)
```

### Словарь с упорядочением


Сохраняет порядок, у котором вносились элементы списка


```python
# обычный словарь
d = {'вишня': 5, 'яблоко': 1, 'персик': 8, 'груша': 2, 'тыква':7,  'баклажан':11}
```


```python
# словарь, отсортированый по ключам
OrderedDict(sorted(d.items(), key=lambda t: t[0]))
```


```python
# словарь, отсортированый позначениям
OrderedDict(sorted(d.items(), key=lambda t: t[1]))
```


```python
# словарь, отсортированый по длине слова-ключа
OrderedDict(sorted(d.items(), key=lambda t: len(t[0])))
```

### Счетчик вхождения элементов

Интерфейс аналогичен словарю


```python
from collections import Counter
import urllib
import re

hamlet_url = "http://erdani.com/tdpl/hamlet.txt"
f = urllib.request.urlopen(hamlet_url)
hamlet_text = f.read().decode('utf-8')
words = re.findall(r'\w+', hamlet_text.lower())
ctr = Counter(words).most_common(10)
ctr
```


```python
sum([k for i,k in ctr])
```

# Дерево

### <font color=red>Задание</font>

* Создайте список из 1000 случайных строк, состоящих из латинских букв.
* Каждая строка - случайной длиной от 3 до 8 символов.
* Реализуйте бинарное дерево для строк
* Реализуйте поиск ближайшего слова для заданного.


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

    def insert(self, value):
        if self.root is None:
            self.root = self.Node(value)
        else:
            self.root = self.insert_node(self.root, value)
        self.size += 1

    def insert_node(self, cur_node, value):
        if cur_node is None:
            return self.Node(value)
        elif cur_node.value <= value:
            cur_node.right = self.insert_node(cur_node.right, value)
        else:
            cur_node.left = self.insert_node(cur_node.left, value)
        return cur_node

    def remove(self, value):
        self.root = self.remove_node(self.root, value)
        self.size -= 1

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
    
    def nearest_word(self, value):
        return self.search_node(self.root, value)

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
```


```python
from random import randint, sample
```


```python
tree = Bst()

for i in (''.join(sample(ascii_letters, randint(3, 8))) for i in range(1000)):
    tree.insert(i)

print('len is ', len(tree))
```


```python
tree.nearest_word('kappa')
```

# Исключения и их обработка

```python
try:
    You do your operations here;
    ......................
except ExceptionI:
    If there is ExceptionI, then execute this block.
except ExceptionII:
    If there is ExceptionII, then execute this block.
    ......................
else:
    If there is no exception then execute this block. 
```


```python
l = list(range(10))
l[11]
```


```python
try:
    print(l[9])
except IndexError:
    print('Выход за границы списка!')
else:
    print('Получилось!')

```


```python
try:
    open('very_strange_file.txt')
except:
    print('Что-то не так с файлом!')
else:
    print('Странно ... Откуда он здесь?')
```


```python
def func( n ):
    if n < 0:
        raise ValueError("n должен быть больше 0!", n)
    pass
```


```python
func(-2)
```

## Экзаменационные вопросы:

* Рекурсия
* Регулярные выражения
* Обработка и индексирование строк
* Обработка и индексирование списков
* Словари
* Множества
* Очередь
* Дерево
* Стек
* Исключения и их обработка

### К следующему занятию прочитать:


* что такое объектно ориентированное програмирование
* что такое функциональное программирование
