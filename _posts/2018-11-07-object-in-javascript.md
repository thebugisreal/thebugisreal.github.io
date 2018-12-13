---
layout: post
title:  "Tìm hiểu về Object trong Javascript"
date:   2018-11-07 21:00
categories: JavaScript
permalink: /javascript-object
excerpt: Một object là một unordered list của các kiểu dữ liệu tham trị (và thỉnh thoảng là kiểu dữ liệu tham chiếu) được lưu trữ dưới dạng một loạt các cặp name-value. Mỗi một item trong list được gọi là property (functions gọi là methods).
---

## Reference Data Type và Primitive Data Types
Một trong những khác biệt chính giữa reference data type (kiểu dữ liệu tham chiếu) và primitive data type (kiểu dữ liệu tham trị) là giá trị của reference data type được lưu trữ dưới dạng một tham chiếu chứ không lưu trực tiếp giá trị lên biến như primitive data type.

Các primitive data type cơ bản của javascript là `String`, `Number`, `Boolean`, `undefined` và `null`. Mặt khác, các reference data type sẽ là `Object` và `Array`.

Để làm rõ hơn về sự khác nhau giữ 2 kiểu dữ liệu này, mời bạn xem các vị dụ dưới đây:

```javascript
// Primitive data type `String` lưu trữ dưới dạng giá trị.
var dog = 'Alaskan';
var anotherDog = dog; // gán cho giá trị biến dog

// before change
console.log(dog); // Alaskan
console.log(anotherDog); // Alaskan

// Bây giờ thử thay đổi giá trị biến dog ban đầu.
dog = 'Poodle';

// after changed
console.log(dog); // Poodle
console.log(anotherDog) // Alaskan
```

Điều cần chú ý trong ví dụ trên đó là khi thay đổi giá trị biến `dog` thành `Poodle` thì `anotherDog` vẫn giữ nguyên giá trị cũ - đây chính là primitive data type.

Cùng ví dụ trên, bây giờ ta sẽ thay primitive data type thành reference data type để xem điều gì sẽ xảy ra nhé. Ta sẽ áp dụng với kiểu dữ liệu Object.

```javascript
// Reference data type `Object` lưu trữ dưới dạng tham chiếu.
var dog = { name: 'Alaskan' };
var anotherDog = dog; // gán giá trị của object dog.

// before change
console.log(dog); // { name: 'Alaskan' }
console.log(anotherDog); // { name: 'Alaskan' }

// Bây giờ thử thay đổi giá trị object dog ban đầu.
dog.name = 'Poodle';

// after changed
console.log(dog); // { name: 'Poodle' }
console.log(anotherDog) // { name: 'Poodle' }
```

Sự khác biệt nằm ở đây, khi ta gán giá trị của biến `dog` vào biến `anotherDog` - vì giá trị của `dog` được lưu dưới dạng một tham chiếu chứ không phải một giá trị thực tế, nên khi ta thay đổi property `name` của biến `dog` thành một giá trị khác thì giá trị biến `anotherDog` cũng thay đổi theo. Có thể hiểu rằng 2 biến này cùng tham chiếu tới cùng 1 giá trị, thay đổi giá trị này sẽ thay đổi giá trị của tất cả những đối tượng khác đang tham chiếu tới nó.

## Object Data Properties have Attributes
Mỗi một data property không chỉ bao gồm một cặp name-value mà còn có 3 attributes (the three attributes are set to true by default):

- **Configurable Attribute**: chỉ định liệu property có thể xóa hoặc thay đổi hay không.
- **Enumerable**: chỉ định liệu property có thể returned trong một vòng lặp `for..in` hay không.
- **Writable**: chỉ định liệu property có thể thay đổi hay không.

## Phương thức khởi tạo Object
Có 2 cách thông thường để tạo 1 object.

**1. Sử dụng Object Literals**

Dùng cặp ngoặc nhọn để khai báo một object. Đây là cách đơn giản, thông dụng nhất.

```javascript
const dog = {
  color: 'yellow',
  legs: 4,
  hasTail: true,
  bark: function() {
    return 'Gâu Gâu'
  }
}
```

**2. Sử dụng Object Constructor**

Cách thông dụng thứ hai để tạo một object đó là sử dụng Object constructor. Một constructor là một function sử dụng để khởi tạo object mới, và bạn phải sử dụng keyword `new` để gọi constructor.

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

Object có thể chứa bất kì kiểu dữ liệu nào, bao gồm `Numbers`, `Arrays` hoặc ngay cả một `Object` khác.

## Tìm hiểu về Pattern để tạo Objects

Với các object đơn giản chỉ sử dụng một lần để lưu trữ data trong ứng dụng của bạn, 2 cách phía trên là đủ cho việc tạo object.

Thử tưởng tượng bạn có một ứng dụng bán chó con, bạn cần hiển thị một danh sách các chú chó kèm thông tin của từng con. Tất cả các chú chó đều có những thuộc tính chung như: loại chó, màu lông, cân nặng, giá bán và một function hiển thị giá bán. Sẽ thế nào nếu mỗi lần thêm 1 con bạn cần viết lại tất cả đoạn code dưới đây

```javascript
const dogA = {
  type: 'Alaskan',
  color: 'white',
  weight: 5,
  price: 1000000,
  showPrice: function() {
    console.log(`${this.type} is priced at ${this.price}`)
  }
}
```

Nếu bạn có 10 chú chó, bạn cần cần lặp lại đoạn code trên 10 lần. Nếu bạn muốn thay đổi một property nào đó trong object, bạn cũng sẽ cần thay đổi lần lượt cả 10 object. Như vậy rất tốn công sức và không ai làm như thế cả. Để giải quyết vấn đề dó, chúng ta sẽ sử dụng `pattern` - một khuôn mẫu chung cho tất cả các object có chung property.

Có 2 `pattern` thông thường để tạo objects:

- Sử dụng Constructor Pattern.
- Sử dụng Prototype Pattern.

Ngoài ra, với ES6 mới nhất ta có thể thay thế `constructor pattern` bằng cách sử dụng `classes`. Mình cũng sẽ giới thiệu luôn phía dưới đây.

**1. Sử dụng Constructor Pattern**

```javascript
// Khởi tạo Constructor pattern
function Dog (theType, theColor, theWeight, thePrice) {
  this.type = theType,
  this.color = theColor,
  this.weight = theWeight,
  this.price = thePrice,
  this.showPrice = function() {
    console.log(`${this.type} is priced at ${this.price}`)
  }
}
```

Với `pattern` phía trên, thật dễ dàng để tạo ra các chú chó khác nhau có cùng thuộc tính:

```javascript
const dogA = new Dog('alaskan', 'white', 5, 1000000);
dogA.showPrice() // alaskan is priced at 1000000

const dogB = new Dog('poodle', 'black', 3, 1200000);
dogB.showPrice() // poodle is priced at 1200000
```

Nếu bạn muốn thay đổi gì đó ví dụ như hàm `showPrice`, bạn chỉ cần thay đổi trong function `Dog` và tất cả các object kế thừa nó như `dogA`, `dogB` đều thay đổi theo.

**2. Sử dụng Prototype Pattern**
```javascript
function Dog() {
}

Dog.prototype.type = 'generic_type';
Dog.prototype.color = 'generic_color';
Dog.prototype.weight = 'generic_weight';
Dog.prototype.price = 'generic_price';
Dog.prototype.showPrice = function() {
  console.log(`${this.type} is priced at ${this.price}`);
}
```

Và đây là cách gọi `Dog` constructor trong `prototype pattern`:

```javascript
const dogA = new Dog();
dogA.type = 'Alaskan';
dogA.price = '1000000';
dogA.showPrice() // Alaskan is priced at 1000000
```

**3. Sử dụng classes trong ES6**
```javascript
class Dog {
  constructor(type, color, weight) {
    this.type = type;
    this.color = color;
    this.weight = weight;
  }
  setPrice(price) {
    this.price = price
  }
  showPrice() {
    console.log(`${this.type} is priced at ${this.price}`);
  }
}

const dogA = new Dog('Alaskan', 'yellow', 5)
dogA.setPrice(1000000);

const dogB = new Dog('Poodle', 'black', 3)
dogB.setPrice(1200000);

dogA.showPrice() // Alaskan is priced at 1000000
dogB.showPrice() // Poodle is priced at 1200000
```

## Own Properties và Inherited Properties của Object
Một cách khái quát, thuộc tính riêng (own property) là thuộc tính được định nghĩa tại bản thân của đối tượng (tại bản thân object), thuộc tính kế thừa (inherited property) là những thuộc tính được kế thừa từ đối tượng prototype của object đó. 

Để kiểm tra một thuộc tính có thuộc object hay không (kể cả thuộc tính riêng và thuộc tính kế thừa), ta dùng toán tử `in`:

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

## Từ khóa "this" trong Object
Khi một function được gán vào một property của object thì nó sẽ được gọi là method.
Method đó có con trỏ `this` đại diện cho value của object, hay nói cách khác context của method chính là object.

```javascript
const dog = {
  bark: 'Gâu Gâu', // property
  actionBark: function() { // method
    return this.bark
  }
}
console.log(dog.actionBark()) // 'Gâu Gâu'
```

Ta có 2 loại context là `Global context` và `Function context`.

Xem ví dụ dưới đây:

```javascript
const dog = {
  bark: 'Gâu Gâu',
  callback: function(callbackFunc) {
    callbackFunc();
  },
  actionBark: function() {     // (1)
    this.callback(function() { // (2)
      console.log(this.bark)
    })
  }
}
dog.actionBark() // undefined
```

Context của một function chính là đối tượng gọi tới function đó, hay con trỏ `this` của một function sẽ đại diện cho giá trị của đối tượng gọi tới nó.

Ta thấy, trong function (1), object `dog` chính là đối tượng gọi tới method `actionBark`, do đó ta có thể dùng `this` để trỏ tới các giá trị của object `dog`.

Trong function (2), lúc này đối tượng gọi tới nó không còn là object `dog` nữa. Context lúc này đã trở thành `global context`. Trên trình duyệt, `global context` chính là đối tượng `window`. Do `window` không có property `bark` nên giá trị trả về là `undefined`.

Để khắc phục vấn đề này, ta có 3 cách sau:

**1**. Gán con trỏ `this` vào một biến bên trong function (1), lúc này biến đó đã đại diện cho `this` (hay cho các giá trị của object `dog`). Ta có thể lồng nhiều function mà không cần lo đến context của nó nữa.

```javascript
const dog = {
  bark: 'Gâu Gâu',
  callback: function(callbackFunc) {
    callbackFunc();
  },
  actionBark: function() { // (1)
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
dog.actionBark() // Gâu Gâu
```

**2**. Trong ES5 cung cấp method `bind()` dùng để gán dữ liệu vào con trỏ `this` của function đang sử dụng. Khi truyền một object vào method `bind()` thì `this` sẽ đại diện cho các giá trị của object đó.

```javascript
const dog = {
  color: 'yellow',
  legs: 4,
  bark: 'Gâu Gâu',
  callback: function(callbackFunc) {
    callbackFunc();
  },
  actionBark: function() {
    this.callback(function() {
      console.log(this.bark)
    }.bind(this)) // truyền this vì context lúc này vẫn là object 'dog'
    // hoặc có thể truyền .bind(dog)
  }
}
dog.actionBark() // Gâu Gâu
```

**3**. Sử dụng arrow function trong ES6, vì arrow function không có context, nên context của function sẽ là context của hàm gần nhất (hàm mà function đó nằm trong)

```javascript
const dog = {
  bark: 'Gâu Gâu',
  callback: function(callbackFunc) {
    callbackFunc();
  },
  actionBark: function() {    // context là object dog
    this.callback(() => {     // context là function bao ngoài
      this.callback(() => {   // context là function bao ngoài
        this.callback(() => { // context là function bao ngoài 
          console.log(this.bark)
        })
      })
    })
  }
}
dog.actionBark() // Gâu Gâu
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

Giờ đây, chúng ta có thể sử dụng các method mới của object để chuyển 1 object thành 1 array trước khi tiến hành đưa lặp chúng.

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

Sau khi đã có được array gồm các giá trị mà ta mong muốn thì có thể thoải mái dùng các kiểu vòng lặp để xử lý nó rồi. Riêng method `Object.entries` trả về giá trị hơi đặc biệt, nên mình recommend sử dụng tính năng `Destructuring assignment` mới của ES6.

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

## Xóa Properties của một Object
Để xóa một property khỏi một object, ta sử dụng toán tử `Delete`. Tuy nhiên, bạn không thể xóa các properties được kế thừa, và cũng không thể xóa các properties với các attributes đã được đặt thành `configurable`. Bạn phải xóa inherited properties trên prototype object (nơi mà properties được định nghĩa). Ngoài ra, bạn không thể xóa properties của global object được khai báo bằng từ khóa `var`.

Toán tử `Delete` trả về `true` nếu đã delete thành công. Và cũng cần lưu ý kết quả cũng sẽ trả về `true` nếu property không tồn tại hoặc property không thể bị xóa (chẳng hạn như không thể cấu hình hoặc không thuộc sở hữu của object).

Xem ví dụ sau để dễ hiểu hơn nhé:

```javascript
var christmasList = {mike:"Book", jason:"sweater" }
delete christmasList.mike; // deletes property "mike"

// dòng vòng lặp for...in để xem tất cả properties của object.
for (var people in christmasList) {
	console.log(people);
}
// result: jason
// > mike đã xóa thành công

delete christmasList.toString; // true
// như đã nói ở trên, mặc dù kết quả trả về true nhưng method 
// toString không bị delete vì nó là một method kế thừa.

// thử gọi method toString và ta thấy nó vẫn hoạt động bình thường.
christmasList.toString(); //"[object Object]"
```

Bạn có thể xóa một property của một instance nếu property là một own property của instance đó. Xem ví dụ dưới đây:

```javascript
function HigherLearning () {
  this.educationLevel = "University";
}

// Implement inheritance with the HigherLearning constructor
var school = new HigherLearning ();
school.schoolName = "MIT";

console.log(school.hasOwnProperty("educationLevel")); // true

delete school.educationLevel; // true

// Chúng ta có thể delete property "educationLevel" khỏi object "school"
// bởi vì property "educationLevel" được định nghĩa trên instance đó (school).

// Khi dùng từ khóa "this" để tạo property khi định nghĩa function
// HigherLearning thay vì dùng prototype thì "educationLevel" là own property.

// kiểm tra sau khi xóa
console.log(school.educationLevel); // undefined

// Tuy đã xóa ra khỏi instance school, property "educationLevel" vẫn còn
// trên function HigherLearning.
var newSchool = new HigherLearning ();
console.log(newSchool.educationLevel); // University

// Nếu chúng ta định nghĩa một property trên function HigherLearning với prototype
// chẳng hạn như property educationLevel2 thì nó không phải own property:

HigherLearning.prototype.educationLevel2 = "University 2";

// instance school lúc này sẽ kế thừa property educationLevel2, 
// nhưng không phải own property

// check thử với method hasOwnProperty
console.log(school.hasOwnProperty("educationLevel2")); false
// check inherited property  
console.log(school.educationLevel2); // University 2

// thử xóa inherited property 
delete school.educationLevel2; // true (luôn luôn return true, như đã nói phía trên)

// và inherited property vẫn không bị xóa
console.log(school.educationLevel2); // University 2
```