<details>
  <summary>Дополнительное задание на Python #1</summary>

# [1] 
 ```python

'''1. Перевернуть строку'''

def reverse_string(s):
    return s[::-1]

print(reverse_string("hello"))  # "olleh"
 ```
# [2] 
 ```python

'''2. Проверить, является ли строка палиндромом'''

def is_palindrome(s):
    return s == s[::-1]

print(is_palindrome("madm"))
 ```
 # [3] 
 ```python

'''3. Подсчитать количество вхождений символа в строку'''

def count_char(s, char):
  cnt = 0
  for i in s:
    if i == char:
      cnt += 1
  return cnt

print(count_char("hello", "l"))
 ```
 # [4] 
 ```python

'''4. Удалить все гласные из строки'''

def remove_vowels(s):
    t = []
    for i in s:
      if i not in ['a', 'e', 'i', 'o', 'u']:
        t.append(i)
    return ''.join(t)

print(remove_vowels("hello"))  # "hll"
 ```

  # [5] 
 ```python

'''5. Найти самое длинное слово в строке'''

def longest_words(s):
    words = s.split()

    max_length = max(len(word) for word in words)
    return [word for word in words if len(word) == max_length]

print(longest_words("hello world this is a test"))
 ```

   # [6] 
 ```python

'''6. Заменить все пробелы в строке на подчеркивания'''
def replace_spaces(s):
    new = []
    for i in s:
      if i != ' ':
        new.append(i)
      else:
        new.append('_')
    return ''.join(new)

print(replace_spaces("hello world"))
 ```
# [7] 
```python
 '''7. Удалить все цифры из строки'''
def remove_digits(s):
    s_1 = []
    for i in s:
      if i in ['1','2','3','4','5','6','7','8','9','0']:
        continue
      else:
        s_1.append(i)
    return ''.join(s_1)

print(remove_digits("hello123"))

 ```
# [8] 
 ```python
 '''8. Вернуть первые n символов строки'''

def first_n_chars(s, n):
  cnt = 0
  r = []
  for i in s:
    if cnt != n:
        r.append(i)
        cnt += 1
  return ''.join(r)

print(first_n_chars("hello", 5))

 ```
# [9]
  ```python
 '''9. Сделать первую букву строки заглавной'''

def capitalize_first(s):
  return s[0].upper() + s[1:]


print(capitalize_first("hello"))

 ```
# [10]
   ```python
 '''10. Поменять местами первую и последнюю буквы строки'''

def swap_first_last(s):
    return s[-1] + s[1:-1] + s[0]

print(swap_first_last("hello"))  # "oellh"
 ```
# [11]
   ```python
 '''11. Найти сумму элементов списка'''

def sum_list(lst):
  sum = 0
  for i in lst:
    sum = sum + i
  return sum
print(sum_list([1, 2, 3, 4]))
 ```
# [12]
   ```python
 '''12. Найти максимальный и минимальный элементы списка'''

def min_max(lst):
    return min(lst), max(lst)

print(min_max([1, 2, 3, 4]))
 ```
# [13]
```python
 '''13. Удалить дубликаты из списка'''

def remove_duplicates(lst):
    return list(set(lst))
 ```
# [14]
```python
  '''14. Отсортировать список по убыванию'''

def sort_desc(lst):
    lst.sort(reverse=True)
    return lst
 ```
# [15]
```python
  '''15. Перевернуть список'''

def reverse_list(lst):
    lst.reverse()
    return lst

print(reverse_list([1, 2, 3, 4]))
 ```
# [16]
```python
  '''16. Объединить два списка без дубликатов'''

def merge_lists(lst1, lst2):
    return list(set(lst1+lst2))

print(merge_lists([1, 2], [2, 3]))
 ```
# [17]
```python
  '''17. Вернуть список только с четными числами'''
def even_numbers(lst):
    for i in lst:
      if i % 2 != 0:
        lst.remove(i)
    return(lst)

print(even_numbers([1, 2, 3, 4]))
 ```
# [18]
```python
   '''18. Посчитать количество положительных чисел в списке'''
def count_positive(lst):
    cnt = 0
    for i in lst:
      if i > 0:
        cnt += 1
    return cnt
print(count_positive([1, -2, 3, 4]))
 ```
# [19]
```python
   '''19. Создать список квадратов чисел'''
def square_numbers(lst):
    k =[]
    for i in lst:
      k.append(i**2)
    return k
print(square_numbers([1, 2, 3]))
 ```
# [20]
```python
   '''20. Объединить два списка в список кортежей'''
def pair_lists(lst1, lst2):
    return list(zip(lst1, lst2)) 

print(pair_lists([1, 2], ["a", "b"])) 
 ```
# [21]
```python
   '''21. Проверить, является ли строка строкой с уникальными символами'''
def has_unique_chars(s):
    if len(set(s)) == len(s):
      return True
    else:
      return False

print(has_unique_chars("abcdef"))  # True
print(has_unique_chars("aabbcc"))
 ```
# [22]
```python
   '''22. Найти индекс первого вхождения символа в строке'''
def first_occurrence(s, char):
    return s.find(char)

print(first_occurrence("hello", "l"))
 ```
# [23]
 ```python
   '''23. Проверить, является ли число простым'''
def is_prime_simple(n):
    if n <= 1:
        return False
    
    for i in range(2, n):
        if n % i == 0:
            return False
    
    return True
 ```
# [24]
```python
   '''24. Найти количество слов в строке'''
def word_count(s):
    cnt = 0
    for i in s:
      if i == ' ':
        cnt +=1
    return(cnt+1)
 ```
# [25]
```python
'''25. Найти наибольшее число из двух'''
def max_of_two(a, b):
    if a > b:
      return a
    else:
      return b

print(max_of_two(3, 5))
 ```
# [26]
```python
'''26. Найти наименьшее число из двух'''
def min_of_two(a, b):
    if a>b:
      return b
    else: return a

print(min_of_two(3, 5))
 ```
# [27]
```python
'''27. Проверить, является ли строка числом'''
def is_number(s):
  cnt = 0
  for i in s:
    if i in ['0','1','2','3','4','5','6','7','8','9']:
      cnt += 1 
  if cnt == len(s):
      return True
  else: 
      return False
 ```
# [28]
 ```python
'''28. Найти длину строки'''
def string_length(s):
    return len(s)
 ```
 # [29]
 ```python
 '''29. Найти индекс последнего вхождения символа в строке'''
def last_occurrence(s, char):
    return s.rfind(char)

print(last_occurrence("hello", "l"))
 ```
# [30]
  ```python
 '''30. Проверить, является ли строка пустой'''
def is_empty(s):
    if s == '':
      return True
    else:
      return False
 ```
# [31]
```python
 '''31. Перевести строку в нижний регистр'''
def to_lower_case(s):
    return s.lower()
 ```
# [32]
```python
 '''32. Перевести строку в верхний регистр'''
def to_upper_case(s):
    return s.upper()
 ```
# [33]
```python
 '''33. Найти разницу двух списков'''
def list_difference(lst1, lst2):
    return [i for i in lst1 if i not in lst2]
 ```
# [34]
```python
 '''34. Найти индекс элемента в спискев'''
def find_index(lst, value):
  return lst.index(value)
 ```
# [35]
```python
 '''35. Проверить, является ли число четным'''
def is_even(n):
    if n % 2 == 0:
      return True
    else:
      return False
 ```
# [36]
```python
 '''36. Проверить, является ли число нечетным'''
def is_odd(n):
    if n % 2 != 0:
      return True
    else:
      return False
 ```
# [37]
```python
 '''37. Найти второе по величине число в списке'''
def second_largest(lst):
    return sorted(lst)[2]

 ```
 # [38]
```python
 '''38. Поменять местами два элемента списка'''
def swap_elements(lst, i, j):
    lst[i], lst[j] = lst[j], lst[i]
    return lst


 ```
# [39]
```python
 '''39. Найти общие элементы двух строк'''
def common_chars(s1, s2):
    return [i for i in s1 if i in s2]
 ```
# [40]
```python
 '''40. Проверить, является ли число палиндромом'''
def is_number_palindrome(n):
    if str(n) == str(n)[::-1]:
      return True
    else:
      return False
 ```
 # [41]
```python
 '''41. Найти сумму всех простых чисел до n'''
def sum_primes(n):
    if n < 2:
        return 0
    
    def is_prime(num):
        if num < 2:
            return False
        if num == 2:
            return True
        if num % 2 == 0:
            return False
        for i in range(3, int(num**0.5) + 1, 2):
            if num % i == 0:
                return False
        return True
    
    total = 0
    for i in range(2, n + 1):
        if is_prime(i):
            total += i
    return total
 ```
 # [42]
```python
 '''42. Удалить дубликаты из списка, сохранив порядок'''
def remove_duplicates_ordered(lst):
    return list(set(lst))
 ```
# [43]
```python
  '''43. Проверить, является ли строка панграммой'''
import string

def is_pangram(s):
  s_lower = s.lower()
  return len(set(string.ascii_lowercase) - set(s_lower)) == 0
 ```
 # [44]
 ```python
'''44. Найти пересечение двух списков'''
def list_intersection(lst1, lst2):
    return [i for i in lst1 if i in lst2]
 ```
# [45]
```python
'''45. Найти наибольшую общую подстроку двух строк'''
def list_intersection(lst1, lst2):
    return [i for i in lst1 if i in lst2]
 ```
# [46]
```python
'''46. Найти произведение всех элементов списка'''
def product_of_list(lst):
    c = 1
    for i in lst:
      c = i*c
    return c
 ```
 # [47]
```python
'''47. Проверить, является ли список монотонным'''
def is_monotonic(lst):
    increasing = decreasing = True
    
    for i in range(1, len(lst)):
        if lst[i] > lst[i-1]:
            decreasing = False
        if lst[i] < lst[i-1]:
            increasing = False
    
    return increasing or decreasing
 ```
# [48]
 ```python
'''48. Найти первый неповторяющийся символ в строке'''
from collections import Counter

def first_unique_char(s):
    count = Counter(s)
    
    for i in s:
        if count[i] == 1:
            return i
    return None
 ```
 # [49]
 ```python
 '''49. Найти все перестановки строки'''
from itertools import permutations

def string_permutations(s):
    return [''.join(p) for p in permutations(s)]
 ```
 # [50]
  ```python
 '''50. Проверить, можно ли составить строку из слов в списке'''
def can_form_string(words, target):
    return str(words[0] + words[1]) == target
 ```

</details>



<details>
  <summary>Задачи с собесов</summary>

<details>
  <summary>[1] Списки и циклы, базовые задачки [BASE]</summary>

 # [1]
  ```python
'''На вход программы подаётся список элементовe в случайном порядке. Необходимо вывести элементы в порядке RGB'''

color = ['black', 'blue', 'green', 'grey', 'red', 'rose']
r = []
g = []
b = []

for col in color:
  if col[0] == 'r':
   r.append(col)
  if col[0] == 'g':
   g.append(col)
  if col[0] == 'b':
   b.append(col)
   
r = sorted(r)
g = sorted(g)
b = sorted(b)

print(r + g + b)
 ```

  # [2]
  ```python
'''Даны две отсортированных по неубыванию последовательности целых чисел. Необходимо вернуть все элементы из первой последовательности, которых нет во второй'''

def FilterSortedList (seq1, seq2):
  return [i for i in seq1 if i not in seq2]
 ```

# [3]
  ```python
'''Написать функцию even_odd, которая принимает на вход один параметр limit. Функция должна возвращать все числа между 0 и limit (включительно) и для каждого числа указывать "чётное" или "нечётное"'''

def even_odd (limit):
  s = []
  for i in range (0,limit+1):
    status = 'Четное' if i % 2 == 0 else 'Нечетное'
    s.append(f"{i} - {status}")
  return s
 ```
# [4]
 Error, потому что 10 - это целое число (int)

# [5]
[10, 11, 12, 'a', 'b', 'c'], потому что extend() добавляет все элементы (в отличие от append, который добавляет только один элемент)

# [6]
[1, 2, [3, 10], 4] 
Здесь было непонятно, почему вывод не  [1, 2, [3], 4]. Потом нашла инфу, что у всех изменяемых типов (а здесь список [3] - изменяемый)  при присваивании создается ссылка, а у неизменяемых новый объект

# [7]
  ```python
'''Дан массив чисел. Написать код, который найдет первый локальный минимум и выведет его. Если локального минимума нет - ничего не выводить'''

def local_min (x):
  for i in range (len(x)):
    if x[i] < x[i-1] and x[i]<x[i+1]:
     return x[i]
local_min(X)
 ```

# [8]
01) 6 - 2
02) 1,2,3,4
    1,2,3,4
03) k,k,k,k,k,k,k,k,k
04) 'y', 'y5', 'y+y'

</details>



<details>
  <summary>[2] Множества [BASE]</summary>

# [1]
  ```python
'''На вход подается массив. Функция должна посчитать, сколько можно составить пар одинаковых элементов'''

'''Моя версия кода (не такой красивенький)'''

def func (x):
  cnt = 0
  cnt_2 = 0
  tmp = []
  dic = {}
  for i in x:
    if i not in tmp:
     tmp.append(i)
     for g in x:
       if i == g:
        cnt +=1
     if cnt % 2 ==0:
      cnt_2 += int(cnt/2)
     cnt = 0
  return cnt_2

'''Чатик подсказал решение получше'''
def func(arr):
    freq = {}
    for num in arr:
        freq[num] = freq.get(num, 0) + 1
    return sum(cnt // 2 for cnt in freq.values())
 ```

# [2]
  ```python
'''Дан массив целых чисел и число K. Найти два таких (не обязательно различных) числа в массиве, сумма которых равна K. Если таких чисел нет - вывести null или ничего'''

'''Моя версия кода (опять большой получился, но работает)'''

def func (input, k):
  flag = True
  s = []
  for i in range (len(input)):
    for g in range (i+1, len(input)):
      if input[i] + input[g] == k:
        s.append([input[i], input[g]])
        flag = True
      elif flag != True:
        flag = False
    if s == []:
      flag = False
  return s if flag != False else 'Таких чисел нет'
 ```
# [3]
  ```python
'''Получить пересечение этих двух списков без повторений'''

def func(list_a, list_b):
  return list(set([i for i in list_a if i in list_b]))
 ```

 # [4]
  ```python
'''Получить уникальные значения'''
def func(list_a):
  return list(set(list_a))

 ```

</details>



<details>
  <summary>[3] Строки [BASE]</summary>

# [1]
  ```python
'''Написать функцию на Python, которая принимает на вход строку и возвращает максимальное количество подряд идущих одинаковых символов в этой строке'''

def func (s):
  s = s + ' '
  cnt = 0
  k = []
  for i in range (len(s)-1):
    if s[i] == s[i+1]:
      cnt +=1
    else:
      k.append(cnt+1)
      cnt = 0
  return max(k)

func(s)
 ```

# [2]
  ```python
'''Нужно написать функцию, которая выполняет операцию RLE-сжатия, заменяя повторяющиеся символы на один символ и число его повторений'''

def func (s):
  cnt = 1
  s = s + ' '
  s1 = []
  for i in range (len(s)-1):
    if s[i] == s[i+1]:
      cnt += 1
    else:
      s1.append(s[i])
      s1.append(str(cnt))
      cnt = 1
  return ''.join(s1) 
func(s)
 ```

 # [3]
  ```python
'''Написать функцию, которая проверяет строчку на палиндром'''
def func (s):
  if s == s[::-1]:
    return True
  else:
    return False
func(s)
 ```

  # [4]
  ```python
'''Напишите код, который выведет максимальную последовательность повторяющихся символов в строке (сам символ и кол-во повторений'''
def func (s):
   max = 0
   s = s + ' ' 
   cnt = 1
   char = ''
   for i in range (len(s)-1):
      if s[i] == s[i+1]:
        cnt += 1
      else:
       if cnt > max:
         max = cnt
         char = s[i]
         cnt = 1
       else: 
        cnt = 1
   return max, char
func(s)
 ```
  # [5]
  ```python
'''Даны две строки. Нужно проверить, являются ли они анаграммами друг друга. Анаграмма - когда одна строка может быть получена из другой перестановкой букв'''
def func (s1, s2):
  if set(s1) == set(s2):
    return True
  else: 
    return False

func(s1, s2)
 ```
</details>

<details>
  <summary>[5] Словарь [BASE]</summary>

# [1]
  ```python
def func (d):
  for key in d:
    print (f"{key}: {d[key]}") 
  return
 ```
</details>

<details>
  <summary>[6] Работа с переменными [BASE]</summary>

# [1]

6 - 2 (значения поменялись)

</details>

<details>
  <summary>[7] Списки и циклы, продвинутые задачки [UPPER]</summary>

# [1]
  ```python
'''Написать программу для определения правильности скобочной последовательности'''
def check (s):
  s = list(s)
  flag = True
  for i in range(len(s)):
    if s[i] == '(':
      for g in range(i+1, len(s)):
        if s[g] == ')':
          s[i] = 0
          s[g] = 0
          break

    if s[i] == '[':
      for g in range(i+1, len(s)):
        if s[g] == ']':
          s[i] = 0
          s[g] = 0
          break

    if s[i] == '{':
      for g in range(i+1, len(s)):
        if s[g] == '}':
          s[i] = 0
          s[g] = 0
          break
  for i in s:
    if i == 0:
      flag = True
    else: 
      flag = False
      break

  return flag

 ```
# [2]
  ```python
'''Написать функцию, которая преобразует список чисел так, чтобы на месте каждого элемента стояло произведение всех чисел списка, кроме него самого'''

def func(s):
  l = 1
  result = []
  for i in range (len(s)):
    for g in range (len(s)):
      l = l*s[g]
    if s[i] == 0: l = l
    else: l = int(l/s[i])
    result.append(l)
    l = 1
  return (result)

 ```

 # [3]
  ```python
'''Требуется написать скрипт, который считает площади под кривыми и выводит их разницу'''

def trapezoidal_area(roc_curve):
    area = 0
    for i in range(len(roc_curve) - 1):
        x1, y1 = roc_curve[i]
        x2, y2 = roc_curve[i + 1]
        area += (y1 + y2) * (x2 - x1) / 2
    return area

print(abs(trapezoidal_area(A) - trapezoidal_area(B)))

 ```
</details>
</details>


<details>
  <summary>Алгосики</summary>

<details>
  <summary>Array / String</summary>

 # [1] - Easy
  ```python
''' '''


 ```

 # [2] - Easy
  ```python
''' '''


 ```

  # [3] - Easy
  ```python
''' '''


 ```

  # [4] - Medium
  ```python
''' '''


 ```

  # [5] - Medium
  ```python
''' '''


 ```

  # [6] - Medium
  ```python
''' '''


 ```

  </details>


<details>
  <summary>Two Pointers</summary>

 # [1] - Easy
  ```python
''' '''


 ```

 # [2] - Easy
  ```python
''' '''


 ```

  # [3] - Easy
  ```python
''' '''


 ```

  # [4] - Medium
  ```python
''' '''


 ```

  # [5] - Medium
  ```python
''' '''


 ```

  # [6] - Medium
  ```python
''' '''


 ```

  </details>


  <details>
  <summary>Sliding Window</summary>

 # [1] - Easy
  ```python
''' '''


 ```

 # [2] - Easy
  ```python
''' '''


 ```

  # [3] - Easy
  ```python
''' '''


 ```

  # [4] - Medium
  ```python
''' '''


 ```

  # [5] - Medium
  ```python
''' '''


 ```

  # [6] - Medium
  ```python
''' '''


 ```

  </details>






  </details>