---
layout: post
title:  "Tìm hiểu về Object trong Javascript"
date:   2018-11-07
categories: JavaScript
permalink: /javascript/object
---

## Phương thức khởi tạo Object
Có 3 cách để tạo 1 object.

### 1. Sử dụng Object Literals

Là dùng cặp ngoặc nhọn để khai báo một object.

```javascript
const dog = {
  color: 'yellow',      // String
  legs: 4,              // Number  
  hasTail: true,        // Boolean
  bark: function() {    // Function
    return 'Gâu Gâu'
  }
}
```

### 2. Sử dụng Object Constructor

Cách này dùng method khởi tạo Constructor của kiểu dữ liệu Object để tạo ra các object. Method này là một hàm để tạo ra các object mới - sử dụng với keyword `new`

```javascript
// Tạo 1 object mới
const dog = new Object();
// Thêm property cho object
dog.color = 'yellow';
dog.legs = 4;
dog.hasTail = true;
dog.bark = function() {
  return 'Gâu Gâu';
}
```

**Tạo hàng loạt Object cùng loại**

Nếu bạn chỉ cần tạo các object riêng biêt, 2 cách phía trên là đủ cho bạn. Nhưng nếu bạn đang cần tạo 1 loạt các object giống nhau (cùng property) thì mình xin giới thiệu 2 cách nữa để làm việc này, đó là:

- Sử dụng prototype (ES5).
- Sử dụng classes (ES6).

### 3. Sử dụng prototype trong ES5
```javascript
function Animals() {
}
// Tạo prototype
Animals.prototype.name = 'general_name';
Animals.prototype.bark = 'general_bark';
Animals.prototype.getBark = function() {
  console.log(`${this.name} barking ${this.bark}`);
}
// Tạo object
const dog = new Animals();
dog.name = 'Alaskan';
dog.bark = 'gâu gâu';
dog.getBark() // Alaskan barking gâu gâu

console.log(dog) // Animals { name: 'Alaskan', bark: 'gâu gâu' }
```

### 4. Sử dụng classes trong ES6
```javascript
class Animals {
  constructor(name, color) {
    this.name = name;
    this.color = color;
  }
  setBark(bark) {
    this.bark = bark
  }
  getBark() {
    return this.bark;
  }
}
// Tạo object với từ khóa new
const dog = new Animals('Alaskan', 'yellow')
dog.setBark('Gâu Gâu');

const cat = new Animals('Doraemon', 'blue')
cat.setBark('Meo Meo');

console.log(dog) 
// Animals { name: 'Alaskan', color: 'yello', bark: 'Gâu Gâu' }
console.log(cat) 
// Animals { name: 'Doraemon', color: 'blue', bark: 'Meo Meo' }
console.log(dog.getBark()) // Gâu Gâu
console.log(cat.getBark()) // Meo Meo
```

## Từ khóa "this" trong Object
Khi một function được gán vào một property của object thì nó sẽ được gọi là method.
Method đó có con trỏ `this` đại diện cho value của object, hay nói cách khác context của method chính là object.

```javascript
const dog = {
  bark: 'Gâu Gâu', // property
  handleBark: function() { // method
    return this.bark
  }
}
console.log(dog.handleBark()) // 'Gâu Gâu'
```

Ta có 2 loại context là `Global context` và `Function context`.

Xem ví dụ dưới đây:

```javascript
const dog = {
  bark: 'Gâu Gâu',
  callback: function(callbackFunc) {
    callbackFunc();
  },
  handleBark: function() {     // (1)
    this.callback(function() { // (2)
      console.log(this.bark)
    })
  }
}
dog.handleBark() // undefined
```

Cần lắm được, context của một function chính là đối tượng gọi function đó, hay `this` của function sẽ đại diện cho giá trị của đối tượng gọi function đó.

Ta thấy, trong function (1), object `dog` chính là đối tượng gọi tới method `handleBark`, do đó ta có thể dùng `this` để trỏ tới các giá trị của object `dog` - cụ thể trong ví dụ là trỏ tới method `callback` => code chạy tới đây vẫn ok.

Trong function (2), lúc này đối tượng gọi tới nó không còn là object `dog` nữa. Context lúc này đã trở thành `global context`. Trên trình duyệt, `global context` chính là đối tượng `window`. Do `window` không có property `bark` nên giá trị trả về là `undefined`.

Để khắc phục vấn đề này, ta có 3 cách sau:

[1]. Gán con trỏ `this` vào một biến bên trong function (1), lúc này biến đó đã đại diện cho `this` (hay cho các giá trị của object `dog`). Ta có thể lồng nhiều function mà không cần lo đến context của nó nữa.

```javascript
const dog = {
  bark: 'Gâu Gâu',
  callback: function(callbackFunc) {
    callbackFunc();
  },
  handleBark: function() { // (1)
    const that = this;
    that.callback(function() {     
      that.callback(function() {
        that.callback(function() {
          console.log(that.bark)
        })
      })
    })
  }
}
dog.handleBark() // Gâu Gâu
```

[2]. Trong ES5 cung cấp method `bind()` dùng để gán dữ liệu vào con trỏ `this` của function đang sử dụng. Khi truyền một object vào method `bind()` thì `this` sẽ đại diện cho các giá trị của object đó.

```javascript
const dog = {
  color: 'yellow',
  legs: 4,
  bark: 'Gâu Gâu',
  callback: function(callbackFunc) {
    callbackFunc();
  },
  handleBark: function() {
    this.callback(function() {
      console.log(this.bark)
    }.bind(this)) // truyền this vì context lúc này vẫn là object 'dog'
    // hoặc có thể truyền .bind(dog)
  }
}
dog.handleBark() // Gâu Gâu
```

[3]. Sử dụng arrow function trong ES6, vì arrow function không có context, nên context của function sẽ là context của hàm gần nhất (hàm mà function đó nằm trong)

```javascript
const dog = {
  bark: 'Gâu Gâu',
  callback: function(callbackFunc) {
    callbackFunc();
  },
  handleBark: function() {    // context là object dog
    this.callback(() => {     // context là function bao ngoài
      this.callback(() => {   // context là function bao ngoài
        this.callback(() => { // context là function bao ngoài 
          console.log(this.bark)
        })
      })
    })
  }
}
dog.handleBark() // Gâu Gâu
```

## Đếm số lượng key trong Object
Để làm việc này, chúng ta sử dụng method `Object.keys()`. Truyền vào trong nó một object, kết quả trả về là một mảng gồm các key của object đó. Thứ tự các key trong mảng lần lượt từ `Number` -> `String`.

```javascript
const dog = {
  color: 'yellow',
  legs: 4,
  hasTail: true,
  123: 'test'
}
Object.keys(dog) // [123, 'color', 'legs', 'hasTail']
Object.keys(dog).length // 9
```

**Lưu ý**: nếu truyền vào Object.keys() một: 

- Object không tồn tại. *Ex: Object.keys(dog2) => undefined*
- Number. *Ex: Object.keys(123) => []*
- String. *Ex: Object.keys('hello') => ['0', '1', '2', '3', '4']*

## Lặp một object
Trước khi có ES6, chúng ta thường sử dụng vòng lặp `for...in` để thực hiện việc lặp qua một object, nhưng cách này có 1 điểm hạn chế đó là khi vòng lặp chạy nó sẽ lặp qua tất cả các thuộc tính trong chuỗi Prototype - vì vậy cần bổ sung thêm method `hasOwnProperty` để check xem propery có thuộc về object hay không. 

Giờ đây, thật may mắn khi ta có thể lặp qua object một cách dễ dàng. Đầu tiên, chúng ta sẽ sử dụng các method mới của object để chuyển 1 object thành 1 array theo yêu cầu mà mình mong muốn.

Có 3 method mà mình muốn giới thiệu đó là:

- `Object.keys()`
- `Object.values()`
- `Object.entries()`

**Oject.keys()**

Truyền vào method `Object.keys()` một object, giá trị trả về sẽ là một mảng bao gồm các property của object đó.

```javascript
const animals = {
  one: 'dog',
  two: 'cat',
  three: 'bird'
}

const toArray = Object.keys(animals); // ['one', 'two', 'three']
```

**Object.values()**

Truyền vào method `Object.values()` một object, giá trị trả về sẽ là một mảng bao gồm các value của object đó.

```javascript
const animals = {
  one: 'dog',
  two: 'cat',
  three: 'bird'
}

const toArray = Object.values(animals); // ['dog', 'cat', 'bird']
```

**Object.entries()**

Truyền vào method `Object.entries()` một object, giá trị trả về sẽ là một mảng chứa các mảng con - mỗi mảng con sẽ gồm 2 giá trị là key và value của object.

```javascript
const animals = {
  one: 'dog',
  two: 'cat',
  three: 'bird'
}

const toArray = Object.entries(animals); 
// [ ['one', 'dog'], ['two', 'cat'], ['three', 'bird'] ]
```

Sau khi đã có được array gồm các giá trị mà ta mong muốn thì có thể thoải mái dùng các kiểu vòng lặp để xử lý nó rồi. Riêng có method `Object.entries` trả về giá trị hơi đặc biệt, nên hãy dùng tính năng `Destructuring assignment` mới của ES6 cho clean code hơn.

```javascript
const animals = {
  one: 'dog',
  two: 'cat',
  three: 'bird'
}

const toArray = Object.entries(animals); 

for (const [key, value] of arr) {
  console.log(`${key} has value is ${value}`)
}
```

## Kiểu dữ liệu tham trị và tham chiếu
Sự khác biệt cơ bản giữa kiểu dữ liệu tham trị và kiểu tham chiếu đó là: giá trị của kiểu dữ liệu tham chiếu được lưu trữ như là “một tham chiếu”, tức là giá trị của biến sẽ không được lưu trực tiếp tại biến, mà biến đó sẽ lưu một tham chiếu tới giá trị thực.

Các kiểu dữ liệu cơ bản trong Javascript là các kiểu dữ liệu tham trị, còn kiểu dữ liệu Object sẽ là kiểu dữ liệu tham chiếu. Minh hoạ kiểu tham trị như sau:

```javascript
// Kiểu dữ liệu cơ bản lưu giá trị thực sự (tham trị)
var num = 1;
var otherNum = num;  // bây giờ otherNum có giá trị là 1
 
// Thử thay đổi giá trị
num = 2;
console.log(num);       // 2
console.log(otherNum);  // 1
```

Như đã thấy ở trên, các kiểu dữ liệu cơ bản trong Javascript sẽ lưu giá trị theo kiểu tham trị. Ngược lại, kiểu dữ liệu Object sẽ lưu giá trị kiểu tham chiếu:

```javascript
// Khai báo 2 đối tượng kiểu Object
var person = {name: "John"};
var otherPerson = person;
 
// Thử thay đổi giá trị
person.name = "Peter";
console.log(person.name);       // Peter
console.log(otherPerson.name);  // Peter
```

Như đã thấy, mặc dù thay đổi giá trị của biến `person`, nhưng giá trị của biến `otherPerson` cũng bị thay đổi. Nguyên do của việc này là bởi các đối tượng Object lưu giá trị theo kiểu tham chiếu, tức là 2 biến này cùng tham chiếu tới 1 giá trị, thay đổi giá trị này sẽ thay đổi giá trị thuộc tính của tất cả những đối tượng khác đang tham chiếu tới nó.

## Thuộc tính riêng và thuộc tính kế thừa của Object
Một cách khái quát, thuộc tính riêng (own property) là thuộc tính được định nghĩa tại bản thân của đối tượng (tại bản thân object), thuộc tính kế thừa (inherited property) là những thuộc tính được kế thừa từ đối tượng prototype của object đó. 

Để kiểm tra một thuộc tính có thuộc object hay không (kể cả thuộc tính riêng và thuộc tính kế thừa), ta dùng toán tử “in”:

```javascript
var school = {schoolName: 'Havard'};
 
// kiểm tra thuộc tính
console.log('schoolName' in school); // true
console.log('schoolType' in school); // false
```

Để kiểm tra một thuộc tính có phải là thuộc tính riêng hay không, ta có thể dùng phương thức “hasOwnProperty” của Object.

```javascript
// tạo mẫu khởi tạo
function Fruit(){
};
Fruit.prototype.color = 'general_color';
 
// tạo đối tượng
var apple = new Fruit();
// tạo thuộc tính riêng của object
apple.name = 'ownName';
// kiểm tra thuộc tính
console.log(apple.hasOwnProperty('color')); // false
console.log(apple.hasOwnProperty('name')); // true
```

Ta có thể nhận định thêm rằng, các thuộc tính định nghĩa trong prototype sẽ được kế thừa tới mọi object, nhưng ta vẫn có thể thêm các thuộc tính riêng cho từng object khác nhau. Đây là một trong những đặc điểm rất đặc biệt của thuộc tính prototype trong Javascript.