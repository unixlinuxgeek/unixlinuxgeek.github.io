# 11 часто используемых методов массива.

В JavaScript массивы представляют собой структуру данных, которая содержит список значений. Эти значения могут быть строками, целыми числами или объектами. Массивы не имеют фиксированных типов или ограниченной длины.
Существует много различных методов, 
но мы рассмотрим наиболее часто используемые методы. 


## Методы

### pop()

Удаляет последний элемент в массиве и возвращает удаленный элемент.

```js
let marks = ["BMW", "Lexus", "Mercedes"];
marks.pop();
console.log(marks);
// Вывод: ["BMW", "Lexus"];
```

### map()

Создает новый массив с результатом выполнения предоставленной функции для каждого элемента в массиве

```js
let numbers = [100, 300, 50, 200];
let addHundred = numbers.map(element => element + 100);
// Вывод: [200, 400, 150, 300]
```
*На заметку, разница между .forEach и .map заключается в том, что когда вы вызываете метод .forEach, он возвращает undefined, а .map возвращает новый массив!*

### push()

Добавляет элемент в конец массива

```js
let os = ["Android","IOS","Linux"];
os.push("Windows");
// Вывод: ["Android","IOS","Linux", "Windows"]
```

### splice()

Добавляет, удаляет или заменяет элемент по указанному индексу. Первый параметр - это индекс, второй параметр - сколько элементов вы хотите удалить, а третий параметр - это значение, которое вы хотите заменить или добавить.

```js
// Удаляем элементы
let names = ["Alex", "Alan", "Kate","Alice"];
names.splice(1,2);
console.log(names);
// Вывод: ["Alex", "Alice"]
```
```js
// Добавление элемента
let names = ["Alex", "Alan", "Kate","Alice"];
names.splice(2,0,"John");
console.log(names);
//  Вывод: ["Alex", "Alan", "John", "Kate", "Alice"]
```
```js
// Заменить элементы
let marks = ["BMW", "Lexus", "Mercedes", "Audi", "Ford"];
marks.splice(2,1, "Toyota");
console.log(marks);
// Вывод:  ["BMW", "Lexus", "Toyota", "Audi", "Ford"]
```

### slice()

Возвращает поверхностную копию массива. Он может принимать один или два параметра, которая представляет «начало» и «конец» того места, где вы хотите срезать. Обратите внимание, что параметр «begin» включен, тогда как параметр «end» не должен быть включен.

```js
let cities = ["Washington", "Rome", "Paris", "Berlin"];

let europeanСities = cities = cities.slice(1,3);
console.log(europeanСities);
// Вывод: ["Rome", "Paris"]
```

### filter()

Создает новый отфильтрованный массив.

```js
let menuItems = ["home", "news","about"];
let filteredItems = menuItems.filter(item => item === 'Home');
console.log(filteredItems);
// Вывод:  ["Home"]
```

### forEach()

Выполняет функцию для каждого элемента в массиве.

```js
let numbers = [1,2,3,4,5];
numbers.forEach(element => console.log(element));
```

### sort()

```js
let numbers = [9,5,1,8,2,10000];
numbers.sort((a,b) => {
  if (a < b) {
    return -1;
  }
  if (a > b) {
    return 1;
  }
  return 0;
});
console.log(numbers);
// Вывод: [1, 2, 5, 8, 9, 10000]
```

### reduce()

Объединяет (или уменьшает) массив в одно единственное значение. Простой способ изучить метод Reduce - это сложить все элементы в массиве. Метод принимает два параметра: аккумулятор и текущее значение.

```js
let numbers = [33, 45, 76];
let reducer = numbers.reduce((accumulator, currentValue) => accumulator+currentValue);
console.log(reducer);
// Вывод: 154
```

### unshift()

добавляет элемент в начало массива

```js
let distance = [50, 45, 21];
distance.unshift(22);
console.log(distance);
// Вывод: [22, 50, 45, 21]
```
### shift()

Удаляет первый элемент из массива и возвращает этот элемент.

```js
let motorcycles = ['Ducati', 'Suzuki', 'BMW', 'Kawasaki'];
motorcycles.shift();
console.log(motorcycles);
// Вывод:  ["Suzuki", "BMW", "Kawasaki"]
```