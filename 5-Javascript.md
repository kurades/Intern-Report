# Javascript vài điều cần biết

## Bài rất dài nên sẽ có Table of content

- [Kiểu dữ liệu](#kiểu-dữ-liệu-trong-js)
  1. [Number](#1-number)
  2. [String](#2-string)
  3. [Boolean](#3-boolean)
  4. [undefined và null](#4-null-và-undefined)
  5. [Object](#5-object)
- [Vòng lặp và switch case](#vòng-lặp)
  1. [Vòng lặp](#vòng-lặp-while)
  2. [switch case](#lệnh-switch)
- [If else](#if-else)
- [Function/Arrow Function](#functionarrow-function)
- [Logic](#toán-tử-logic)
  1. [OR](#or)
  2. [AND](#and)
  3. [NOT](#not)
  4. [??](#kiểm-tra-tồn-tại-giá-trị)
- [Xử lý lỗi](#xử-lý-lỗi-bằng-trycatch)
- [Object advance](#object)
- [Async](#asynchronous)
  1. [callback](#callback)
  2. [promise](#promise)
  3. [async/await](#asyncawait)
- [Import/Export](#export-và-import)
  1. [export](#export-default)
  2. [import](#import)
  3. [trường hợp khác](#các-trường-hợp-thông-dụng)
- [Class](#class)
  1. [class expression](#class-expression)
  2. [getter and setter](#getter-và-setter)
  3. [inheritance](#class-inheritance)
  4. [static](#static-propertymethod)
  5. [protected](#protected)
  6. [private](#private)
- [Date time](#date-time)
### Kiểu dữ liệu trong JS
Có một điều đặc biệt là biến(variable) trong js không cần định nghĩa kiểu dữ liệu, chúng ta có thể khai báo biến với một giá trị bất kỳ, lấy ví dụ:
```Javascript
    let a = 'Hello work :)';
    b = 6996;
```
#### 1. Number
```Javascript
    let n = 1234;
    n = 12.345;
```
Kiểu số trong js có thể biểu diễn theo 2 kiểu integer hoặc float, các số đều có thể `+` `-` `*` `/` với nhau mà không cần ép kiểu sang cùng một loại, ví dụ :
```Javascript
    let sum = 12.345 + 21; //ok
    let div = 543/12.21; //ok nốt
```
Có một giá trị đặc biệt của number cần lưu ý, đó là `NaN`. Nó biểu diễn cho một phép toán lỗi, ví dụ `'chuỗi'/12 = NaN` 

`NaN` thực hiện phép toán nào cũng ra giá trị `NaN` ngoại trừ `NaN ** 0 = 1`

Một vài cách khai báo number : 
```Javascript
    let n = 1_000_000_000; // 1000000000
    n = 3e4; // 3 * 10000
    n = 0xff; // dạng hex của 255
    n = 0b11111111; // dạng bin của 255
    n = 0o377; // dạng oct của 255
    //.....
```

Chuyển đổi dạng từ số sang hệ cơ số
```Javascript
    let num = 255;
    let hex = num.toString(16);  // ff
    let bin = num.toString(2);   // 11111111
    //.....
```

Chuyển đổi họ hàng của số sang số
```Javascript
    parseInt('100px'); //100
    parseFloat('12.21em'); //12.21
    parseInt('12.22'); //12
    parseFloat('12.3.4'); //12.3
    parseInt('0xff', 16); //255
    parseInt('ff', 16); // không có 0x cũng ok nốt
```
hàm parse chỉ chuyển đổi các giá trị có giá trị đầu là số, nếu giá trị đầu tiên không phải là số nó sẽ xảy ra lỗi : 
``` Javascript
    parseInt('a123'); //NaN
```

#### 2. String
String trong js được thể hiện ở trong các dấu ngoặc `' '`, `" "` hoặc `` ` ` ``.

Dấu `` ` `` là một dấu đặc biệt, nó có thể nhúng hàm hoặc biến trong nó, ví dụ :
```Javascript
    let name = "John";

    // embed a variable
    alert( `Hello, ${name}!` ); // Hello, John!

    // embed an expression
    alert( `the result is ${1 + 2}` ); // the result is 3
```

#### 3. Boolean
Chỉ có 2 giá trị `true` hoặc `false`, cách khai báo rất đơn giản: 
```Javascript
    let nameFieldChecked = true; // yes, name field is checked
    let ageFieldChecked = false; // no, age field is not checked
```

Boolean cũng có thể khai báo bằng cách cho một hàm so sánh hoặc một hàm logic :
```Javascript
    let isGreater = 4 > 1; //true
    let logic = true || false; //true
    let logic2 = true && false; //false
```
**Truthy và falsy**

Mỗi giá trị điều có khả năng chuyển thành dạng true và false. Các giá trị **_falsy_** có thể là `false` `0` `-0` `` "",'', ` ` ``  `null` `undefined` `NaN`. Những giá trị khác ngoài danh sách này là **_truthy_**

#### 4. Null và Undefined
`Null` và `Undefined` đều là những giá trị nằm ngoài các kiểu trên. `Null` được biểu diễn như là không có giá trị, trống rỗng. `Undefined` cũng như `Null`, nó biểu diễn như giá trị chưa được gán vào, nếu giá trị được định nghĩa nhưng chưa gán giá trị thì giá trị của nó sẽ là `undefined`

```Javascript
    let age = null;

    let name;
    console.log(name) //undefined
```

#### 5. Object
Một `object` có thể được định nghĩa bằng dấu `{}` với danh sách các cặp `key : value`, `key` là chuỗi và `value` có giá trị gì cũng được

```Javascript
    let obj = new Object(); // object constructor
    let obj2 = {}; // object literal
    
    let obj3 = {
        name : 'bla',
        age : 69,
        whatt : {
            hey : true,
        }
    }

    obj3.whatt.foo = 'bar' //thêm cặp key value vào obj3
```
Để gọi một giá trị trong object
```Javascript
    alert(obj.name); //bla
    alert(obj.whatt.hey); //true
```
Để xóa một cặp key value
```Javascript
    delete obj.whatt.hey;
```
Cách khai báo key khác
```Javascript
    const key = 'this is key';
    let obj = {
        name : 'bla',
        age : 12,
    }
    obj[key] = 'ehe';
```
**Giới hạn việc đặt tên biến**

Các biến không thể đặt các tên như `for`, `return`,.... Nhưng các đặc tính của object thì không có giới hạn đó.
```Javascript
    // these properties are all right
    let obj = {
        for: 1,
        let: 2,
        return: 3
    };

    alert( obj.for + obj.let + obj.return );  // 6
```
Ngay cả khi tên của thuộc tính không phải là một chuỗi mà là một số, lấy ví dụ `0` thì nó sẽ được tự động đổi sang `"0"`.
```Javascript
let obj = {
  0: "test" // same as "0": "test"
};

// both alerts access the same property (the number 0 is converted to string "0")
alert( obj["0"] ); // test
alert( obj[0] ); // test (same property)
```
**Các hàm thường dùng của object**

**_Kiểm tra key có tồn tại trong một object, lệnh `in`_**
```Javascript
let user = { name: "John", age: 30 };

alert( "age" in user ); // true, user.age exists
alert( "blabla" in user ); // false, user.blabla doesn't exist
```

**_Vòng lặp `for ... in ....`_**
```Javascript
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // keys
  alert( key );  // name, age, isAdmin
  // values for the keys
  alert( user[key] ); // John, 30, true
}
```

### Vòng lặp
#### Vòng lặp while
Vòng lặp `while` có cấu trúc sau:
```Javascript
while(condition){
    //code
}
```
Khi giá trị của `condition` là `truthy` thì code sẽ được thực thi.

#### Vòng lặp do...while
Vòng lặp `do...while` có cấu trúc sau:
```Javascript
do {
  // loop body
} while (condition);    
```
Vòng lặp này sẽ thực thi code trước rồi mới kiểm tra điều kiện, ngược lại với vòng lặp while.

#### Vòng lặp for
Mặc định vòng lặp for sẽ nhận 3 giá trị `begin, condition, step`
```Javascript
for (begin; condition; step) {
  // ... loop body ...
}
```

Đầu tiên nó sẽ khởi tạo giá trị cho vòng lặp. Sau đó kiểm tra điều kiện, nếu giá trị `truthy` nó sẽ thực thi code, tiếp theo dùng step để nhảy đến giá trị kế tiếp của vòng lặp, và cứ tiếp tục như vậy. Đây là sơ đồ
```
Run begin
→ (if condition → run body and run step)
→ (if condition → run body and run step)
→ (if condition → run body and run step)
→ ...
```
Vòng lặp for còn có thể bỏ qua một vài giá trị khởi tạo. Chẳng hạn:
```Javascript
let i = 0; // we have i already declared and assigned

for (; i < 3; i++) { // no need for "begin"
  alert( i ); // 0, 1, 2
}
// or
let i = 0;

for (; i < 3;) { //skip begin and step
  alert( i++ );
}
// or
for (;;) {
  // repeats without limits
}
```
#### Phá vỡ vòng lặp
Để phá vòng lặp chúng ta sử dụng `break`. Nó có thể dùng để phá vỡ vòng lặp hiện tại trong mọi hoàn cảnh nhưng thông thường ta sẽ cho một điều kiện để phá vòng lặp.
```Javascript
let sum = 0;

while (true) {

  let value = +prompt("Enter a number", '');

  if (!value) break; // (*)

  sum += value;

}
alert( 'Sum: ' + sum );
```
Lệnh break sẽ thực hiện ở dòng `(*)` nếu người dùng không nhập giá trị gì cho input. Nó sẽ dừng hoàn toàn vòng lặp và đi đến dòng đầu tiên sau vòng lặp, cụ thể ở đây là `alert()`
#### Đến bước lặp tiếp theo bằng lệnh `continue`
`continue` cũng tương tự như `break` nhưng nó không ngừng hoàn toàn vòng lặp, nó chỉ ngừng bước lặp hiện tại để nhảy trực tiếp sang bước lặp tiếp theo(nếu thỏa mãn `condition`).

Vòng lặp bên dưới dùng `continue` để in ra giá trị chẵn:
```Javascript
for (let i = 0; i < 10; i++) {

  // if true, skip the remaining part of the body
  if (i % 2 == 0) continue;

  alert(i); // 1, then 3, 5, 7, 9
}
```
`break` và `continue` không thể đứng bên phải của toán tử `?`, lấy ví dụ:
```Javascript
(i > 5) ? alert(i) : continue; // continue isn't allowed here
```
#### Nhãn dành cho break/continue
Giả sử bạn có 2 vòng lặp lồng vào nhau, sẽ thế nào nếu bạn muốn phá vòng lặp cha ở trong vòng lặp con? Lấy ví dụ:
```Javascript
for (let i = 0; i < 3; i++) {

  for (let j = 0; j < 3; j++) {

    let input = prompt(`Value at coords (${i},${j})`, '');

    // what if we want to exit from here to Done (below)?
  }
}

alert('Done!');
```

Lúc đó chúng ta sẽ dùng `lable`, nó sẽ thành thế này
```Javascript
outer: for (let i = 0; i < 3; i++) {

  for (let j = 0; j < 3; j++) {

    let input = prompt(`Value at coords (${i},${j})`, '');

    // if an empty string or canceled, then break out of both loops
    if (!input) break outer; // (*)

    // do something with the value...
  }
}

alert('Done!');
```
Ở đoạn code trên chúng ta thấy vòng for đầu tiên được gắn cho một nhãn `outer` và ở dòng (*) ta sẽ thấy lệnh `break outer`, nó sẽ ngừng lệnh lặp bên ngoài đồng nghĩa với việc vòng lặp sẽ dừng hẳn và nhảy đến lệnh `alert()`

#### Lệnh switch 
`switch` có thể thay thế nhiều lệnh `if` lồng nhau

Cú pháp:
```Javascript
switch(x) {
  case 'value1':  // if (x === 'value1')
    ...
    [break]

  case 'value2':  // if (x === 'value2')
    ...
    [break]

  default:
    ...
    [break]
}
```

Giá trị của `x` sẽ được kiểm tra chặt chẽ với giá trị của các `case`, nó sẽ kiểm tra theo thứ tự từ trên xuống.

Nếu giá trị giống nhau, nó sẽ thực thi code ở `case` đó cho đến lệnh `break` gần nhất hoặc cho đến kết thúc vòng lặp.

Nếu không có giá trị nào, code ở trong `default` sẽ được chạy (nếu có).
Lấy ví dụ :
```Javascript
let a = 2 + 2;

switch (a) {
  case 3:
    alert( 'Too small' );
    break;
  case 4:
    alert( 'Exactly!' );
    break;
  case 5:
    alert( 'Too big' );
    break;
  default:
    alert( "I don't know such values" );
}
```
Trong trường hợp này code ở `case 4` sẽ được chạy, in ra giá trị `Exactly!`, và khi gặp lệnh `break` nó sẽ kết thúc.

Ví dụ khi không có `break`:
```Javascript
let a = 2 + 2;

switch (a) {
  case 3:
    alert( 'Too small' );
  case 4:
    alert( 'Exactly!' );
  case 5:
    alert( 'Too big' );
  default:
    alert( "I don't know such values" );
}
```
Ở trường hợp này code sẽ chạy từ `case 4` cho đến `default` bởi vì không cho lệnh `break` nào, nó sẽ in ra
```
Too small
Exactly
Too big
I don't know such values
```

Có một vấn đề về việc so sánh giá trị giống nhau nhưng khác kiểu dữ liệu. Lệnh `switch` sẽ so sánh bằng toán tử `===` cho nên nếu giá trị nhập vào là `0` nhưng giá trị của `case` là ` "0" ` thì nó vẫn xem là khác nhau.
### If else
#### Lệnh if
Lệnh `if` sẽ kiểm tra điều kiện được cho và thực thi code nếu điều kiện là `truthy`

Lấy ví dụ:
```Javascript
let year = prompt('In which year was ECMAScript-2015 specification published?', '');

if (year == 2015) alert( 'You are right!' );
```
#### Truthy và Falsy
Lệnh if cũng có thể lấy giá trị khác `true` và `false`.

Nhắc lại về [truthy và falsy](#3-boolean)

Vì vậy 
```Javascript
if (0) { // 0 is falsy
  ...
}

if (1) { // 1 is truthy
  ...
}
```

#### Lệnh else
Một lệnh `if` có thể hoặc không một lệnh `else` phía sau, và `else` sẽ được thực thi khi giá trị của điều kiện là `falsy`
```Javascript
let year = prompt('In which year was the ECMAScript-2015 specification published?', '');

if (year == 2015) {
  alert( 'You guessed it right!' );
} else {
  alert( 'How can you be so wrong?' ); // any value except 2015
}
```
#### Toán tử '?'
Lệnh `if else` có thể được rút gọn thành 
```Javascript
if (age > 18) {
  accessAllowed = true;
} else {
  accessAllowed = false;
}
// sẽ trở thành
let accessAllowed = (age>18)?true:false
//hoặc rút gọn thành
let accessAllowed = (age>18)
```

Có thể có nhiều `?` trong một sequence nhưng hạn chế lạm dụng, nếu không code sẽ trở nên khó đọc.

### Function/Arrow Function
#### Function
Có 2 cách khai báo function : 
```Javascript
function sayHi() { //(*)
  alert( "Hello" );
}
// or
let sayHi = function() { //(**)
  alert( "Hello" );
};
```

Ở `(*)` là khai báo một function thông thường gọi là `function declaration`, ngoài ra còn có cách thứ 2 `(**)`, chúng ta có thể gán cho biến `sayHi` giá trị là một `function`, cách này được gọi là `function expression`.

Khi một function được gọi, lấy ví dụ:
```Javascript
function sayHi() {
  alert( "Hello" );
}

alert( sayHi ); // shows the function code
```
Ở đây kết quả hiện ra sẽ là đoạn code của function `sayHi` chứ không phải `Hello`. Để thực thi đoạn code trong function chúng ta sử dụng `sayHi()`

#### Callback function
Là một function có thể được gọi lại khi cần thiết

Ở đây chúng ta sẽ khai báo một hàm `ask` chứa 3 giá trị:

`question` : Một đoạn câu hỏi <br>
`yes` : hàm thực thi nếu câu trả lời là 'yes'   
`no` : hàm thực thi nếu câu trả lời là 'no'

```Javascript
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

function showOk() {
  alert( "You agreed." );
}

function showCancel() {
  alert( "You canceled the execution." );
}

// usage: functions showOk, showCancel are passed as arguments to ask
ask("Do you agree?", showOk, showCancel);
```
#### So sánh giữa function expression và function declaration
function expression | function declaration
---- | ----
được tạo ra khi đoạn code khởi tạo đã được đọc, và chỉ có thể gọi nó ở sau đó | Có thể được gọi trước khi được khai báo
Ở `strict mode` khi function được khởi tạo ở trong một code block thì bên ngoài code block đó không thể đọc được | Có thể khai báo một biến rỗng ở ngoài code block, khi code block đã được thực thi thì có thể gọi nó thoải mái.
#### Arrow function
Là một cách khai báo ngắn gọn hơn của function expression, nó trông như thế này:
```Javascript
let func = (arg1, arg2, ..., argN) => expression;
//or 
let func = (arg1, arg2, ..., argN) => {
    //code
};
// e.g 
let sum = (a, b) => a + b;
alert( sum(1, 2) ); // 3

let double = n => n * 2;
alert( double(3) ); // 6
```
##### Arrow function không có 'this'
Nếu chúng ta lấy `this` nó sẽ lấy các thuộc tính bên ngoài.   
Lấy ví dụ bên dưới, chúng ta có thể tham chiếu this bên ngoài function showList.
```Javascript
let group = {
  title: "Our Group",
  students: ["John", "Pete", "Alice"],

  showList() {
    this.students.forEach(
      student => alert(this.title + ': ' + student)
    );
  }
};

group.showList();
```
### Toán tử logic
Có 4 toán tử logic cơ bản `||`, `&&`, `!`, `??`. Những toán tử này có thể cho các kết quả ngoài `true` `false`.
#### || (OR)
Toán tử này sẽ tìm giá trị đúng đầu tiên
```Javascript
alert( true || true );   // true
alert( false || true );  // true
alert( null || 0 || 1 ); // 1 (the first truthy value)
```
#### && (AND)
Toán tử này tìm và lấy giá trị sai đầu tiên hoặc nếu cả 2 đều đúng thì sẽ lấy cái cuối cùng
```Javascript
alert( true && true );   // true
alert( false && true );  // false
alert( 1 && 5 ); // 5
alert( 1 && 2 && null && 3 ); // null
alert( 1 && 2 && 3 ); // 3, the last one
```
>Lưu ý: Toán tử `&&` có độ ưu tiên cao hơn toán tử `||` cho nên khi gặp một bài toán đều có `&& `và `||` thì `&&` sẽ được chạy trước.
#### ! (NOT)
`!` sẽ nghịch đảo giá trị logic của giá trị đứng sau nó, ngoài ra `!!` giúp đổi một giá trị thành giá trị logic `true` hoặc `false`. Lấy ví dụ:
```Javascript
alert( !true ); // false
alert( !0 ); // true
alert( !!"non-empty string" ); // true
alert( !!null ); // false
```
#### ?? (Kiểm tra tồn tại giá trị)
`??` xem `null` và `undefined` là như nhau. Một giá trị được khởi tạo khi nó không phải là giá trị `null` và `undefined`.
Kết quả của `a ?? b` là :   
- Nếu `a` được định nghĩa -> `a`
- Nếu `a` không được định nghĩa -> `b`

Nói một cách ngắn gọn, nó sẽ lấy giá trị đầu tiên nếu không phải `null/undefined`, ngược lại nó sẽ lấy giá trị thứ 2.   
Lấy ví dụ:
```Javascript
// undefined
let user;
alert(user ?? "Anonymous"); // Anonymous (user not defined)

// defined
let user = "John";
alert(user ?? "Anonymous"); // John (user defined)
```
### Xử Lý lỗi bằng try...catch
Khi chúng ta gặp lỗi đoạn code đang được thực thi sẽ ngưng hoàn toàn, thay vì điều đó chúng ta sử dụng `try...catch`, nó sẽ giúp chúng ta bắt được lỗi và xử lý nó trong khi code của chúng ta vẫn tiếp tục chạy.   
Cú pháp `try...catch`
```Javascript
try {
  // code...
} catch (err) {
  // error handling
}
```
Khi code trong block `try` không xảy ra lỗi, nó sẽ bỏ qua `catch` và tiếp tục chạy.   
Nếu như lỗi xảy ra code trong block `catch` sẽ được thực thi và bỏ qua tất cả code trong block `try`
```Javascript
try {

  alert('Start of try runs');  // (1) <--

  lalala; // error, variable is not defined!

  alert('End of try (never reached)');  // (2)

} catch (err) {

  alert(`Error has occurred!`); // (3) <--

}
```
`try...catch` làm việc một cách đồng bộ, nếu có một lỗi xảy ra không theo thứ tự, chẳng hạn `setTimeout` thì `try...catch` không thể nắm được lỗi. Để tránh việc đó chúng ta sẽ bọc các dòng code trong hàm setTimeout
```Javascript
try {
  setTimeout(function() {
    noSuchVariable; // script will die here
  }, 1000);
} catch (err) {
  alert( "won't work" );
}
// instead do this
setTimeout(function() {
  try {
    noSuchVariable; // try...catch handles the error!
  } catch {
    alert( "error is caught here!" );
  }
}, 1000);
```
#### Hàm throw
`throw` được dùng để chủ động đưa lỗi đến `catch`. 

Cú pháp là:
```Javascript
throw <error object>
```
Để tạo một lỗi chúng ta có cú pháp như sau :
```Javascript
let error = new Error(message);
// or
let error = new SyntaxError(message);
let error = new ReferenceError(message);
// ví dụ

let error = new Error("Things happen o_O");

alert(error.name); // Error
alert(error.message); // Things happen o_O
```
Áp dụng vào `try..catch`
```Javascript
let json = '{ "age": 30 }'; // incomplete data

try {

  let user = JSON.parse(json); // <-- no errors

  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name"); // (*)
  }

  alert( user.name );

} catch (err) {
  alert( "JSON Error: " + err.message ); // JSON Error: Incomplete data: no name
}
```
#### Rethrow Error
Khi bắt gặp một lỗi ngoài ý muốn chúng ta dùng rethrow. Nói cách khác, `catch` chỉ thực hiện xử lý nhưng lỗi nó biết và 'rethrow' những cái còn lại.   
Cách xử lý như sau:
```Javascript
let json = '{ "age": 30 }'; // incomplete data
try {

  let user = JSON.parse(json);

  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name");
  }

  blabla(); // unexpected error

  alert( user.name );

} catch (err) {

  if (err instanceof SyntaxError) {
    alert( "JSON Error: " + err.message );
  } else {
    throw err; // rethrow (*)
  }

}
```
#### try...catch...finally
`try...catch` còn một thứ nữa đó là `finally`. Nếu có `finally`, nó sẽ luôn chạy trong mọi trường hợp. 
```Javascript
try {
   ... try to execute the code ...
} catch (err) {
   ... handle errors ...
} finally {
   ... execute always ...
}
```
`finally` thường dùng để hoàn thành những công việc ở trong `try`
### Object
Object được sử dụng để thể hiện một thực thể ở thế giới thực, chẳng hạn động vật, con người, cây,...

Ở đây chúng ta sẽ khai báo một người có thuộc tính `name` và `age`
```Javascript
let user = {
  name: "John",
  age: 30
};
```
Con người cũng có khả năng **vận động, hành động**, chúng ta sẽ thêm hành động cho nó
```Javascript
let user = {
  name: "John",
  age: 30
};

user.sayHi = function() {
  alert("Hello!");
};

user.sayHi(); // Hello!
```
Ở đây chúng ta dùng `function expression` để tạo thuộc tính `sayHi` cho con người của chúng ta.

#### 'this' trong object
Để có thể truy cập được các thuộc tính của object chúng ta dùng `this`.   
```Javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // "this" is the "current object"
    alert(this.name);
  }

};

user.sayHi(); // John
```

#### 'this' không bị ràng buộc
Trong js, chúng ta có thể gọi `this` ở bất cứ đâu, và giá trị của nó sẽ phụ thuộc vào ngữ nghĩa.   
Lấy một ví dụ, chúng ta có 2 object khác nhau và có 2 `this` riêng biệt.
```Javascript
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```
Nếu chỉ gọi mà không tham chiếu đến object nào thì giá trị của `this` sẽ là `undefined`.
```Javascript
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```
#### Arrow function không có 'this'
Nếu chúng ta cố lấy `this` trong arrow function, `this` sẽ được tham chiếu ra ngoài function khác.   
Ví dụ:
```Javascript
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```
#### Chain call
Một chain call là một object có thể call nhiều function cùng một lúc, lấy ví dụ sau:
```Javascript
ladder.up().up().down().showStep().down().showStep(); // shows 1 then 0
```
Để có thể làm được như thế này chúng ta chỉ cần trả về chính object của nó trong mỗi function.
```Javascript
let ladder = {
  step: 0,
  up() {
    this.step++;
    return this;
  },
  down() {
    this.step--;
    return this;
  },
  showStep() {
    alert( this.step );
    return this;
  }
};

ladder.up().up().down().showStep().down().showStep(); // shows 1 then 0
```
#### Clone một object
Cho ví dụ như sau:
```Javascript
let user = { name: "John" };

let admin = user; // copy the reference
```
Khi chúng ta làm thao tác này, nó chỉ copy reference của object user chứ không thực sự clone được cả object, điều này sẽ dẫn đến việc nếu chúng ta thay đổi thuộc tính của object admin thì object user cũng sẽ bị thay đổi theo. 
```Javascript
let user = { name: 'John' };

let admin = user;

admin.name = 'Pete'; // changed by the "admin" reference

alert(user.name); // 'Pete', changes are seen from the "user" reference
```
Để tránh tình trạng này js đã cung cấp cho chúng ta một vài cách để làm việc như sau:
##### Object.assign
```Javascript
Object.assign(dest, [src1, src2, src3...])
```
Trong này dest là object chính, [src1,src2,...] là object cần clone lại. Ví dụ:
```Javascript
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

Object.assign(user, permissions1, permissions2);
// now user = { name: "John", canView: true, canEdit: true }

// hoặc để clone cả một Object
let user = {
  name: "John",
  age: 30
};

let clone = Object.assign({}, user);
```
#### Toán tử 'new'
Chúng ta dùng `new` để tạo một object tương tự cái được tham chiếu.
```Javascript
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("Jack");

alert(user.name); // Jack
alert(user.isAdmin); // false
```
Khi một function được chạy cùng với `new`, nó sẽ làm theo thứ tự sau:
- Một object mới sẽ được tạo và gán cho `this`
- Bên trong function sẽ được chạy, thông thường là gán thuộc tính cho nó bằng `this`
- Một function mới sẽ được tạo ra bằng cách trả về `this`
```Javascript
function User(name) {
  // this = {};  (implicitly)

  // add properties to this
  this.name = name;
  this.isAdmin = false;

  // return this;  (implicitly)
}
```
#### Optional chaining, vấn đề cho việc gọi một thuộc tính không tồn tại trong object
Đây là một vấn đề khá phổ biến trong js, lấy ví dụ:
```Javascript
let user = {}; // a user without "address" property

alert(user.address.street); // Error!
```
Khi chúng ta cố gọi thuộc tính của `undefined`, nó sẽ gây ra lỗi.   
Để tránh tình trạng này chúng ta sẽ sử dụng `?`, nó sẽ giúp chúng ta trả về `undefined` hoặc `null` nếu thuộc tính đứng trước nó không tồn tại. Rất hữu dụng trong việc gọi API.
```Javascript
let user = {}; // user has no address

alert( user?.address?.street ); // undefined (no error)
```
>Tránh lạm dụng `?.` để không gặp khó khăn khi debug.   
### Asynchronous
#### Callback
Callback được dùng khi cần thực thi một lệnh nào đó sau một việc cụ thể, ví dụ chạy script sau khi nó đã load, bởi vì việc loadscript tốn một thời gian cụ thể, nên js sẽ thực thi các đoạn code phía sau song song với việc loadscript cho nên nó sẽ gây ra lỗi nếu chúng ta cố gọi một function trong script đó.
```Javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  document.head.append(script);
}

loadScript('https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js', script => {
  alert(`Cool, the script ${script.src} is loaded`);
});
```
Callback có thể lồng vào nhau bao nhiêu tùy ý, nhưng đều đó sẽ gây ra một số vấn đề cơ bản, đó là việc đọc code sẽ trở nên khó khăn hơn.
#### Xử lý lỗi trong callback
Chúng ta có thể tự xử lý lỗi trong callback bằng cách bắt sự kiện lỗi.
```Javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));

  document.head.append(script);
}

loadScript('/my/script.js', function(error, script) {
  if (error) {
    // handle error
  } else {
    // script loaded successfully
  }
});
```
#### Promise
Cú pháp khởi tạo của một promise như sau:
```Javascript
let promise = new Promise(function(resolve, reject) {
  // executor (the producing code, "singer")
});
```
Khi một function đưa vào promise, nó sẽ được thực thi và có 2 thuộc tính `resolve` và `reject`. Khi function trả về kết quả, nó sẽ truyền vào một trong hai thuộc tính sau:
- `resolve(value)` --- Khi công việc trong function đã được hoàn thành và trả về kết quả `value`.
- `reject(error)` ---- Khi có lỗi xảy ra và trả về `error`.

Promise có 3 trạng thái `pending` là khi function đang được khởi tạo và đang chờ để xử lý, `value` của trạng thái này sẽ là `undefined`. Nếu thực thi thành công trạng thái sẽ trở thành `fulfilled` và `value` ở trạng thái này là `resolve(value)`, ngược lại nếu việc thực thi xảy ra lỗi, trạng thái sẽ thành `rejected` và giá trị của nó là `reject(err)`. Ví dụ sau sẽ mô tả rõ:
```Javascript
// state fulfilled
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done"), 1000);
});

// state rejected
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});
```
#### Sử dụng 'then, catch'
Sau khi `promise` đã trả về kết quả hay lỗi, chúng ta sẽ sử dụng chúng bằng phương thức `.then()` và `.catch()`
##### Then 
Là phương thức khi `promise` đã trả về kết quả dù thành công hay thất bại. Cú pháp là:
```Javascript
promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
);
```
Hoặc khi chúng ta chỉ muốn bắt sự kiện thành công, chỉ cần lấy function đầu tiên.
```Javascript
let promise = new Promise(resolve => {
  setTimeout(() => resolve("done!"), 1000);
});

promise.then(alert); // shows "done!" after 1 second
```
##### Catch
Là phương thức khi chúng ta chỉ cần bắt sự kiện lỗi.
```Javascript
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// .catch(f) is the same as promise.then(null, f)
promise.catch(alert); // shows "Error: Whoops!" after 1 second
```
##### Finally
Cũng giống như `try...catch`, promise cũng có `finally`, nó có chức năng tương tự là dùng để chạy sau khi promise đã xử lý dù thành công hay thất bại.

##### Chaining promise
Chúng ta có thể gọi nhiều `.then()` cùng một lúc khi cần xử lý nhiều công việc riêng lẽ.
```Javascript
loadScript("/article/promise-chaining/one.js")
  .then(script => loadScript("/article/promise-chaining/two.js"))
  .then(script => loadScript("/article/promise-chaining/three.js"))
  .then(script => {
    // scripts are loaded, we can use functions declared there
    one();
    two();
    three();
  });
```
Lấy ví dụ khi fetch API
```Javascript
fetch('/article/promise-chaining/user.json')
  // Load it as json
  .then(response => response.json())
  // Make a request to GitHub
  .then(user => fetch(`https://api.github.com/users/${user.name}`))
  // Load the response as json
  .then(response => response.json())
  .then(githubUser => {
    let img = document.createElement('img');
    img.src = githubUser.avatar_url;
    img.className = "promise-avatar-example";
    document.body.append(img);

    setTimeout(() => img.remove(), 3000); // (*)
  });
```
#### Promise API 
##### Promise.all
Khi chúng ta có một chuỗi công việc để làm song song với nhau, chẳng hạn fetch nhiều user cùng một lúc, chúng ta sẽ sử dụng `promise.all`.
```Javascript
Promise.all([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
]).then(alert); // 1,2,3 when promises are ready: each promise contributes an array member
```
Nó sẽ đợi tất cả các `promise` trong array hoàn thành xong công việc thì mới thực thi `.then()`
##### Lỗi trong promise.all
Khi một promise trong array bị lỗi, `promise.all` sẽ dừng ngay lập tức và trả về `.catch()`
```Javascript
Promise.all([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Whoops!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).catch(alert); // Error: Whoops!
```
Việc này có thể để lại một chút phiền phức, giả sử chúng ta muốn fetch các user nhưng một user bị lỗi sẽ khiến cả chuỗi trả về lỗi, để tránh tình trạng này chúng ta sẽ sử dụng `Promise.allSettled`.
##### Promise.allSettled
```Javascript
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/remy',
  'https://no-such-url'
];

Promise.allSettled(urls.map(url => fetch(url)))
  .then(results => { // (*)
    results.forEach((result, num) => {
      if (result.status == "fulfilled") {
        alert(`${urls[num]}: ${result.value.status}`);
      }
      if (result.status == "rejected") {
        alert(`${urls[num]}: ${result.reason}`);
      }
    });
  });
```
Kết quả trả về sẽ là 
```Javascript
[
  {status: 'fulfilled', value: ...response...},
  {status: 'fulfilled', value: ...response...},
  {status: 'rejected', reason: ...error object...}
]
```
#### Async/await
##### Async
Cú pháp cơ bản:
```Javascript
async function f() {
  return 1;
}
```
Khi `async` được đặt phía trước một function, function sẽ luôn trả về một Promise.   
Lấy một ví dụ:
```Javascript
async function f() {
  return 1;
}

f().then(alert); // 1
```
##### Await
Cú pháp:
```Javascript
// works only inside async functions
let value = await promise;
```
`await` sẽ làm cho js đợi đến khi `promise` trả về kết quả.
```Javascript
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });

  let result = await promise; // wait until the promise resolves (*)

  alert(result); // "done!"
}

f();
```
function phía trên sẽ đợi ở dòng (*) cho đến khi promise đã hoàn thành, và `result` sẽ trở thành kết quả.   
>Lưu ý: await chỉ có thể được gọi bên trong một hàm async 
### Export và Import
#### Export trước khi khởi tạo
```Javascript
// export an array
export let months = ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

// export a constant
export const MODULES_BECAME_STANDARD_YEAR = 2015;

// export a class
export class User {
  constructor(name) {
    this.name = name;
  }
}
```
#### Export tách riêng khởi tạo
```Javascript
// 📁 say.js
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

function sayBye(user) {
  alert(`Bye, ${user}!`);
}

export {sayHi, sayBye}; // a list of exported variables
```
#### Export default
```Javascript
// 📁 user.js
export default class User { // just add "default"
  constructor(name) {
    this.name = name;
  }
}
```
#### Import *
Dùng để import tất cả trong một module 
```Javascript
// 📁 main.js
import * as say from './say.js';

say.sayHi('John');
say.sayBye('John');
```
#### Import {}
Dùng để import một vài module riêng lẻ
```Javascript
// 📁 main.js
import {sayHi, sayBye} from './say.js';

sayHi('John'); // Hello, John!
sayBye('John'); // Bye, John!
```
#### Các trường hợp thông dụng
import default cùng với một hàm nhất định
```Javascript
// 📁 main.js
import {default as User, sayHi} from './user.js';

new User('John');
```
Khi import một default module, chúng ta có thể đặt tên tùy ý
```Javascript
import User from './user.js'; // works
import MyUser from './user.js'; // works too
// could be import Anything... and it'll still work
```
### Class
Class có các đặc trưng của một object và là một phiên bản mở rộng của nó. Cú pháp của class là:
```Javascript
class MyClass {
  // class methods
  constructor() { ... }
  method1() { ... }
  method2() { ... }
  method3() { ... }
  ...
}
```
Cách khởi tạo một chủ thể của class
```Javascript
class User {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    alert(this.name);
  }
}

// Usage:
let user = new User("John");
user.sayHi();
```
#### Class expression
Tương tự như object, chúng ta cũng có thể dùng expression cho class. Nó cũng có các đặc tính cơ bản của một function expression.
```Javascript
let User = class {
  sayHi() {
    alert("Hello");
  }
};
```
#### Getter và Setter
Cũng như object, class cũng có getter/setter.
```Javascript
class User {

  constructor(name) {
    // invokes the setter
    this.name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (value.length < 4) {
      alert("Name is too short.");
      return;
    }
    this._name = value;
  }

}

let user = new User("John");
alert(user.name); // John

user = new User(""); // Name is too short.
```
#### Class inheritance
Class inheritance là một cách để thừa hưởng các thuộc tính của một class khác.
##### Từ khóa 'extends'
Dùng để thừa hưởng các thuộc tính và phương thức trong một class, giả sử chúng ta có một class là `Animal`, chúng ta muốn một class `Rabbit` thừa hưởng các thuộc tính cơ bản của `Animal` và thêm các thuộc tính riêng chẳng hạn `hide`, chúng ta sẽ làm như sau:
```JavaScript
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }
  run(speed) {
    this.speed = speed;
    alert(`${this.name} runs with speed ${this.speed}.`);
  }
  stop() {
    this.speed = 0;
    alert(`${this.name} stands still.`);
  }
}

class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }
}

let rabbit = new Rabbit("White Rabbit");

rabbit.run(5); // White Rabbit runs with speed 5.
rabbit.hide(); // White Rabbit hides!
```
##### Ghi đè một method (Overriding)
Thông thường, tất cả phương thức không được tạo ra ở `class Rabbit` sẽ được lấy trực tiếp từ `class Animal`. Nhưng nếu chúng ta viết lại một phương thức riêng cho `class Rabbit`, ví dụ như `stop()` thì nó sẽ trực tiếp lấy phương thức đó ở `class Rabbit`. Nhưng chúng ta không thực sự muốn viết lại nó, mà đơn giản chỉ muốn bổ sung một vài hành động, thì sẽ dùng từ khóa `super()`.
```JavaScript
class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }

  stop() {
    super.stop(); // call parent stop
    this.hide(); // and then hide
  }
}
```
#### Static property/method
`static` giúp `this` gọi đến chính class khởi tạo, tương tự function expression.
```Javascript
class User {
  static staticMethod() {
    alert(this === User);
  }
}

User.staticMethod(); // true
```
> Static method chỉ được gọi ở trong một class chứ không phải một thực thể của class đó.   

Thông thường, static method được dùng để thực hiện hàm dành cho class chung, chứ không phải một vài object cụ thể.
```Javascript
class Article {
  constructor(title, date) {
    this.title = title;
    this.date = date;
  }

  static compare(articleA, articleB) {
    return articleA.date - articleB.date;
  }
}

// usage
let articles = [
  new Article("HTML", new Date(2019, 1, 1)),
  new Article("CSS", new Date(2019, 0, 1)),
  new Article("JavaScript", new Date(2019, 11, 1))
];

articles.sort(Article.compare);

alert( articles[0].title ); // CSS
```
Hàm `compare` ở đây không dành riêng cho một `article` cụ thể nào cả, mà là cả một class Article.

Chúng ta cũng có thể dùng `static` để 'factory' một class
```JavaScript
class Article {
  constructor(title, date) {
    this.title = title;
    this.date = date;
  }

  static createTodays() {
    // remember, this = Article
    return new this("Today's digest", new Date());
  }
}

let article = Article.createTodays();

alert( article.title ); // Today's digest
```
Ở đây hàm `Article.createTodays()` sẽ tạo ra một object mới với thuộc tính `date` riêng biệt.   
Static method thường được dùng trong các công việc liên quan đến database dùng để tìm/sửa/xóa một thực thể trong database, ví dụ:
```JavaScript
// assuming Article is a special class for managing articles
// static method to remove the article by id:
Article.remove({id: 12345});
```
##### Protected
Các thuộc tính hay phương thức có từ khóa `protected` sẽ chỉ được gọi từ bên trong class và các class thừa kế. Thông thường để đặt một thuộc tính `protected` chúng ta sẽ thêm `_` đằng trước tên thuộc tính đó, chẳng hạn:
```JavaScript
class CoffeeMachine {
  _waterAmount = 0;

  set waterAmount(value) {
    if (value < 0) {
      value = 0;
    }
    this._waterAmount = value;
  }

  get waterAmount() {
    return this._waterAmount;
  }

  constructor(power) {
    this._power = power;
  }

}

// create the coffee machine
let coffeeMachine = new CoffeeMachine(100);

// add water
coffeeMachine.waterAmount = -10; // _waterAmount will become 0, not -10
```
Để cài một giá trị mặc định cho một thuộc tính, ta có thể xóa setter
```JavaScript
class CoffeeMachine {
  // ...

  constructor(power) {
    this._power = power;
  }

  get power() {
    return this._power;
  }

}

// create the coffee machine
let coffeeMachine = new CoffeeMachine(100);

alert(`Power is: ${coffeeMachine.power}W`); // Power is: 100W

coffeeMachine.power = 25; // Error (no setter)
```
##### Private
Tương tự `protected` nhưng thuộc tính `private` chỉ có thể truy cập được từ class khởi tạo, các class thừa hưởng sẽ không có các thuộc tính này. Các thuộc tính `private` thường bắt đầu bằng `#`.
```JavaScript
class CoffeeMachine {
  #waterLimit = 200;

  #fixWaterAmount(value) {
    if (value < 0) return 0;
    if (value > this.#waterLimit) return this.#waterLimit;
  }

  setWaterAmount(value) {
    this.#waterLimit = this.#fixWaterAmount(value);
  }

}

let coffeeMachine = new CoffeeMachine();

// can't access privates from outside of the class
coffeeMachine.#fixWaterAmount(123); // Error
coffeeMachine.#waterLimit = 1000; // Error
```
### Date Time
#### Khởi tạo
Có thể tạo `Date` bằng các phương thức sau:   

`new Date()`--- Nếu không có tham số truyền vào, nó sẽ tự động lấy thời gian hiện tại.
```JavaScript
let now = new Date();
alert( now ); // shows current date/time
```
`new Date(millisecond)`--- Tạo một object `Date` với thời gian bằng số millisecond được tính từ 1/1/1970 ở mốc UTC +0

`new Date(dateString)`--- Nếu có một tham số và đó là string thì nó sẽ được tự động phân tích chuyển về đúng cú pháp của `Date`
```JavaScript
let date = new Date("2017-01-26");
alert(date);
// The time is not set, so it's assumed to be midnight GMT and
// is adjusted according to the timezone the code is run in
// So the result could be
// Thu Jan 26 2017 11:00:00 GMT+1100 (Australian Eastern Daylight Time)
// or
// Wed Jan 25 2017 16:00:00 GMT-0800 (Pacific Standard Time)
```
`new Date(year, month, date, hours, minutes, seconds, ms)`    
`year`--- Nên là số có 4 chữ số   
`month`--- Được đánh số từ 0(Thg 1) đến 11(Thg 12)   
`date`--- Ngày trong tháng   
`hours, minutes, seconds, ms`--- Tự động trở thành 0 nếu không có tham số cho nó.
```JavaScript 
new Date(2011, 0, 1, 0, 0, 0, 0); // 1 Jan 2011, 00:00:00
new Date(2011, 0, 1); // the same, hours etc are 0 by default
```
#### Phương thức lấy một thành phần cụ thể trong Date
`getFullYear()` --- Lấy 4 chữ số   
`getMonth()` --- Lấy tháng từ 0 - 11   
`getDate()` --- Lấy ngày trong tháng từ 1 - 31   
`getHours(), getMinutes(), getSeconds(), getMiliseconds()`   
`getday()` --- Lấy ngày trong tuần, từ 0(C.N) - 6(T.7)
#### Date.now()
Nếu chúng ta muốn đo thời gian, có thể sử dụng `Date.now()` để trả về thời gian hiện tại. Có thể dùng phương thức này để đo lường hiệu suất.
```JavaScript
let start = Date.now(); // milliseconds count from 1 Jan 1970

// do the job
for (let i = 0; i < 100000; i++) {
  let doSomething = i * i * i;
}

let end = Date.now(); // done

alert( `The loop took ${end - start} ms` ); // subtract numbers, not dates
```
#### Parse từ một chuỗi
Có thể dùng `Date.parse(str)` để chuyển định dạng từ chuỗi sang `Date`.   
Định dạng sẽ là `YYYY-MM-DDTHH:mm:ss.sssZ`, trong đó:
- `YYYY-MM-DD` --- là năm-tháng-ngày 
- `T` --- được dùng như dấu phân cách
- `HH:mm:ss.sss` --- là giờ: phút: giây: mili giây
- `Z` --- Có thể có hoặc không, dùng để xác định múi giờ.

Có thể dùng các format ngắn gọn hơn, chẳng hạn `YYYY-MM-DD` hoặc `YYYY-MM` hoặc `YYYY`