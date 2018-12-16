---
layout: post
title:  "Tìm hiểu về Prototype trong Javascript"
date:   2018-12-15 21:00
categories: JavaScript
permalink: /javascript-prototype
excerpt: Prototype là một khái niềm nền tảng mà tất cả các Javascript developer đều phải hiểu rõ, và bài viết này tập trung vào việc giải thích prototype trong Javascript một cách chi tiết, đơn giản.
---

Có 2 concept liên quan đến `prototype` trong Javascript:

**1**. Thứ nhất, mọi function trong javascript đều có một **prototype property** (property này mặc định là empty), và bạn gán properties và methods lên prototype property này khi bạn muốn thực hiệc việc kế thừa. Prototype property này không phải là `enumerable`; có nghĩa là, nó không được truy cập trong vòng lặp `for...in`. Tuy nhiên Firefox và hầu hết các phiên bản của Safari và Chrome đều có một property giả là `__proto__` (một syntax thay thế), nó cho phép bạn truy cập một prototype property của object. Có thể bạn sẽ không bao giờ dùng property `__proto__`"pseudo" này, nhưng bạn nên biết là nó có tồn tại và nó là một cách đơn giản để truy cập một prototype property của object trên một vài trình duyệt. *The prototype property is used primarily for inheritance*; bạn thêm các method và property vào một function's prototype property để cung cấp các method và property đó cho các instance của function đó.

Nhìn vào ví dụ đơn giản về kế thừa với prototype property sau:

```javascript
function PrintStuff (myDocuments) {
  this.documents = myDocuments;
}

// chúng ta add method print() vào prototype property của function PrintStuff,
// các instaces (objects) khác có thể kế thừa nó:
PrintStuff.prototype.print = function() {
  console.log(this.documents);
}

// tạo một object mới với constructor PrintStuff(), và object này sẽ được kế thừa
// các properties và methods của PrintStuff
var newObj = new PrintStuff("I am a new Object and I can print.");

// newObj kế thừa tất cả properties và methods, bao gồm method print từ 
// function PrintStuff. Bây giờ newObj có thể gọi ngay method print mặc dù chúng ta
// chưa tạo method print () lên nó.
newObj.print(); // I am a new Object and I can print.
```

**2**. Concept thứ hai với prototype trong Javascript là **prototype attribute**. Nghĩ về prototype attribute như một đặc tính/ đặc điểm của object. Đặc tính này cho chúng ta biết tới object "parent". Nói một cách đơn giản: Một object prototype attribute trỏ tới object "parent" - object mà nó được kế thừa các properties. Prototype attribute thường được gọi là prototype object, và nó được thêm vào tự động khi ta tạo mới một object.

Để giải thích về điều này, một object kế thừa các properties từ một object khác thì object khác đó là object prototype attribute hay "parent". (Bạn có thể nghĩ prototype attribute như *the lineage* hoặc *the parent*). Trong đoạn code phía trên, prototype của `newObj` là PrintStuff.prototype.

Lưu ý: Tất cả các object đều có attributes giống như object properties có attributes. Và object attributes là *prototype*, *class*, và *extensible* attributes. Đây là prototype attribute mà ta sẽ thảo luận trong ví dụ thứ hai này.

Also note that the ``__proto__``"pseudo" property contains an object’s prototype object (the parent object it inherited its methods and properties from).

**Constructor**
Trước khi chúng ta tiếp tục, cùng lướt qua một chút về Constructor trước nhé. Một constructor là một function được sử dụng cho việc khởi tạo một object mới, và bạn sử dụng từ khóa `new` để gọi tới constructor.

Ví dụ:

```javascript
function Account() {
}

// đây là cách sử dụng Account constructor để tạo object userAccount.
var userAccount = new Account();
```

Hơn thế nữa, tất cả các objects mà kế thừa từ một object khác đều kế thừa một constructor property. Và constructor property đó đơn giản là một property (like any variable) giữ hoặc trỏ tới constructor của object.

```javascript
// constructor trong ví dụ này là Object()
var myObj = new Object();
// và nếu sau này bạn muốn tìm constructor của myObj:
console.log(myObj.constructor); // Object()

// một ví dụ khác: Account() là một constructor
var userAccount = new Account();
// tìm constructor của userAccount
console.log(userAccount.constructor); // Account()
```

## Prototype Attribute của các Oject được tạo bằng Constructor Function
Các object được tạo bằng từ khóa `new` với tất cả các constructor nào khác ngoại trừ `Object()` đều có thể nhận được prototype của constructor function đó.

Ví dụ:

```javascript
function Account(){

}

var userAccount = new Account(); // userAccount được khởi tạo với Account constructor và như vậy prototype attribute (hay prototype object) của nó là Account.prototype.
```

Tương tự, mọi array khởi tạo như `var myArray = new Array()` sẽ nhận prototype từ Array.prototype và kế thừa các Array.prototype property.

Vậy, có hai cách chung để một Object prototype attribute được chọn khi tạo một object là: 

1. Nếu một object được tạo với một object literal (`var newObj = {}`), nó kế thừa các property của Object.prototype và ta gọi prototype object (hay prototype attribute) của nó là Object.prototype.

2. Nếu một object được tạo với một constructor function chẳng hạn như `new Object()`,
`new Fruit()` hoặc `new Array()` hoặc `new Anything()`, nó sẽ kế thừa từ constructor đó (`Object()`, `Fruit()`, `Array()`, `Anything()`). Ví dụ, với một function như `Fruit()`, mỗi lần ta tạo một instance mới của Fruit (`var aFruit = new Fruit()`), prototype của instance mới này sẽ nhận prototype của Fruit constructor đó là Fruit.prototype. Bất kì đối tượng nào được tạo với `new Array()` sẽ có prototype là Array.prototype. Một object được tạo với `new Fruit()` sẽ có prototype là Fruit.prototype. Và bất kì object được tạo với Object constructor (`Obj()`, như `var anObj = new Object()`) sẽ kế thừa từ Object.prototype.

Điều quan trong cần biết là trong ECMAScript 5, bạn có thể tạo mới một object với method `Object.create()`, nó cho phép bạn thiết lập các object properties mới cho object. Chúng ta sẽ bàn về `Object.create()` trong những bài sau.

## Vì sao Prototype lại quan trọng và khi nào thì sử dụng chúng?
Đây là 2 cách quan trọng để Prototype được sử dụng trong Javascript, như đã nói phía trên:

**1. Prototype Property: Prototype-based Inheritance**

Prototype is important in JavaScript because JavaScript does not have classical inheritance based on Classes (như hầu hết các ngôn ngữ hướng đối tượng có), và vì thì mà kế thừa trong Javascript được thực hiện thông qua prototype property. Javascript có một cơ chế kế thừa dựa trên prototype-based. Kế thừa là một mô hình mà ở đó một object (hoặc Class ở vài ngôn ngữ khác) có thể kế thừa các properties và methods từ một object khác (hoặc Class). Trong Javascript, bạn thực hiện kế thừa bằng prototype property. Để ví dụ, bạn có tạo một function `Fruit` (một object, vì tất cả function trong Javascript đều là object), sau đó thêm các property và method vào prototype property của `Fruit` thì tất cả các instance của function `Fruit` sẽ nhận được tất cả các properties và methods của `Fruit`.

Cùng xem sự kế thừa trong Javascript:

```javascript
function Plant () {
  this.country = "Mexico";
  this.isOrganic = true;
}

// Add the showNameAndColor method to the Plant prototype property
Plant.prototype.showNameAndColor =  function () {
  console.log("I am a " + this.name + " and my color is " + this.color);
}

// Add the amIOrganic method to the Plant prototype property
Plant.prototype.amIOrganic = function () {
  if (this.isOrganic) console.log("I am organic, Baby!");
}

function Fruit (fruitName, fruitColor) {
  this.name = fruitName;
  this.color = fruitColor;
}

// Set the Fruit's prototype to Plant's constructor, thus inheriting all of Plant.prototype methods and properties.
Fruit.prototype = new Plant ();

// Creates a new object, aBanana, with the Fruit constructor
var aBanana = new Fruit ("Banana", "Yellow");

// Here, aBanana uses the name property from the aBanana object prototype, which is Fruit.prototype:
console.log(aBanana.name); // Banana

// Uses the showNameAndColor method from the Fruit object prototype, which is Plant.prototype. The aBanana object inherits all the properties and methods from both the Plant and Fruit functions.
console.log(aBanana.showNameAndColor()); // I am a Banana and my color is yellow.
```

Lưu ý: method `showNameAndColor` đã được kế thừa lên object `aBanana` vì `aBanana` kế thừa `Fruit` mà `Fruit` lại kế thừa `Plant` thông qua prototype.

Bất kì object nào sử dụng `Fruit` constructor đều sẽ kế thừa toàn bộ tất cả properties và methods của `Fruit.prototype` và của cả `Plant.prototype`. Đây là cách kế thừa chính được triển khai trong Javascript và vai trò không thể thiếu của prototype trong Javascript.

**2. Prototype Attribute: Accessing Properties on Objects**

Prototype cũng quan trọng trong việc truy cập đến các properties và methods của object. `prototype attribute` (hay prototype object) của bất kì object nào chính là `parent` object, nơi mà các thuộc tính kế thừa ban đầu được định nghĩa. This is loosely analogous to the way you might inherit your surname from your father—he is your “prototype parent.” If we wanted to find out where your surname came from, we would first check to see if you created it yourself; if not, the search will move to your prototype parent to see if you inherited it from him. If it was not created by him, the search continues to his father (your father’s prototype parent).
Similarly, if you want to access a property of an object, the search for the property begins directly on the object. If the JS runtime can’t find the property there, it then looks for the property on the object’s prototype—the object it inherited its properties from.

If the property is not found on the object’s prototype, the search for the property then moves to prototype of the object’s prototype (the father of the object’s father—the grandfather). And this continues until there is no more prototype (no more great-grand father; no more lineage to follow). This in essence is the prototype chain: the chain from an object’s prototype to its prototype’s prototype and onwards. And JavaScript uses this prototype chain to look for properties and methods of an object.

If the property does not exist on any of the object’s prototype in its prototype chain, then the property does not exist and undefined is returned.

This prototype chain mechanism is essentially the same concept we have discussed above with the prototype-based inheritance, except we are now focusing specifically on how JavaScript accesses object properties and methods via the prototype object.

This example demonstrates the prototype chain of an object’s prototype object:

```js
var myFriends = {name: "Pete"};

// To find the name property below, the search will begin directly on the myFriends object and will immediately find the name property because we defined the property name on the myFriend object. This could be thought of as a prototype chain with one link.
console.log(myFriends.name);

// In this example, the search for the toString () method will also begin on the myFriends’ object, but because we never created a toString method on the myFriends object, the compiler will then search for it on the myFriends prototype (the object which it inherited its properties from).

// And since all objects created with the object literal inherits from Object.prototype, the toString method will be found on Object.prototype—see important note below for all properties inherited from Object.prototype. 

myFriends.toString ();
```

## Object.prototype Properties Inherited by all Objects
Tất cả object trong Javascript đều kế thừa properties và methods từ Object.prototype. Các properties và methods được kế thừa là `constructor`, `hasOwnProperty()`, `isPrototypeOf()`, `propertyIsEnumerable()`, `toLocaleString()`, `toString()`, và `valueOf`. ECMAScript 5 also adds 4 accessor methods to Object.prototype.

Đây là một ví dụ khác về prototype chain:

```js
function People () {
this.superstar = "Michael Jackson";
}
// Define "athlete" property on the People prototype so that "athlete" is accessible by all objects that use the People () constructor.
People.prototype.athlete = "Tiger Woods";

var famousPerson = new People ();
famousPerson.superstar = "Steve Jobs";

// The search for superstar will first look for the superstar property on the famousPerson object, and since we defined it there, that is the property that will be used. Because we have overwritten the famousPerson’s superstar property with one directly on the famousPerson object, the search will NOT proceed up the prototype chain. 
console.log (famousPerson.superstar); // Steve Jobs

// Note that in ECMAScript 5 you can set a property to read only, and in that case you cannot overwrite it as we just did.

// This will show the property from the famousPerson prototype (People.prototype), since the athlete property was not defined on the famousPerson object itself.
console.log (famousPerson.athlete); // Tiger Woods

// In this example, the search proceeds up the prototype chain and find the toString method on Object.prototype, from which the Fruit object inherited—all objects ultimately inherits from Object.prototype as we have noted before.
console.log (famousPerson.toString()); // [object Object]
```

Tất cả các buit-in constructor (`Array()`, `Number()`, `String`, etc.) đều được tạo từ Object constructor, và tất nhiên chúng có prototype là Object.prototype.