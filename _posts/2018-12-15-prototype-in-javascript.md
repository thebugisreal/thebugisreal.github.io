---
layout: post
title:  "Tìm hiểu về Prototype trong Javascript"
date:   2018-12-15 21:00
categories: JavaScript
permalink: /javascript-prototype
excerpt: a
---

Có 2 concept liên quan đến `prototype` trong Javascript:

**1**. Thứ nhất, mọi function trong javascript đều có một **prototype property** (property này mặc định là empty), và bạn gán properties và methods lên prototype property này khi bạn muốn thực hiệc việc kế thừa. Prototype property này không phải là `enumerable`; có nghĩa là, nó không được truy cập trong vòng lặp `for...in`. Tuy nhiên Firefox và hầu hết các phiên bản của Safari và Chrome đều có một property giả là `__proto__` (một syntax thay thế), nó cho phép bạn truy cập một prototype property của object. Có thể bạn sẽ không bao giờ dùng property `__proto__`"pseudo" này, nhưng bạn nên biết là nó có tồn tại và nó là một cách đơn giản để truy cập một prototype property của object trên một vài trình duyệt. The prototype property is used primarily for inheritance; bạn thêm các method và property vào một function's prototype property để cung cấp các method và property đó cho các instance của function đó.

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

// newObj kế thừa tất cả properties và methods, bao gồm print method từ 
// function PrintStuff. Bây giờ newObj có thể gọi ngay print method mặc dù chúng ta
// chưa tạo print () method lên nó.
newObj.print(); // I am a new Object and I can print.
```

**2**. Concept thứ hai với prototype trong Javascript là **prototype attribute**. Nghĩ về prototype attribute như một đặc tính/ đặc điểm của object. Đặc tính này cho chúng ta biết object "parent". Nói một cách đơn giản: Một object prototype attribute trỏ tới object "parent" - object mà nó được kế thừa các properties. Prototype attribute thường được gọi là prototype object, và nó được thêm vào tự động khi ta tạo mới một object.
Để giải thích về điều này, một object kế thừa các properties từ một object khác thì object khác này là object prototype attribute hay "parent". (Bạn có thể nghĩ prototype attribute như *the lineage* hoặc *the parent*). Trong đoạn code phía trên, prototype của `newObj` là PrintStuff.prototype.

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