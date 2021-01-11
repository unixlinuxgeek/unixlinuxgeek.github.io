# Понимание прототипов в JavaScript.

Объекты в JavaScript имеют внутреннее свойство, известное как прототип. Это просто ссылка на другой объект и содержит общие атрибуты / свойства для всех экземпляров объекта. Атрибут прототипа объекта указывает объект, от которого он наследует свойства. Например:

```js
let num = [1, 2, -6, 9, 3, 8]
```

Объект Array имеет прототип Array.prototype, а экземпляр объекта num наследует свойства объекта Array.

```js
console.log(num)
    0: 1
    1: 2
    2: -8
    3: 3
    4: -4
    5: 7
    length: 6
    __proto__: Array(0)
        concat: ƒ concat()
        constructor: ƒ Array()
        copyWithin: ƒ copyWithin()
        entries: ƒ entries()
        every: ƒ every()
        fill: ƒ fill()
        filter: ƒ filter()
        find: ƒ find()
        findIndex: ƒ findIndex()
        flat: ƒ flat()
        flatMap: ƒ flatMap()
        forEach: ƒ forEach()
        includes: ƒ includes()
        indexOf: ƒ indexOf()
        join: ƒ join()
        keys: ƒ keys()
        lastIndexOf: ƒ lastIndexOf()
        length: 0
        map: ƒ map()
        pop: ƒ pop()
        push: ƒ push()
        reduce: ƒ reduce()
        reduceRight: ƒ reduceRight()
        reverse: ƒ reverse()
        shift: ƒ shift()
        slice: ƒ slice()
        some: ƒ some()
        sort: ƒ sort()
        splice: ƒ splice()
        toLocaleString: ƒ toLocaleString()
        toString: ƒ toString()
        unshift: ƒ unshift()
        values: ƒ values()
        Symbol(Symbol.iterator): ƒ values()
        Symbol(Symbol.unscopables): {copyWithin: true, entries: true, fill: true, find: true, findIndex: true, …}
        __proto__: Object
```

Вот почему вы можете использовать метод вроде sort () для экземпляра массива.
То же самое относится и к другим объектам, таким как Date(), String(), Function() и т. д.

Когда строится функция конструктора (псевдоклассическое наследование), вновь созданные объекты наследуют свойства прототипа функции конструктора, и это является критической особенностью конструкторов. Они (функции конструктора) создаются для инициализации вновь созданных объектов. Конструкторы вызываются с помощью нового ключевого слова.

```js
const Car = function (color, model, dateManufactured) {
    this.color = color
    this.model = model
    this.dateManufactured = dateManufactured
}
Car.prototype.getColor = function () {
    return this.color
}
Car.prototype.getModel = function () {
    return this.model
}
Car.prototype.carDate = function () {
    return `Этот ${this.model} был произведен в ${this.dateManufactured} году`
}
let firstCar = new Car('red', 'Porshe', '1980')
console.log(firstCar)
console.log(firstCar.carDate()) // Этот Porshe был произведен в 1985 году.
```

Из приведенного выше примера функции carDate, getColor и getModel являются свойствами объекта Car, и экземпляры объекта Car, такие как firstCar, могут наследовать все его свойства.

## Использование свойства прототипа

Свойство прототипа JavaScript позволяет добавлять новые свойства в конструкторы объектов:

```js
function Person(first, last, age, eyecolor) {
    this.firstName = first
    this.lastName = last
    this.age = age
    this.eyeColor = eyecolor
}
Person.prototype.nationality = 'English'
```

Свойство прототипа JavaScript также позволяет добавлять новые методы в конструкторы объектов:

```js
function Employee(first, last, age, eyecolor, position) {
    this.firstName = first
    this.lastName = last
    this.age = age
    this.eyeColor = eyecolor
    this.position = position
}

Employee.prototype.name = function () {
    return this.firstName + ' ' + this.lastName
}
```

Изменяйте только свои собственные прототипы. Никогда не изменяйте прототипы стандартных объектов JavaScript.

## Цепочка прототипов

Когда объект получает запрос на свойство, которого у него нет, в его прототипе будет выполняться поиск свойства, затем прототип прототипа и т. Д. Так кто же является прототипом объекта? Это великий наследственный прототип, сущность почти всех объектов, Object.prototype. Многие объекты непосредственно не имеют Object.prototype в качестве своего прототипа, но вместо этого имеют другой объект, который предоставляет другой набор свойств по умолчанию. Функции наследуются от Function.prototype, а массивы - от Array.prototype и так далее.

```js
let protoParrot = {
    color: 'grey',
    speak(line) {
        console.log(`The ${this.type} parrot says ${line}`)
    },
}
let killerParrot = Object.create(protoParrot)
killerParrot.type = 'assassin'
killerParrot.speak('chirps')
// → The assassin rabbit says chirps!
```

ProtoParrot действует как контейнер для свойств, которые являются общими для всех попугаев. Отдельный объект parrot, например killerParrot, содержит свойства, которые применяются только к самому себе (в данном случае к его типу), и извлекает общие свойства из своего прототипа. Давайте посмотрим на этот код:

```js
let mainObject = {
    bar: 2,
}
// создать объект, связанный с другим объектом
let myObject = Object.create(mainObject)
for (let k in myObject) {
    console.log('found: ' + k)
}
// найден: bar
'bar' in myObject // true
```

Но, если bar не найден в myObject, то поиск пойдет по его цепочке прототипов. Этот процесс продолжается до тех пор, пока не будет найдено соответствующее имя свойства или цепочка прототипов не закончится. Если к концу цепочки не найдено подходящего свойства, возвращаемый результат операции не определен. Подобно этому процессу поиска цепочки прототипов, если вы используете цикл for..in для итерации по объекту, будет перечислено любое свойство, которое может быть достигнуто через его цепочку и также перечислимо. Если вы используете оператор in для проверки существования свойства объекта, он проверит всю цепочку объекта (независимо от перечисляемости).

## Примечание:

```js
let protoParrot = {
    color: 'grey',
    speak(line) {
        console.log(`The ${this.type} parrot says ${line}`)
    },
}
let killerParrot = Object.create(protoParrot)
killerParrot.type = 'assassin'
killerParrot.speak('chirps!')
```

Метод, описанный в приведенном выше коде, эффективен, потому что, если бы вы создали несколько объектов parrot, у вас была бы одна и та же функция, у каждого объекта, и именно здесь действительно приходят функции прототипа и конструктора. Это можно переписать так:

```js
let protoParrot = function (color, word, type) {
    this.color = color
    this.word = word
    this.type = type
}
protoParrot.prototype.getColor = function () {
    return this.color
}
protoParrot.prototype.speak = function () {
    console.log(`The ${this.type} parrot says ${this.word}`)
}
let killerParrot = new protoParrot('grey', 'chirps!', 'assassin')
killerParrot.speak()
```

Это базовая информация по прототипам JavaScript. Я помог вам понять что такое прототипы и как их использовать.
