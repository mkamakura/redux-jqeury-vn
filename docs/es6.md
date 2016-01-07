# ES6 Frist Step

## Mở đầu
`JavaScript` được implement base theo spec có tên là `ECMAScript`. `ECMAScript6(ES6)` là spec tiêu chuẩn mới của `ECMAScript`. Spec này được quyết định vào tháng 6 năm 2015.
Do là 6 năm rồi mới thực hiện measure update từ  `ES5` nên có thể lúc đầu sẽ thấy là lạ.
Tuy nhiên, vì là update để cho tiện lợi hơn nên hãy lĩnh hội thật chắc chắn và tích cực sử dụng.

### ES6 or ES2015?
Trên Net, có thể thấy các từ `ES6` và `ES2015`, nhưng cả 2 từ này đều chỉ 1 thứ.
Viết đúng phải là `ES2015`. Từ năm sau cũng dự kiến sẽ update lên `ES2016`,`ES2017`….
Lúc đầu, do khi bắt đầuthảo luận spec đã sử dụng từ `ES6` nên tên này vẫn còn sót lại. Trong tài liệu này sẽ thống nhất sử dụng từ `ES6`. (Vì ngắn hơn sẽ dễ đọc hơn.)

### Babel

**Bối cảnh**

Việc quyết định spec của `ES6` được thực hiện vào tháng 6 năm 2015, nhưng không phải là có thể sử dụng được ngay spec mới.
Lý do: về việc implement JavaScript engine(V8,JavaScriptCore,Chakra v.v.), hiện tại khi thực hiện vẫn có chức năng không thể sử dụng được.
[Ở đây] Trên(https://kangax.github.io/compat-table/es6/) site, các chức năng đã được implement vào các browser thì có thể quyết định được.

Tương quan giữa Browser và JavaScript engine là như trong bảng sau:

|Browser|JavaScript engin|
|-|-|
|IE|Chakra|
|Chrome|V8|
|Opera|V8|
|Safari|JavaScriptCore|
|FireFox|SpiderMonkey|

**Babel**

Vẫn chưa đến việc áp dụng spec mới ở mức spec mới được thêm vào browser. Cái đã xuất hiện ở trên đó là `Babel`, và `Babel` là tool thực hiện transpile code của `ES6` thành code của `ES5`. ([Site công khai](https://babeljs.io/)). Về `ES2016`,`ES2017` dự kiến update từ năm sau, cũng đang được implement trước nên được biết là sắp tới cũng sẽ tiếp tục được sử dụng.
Ngay cả khi phát triển trên thực tế, cũng sử dụng `Babel` để transpile, browser sẽ đọc JavaScript code sau transpile.

## ES6 new spec
Ở phần này, tôi giải thích spec đặc biệt hay được sử dụng.

### let, const
`let`, `const` thì có thể khai báo biến của block scope. `const` thì khai báo giá trị không thể thay thế lại. Hãy không sử dụng biến `var` cho đến nay vẫn hay dùng. Về cơ bản, sử dụng `const`, chỉ trường hợp bắt buộc phải thay thế lại thì mới sử dụng `let`.
```js
// ES5
var a = 1;

// ES6
let b = 1; // Giá trị không thể thay thế lại （Chỉ sử dụng khi cần phải thay thế lại）
const c = 1; // Giá trị không thể thay thế lại （recommend）
```

### Templete Strings
Có thể viết một cách đơn giản kết hợp chuỗi ký tự. `${}` của chuỗi ký tự được bao bởi backquote sẽ được triên khai. Và có thể xuống dòng ở bên trong backquote.
```js
// ES5
var errorCode = 404;
var errorMessage = `file not found.`;
console.log('Error!! Code: ' + errorCode + ', Message: ' + errorMessage);
// Error!! Code: 404, Message: file not found.

// ES6
const errorCode = 404;
const errorMessage = `file not found.`;
console.log(`Error!! Code: ${errorCode}, Message: ${errorMessage}`);
// Error!! Code: 404, Message: file not found.
```

### Class
Có thể khai báo class.

```js
class User {
  constructor(name) {
    this.name = name;
  }

  say() {
    return `My name is ${this.name}`;
  }
}

// Kế thừa User class để tạo Admin class
class Admin extends User {
  say() {
    return `[Administrator] ${super.say()}`;
  }
}

const user = new User('Alice');
console.log(user.say()); // My name is Alice

const admin = new Admin('Bob');
console.log(admin.say()); // [Administrator] My name is Bob
```

### Default Parameters
Có thể thiết lập giá trị mặc định cho tham số của hàm số.
```js
function consoleName(name = 'Taro') {
  console.log(`username: ${name}`);
}

consoleName(); // Taro
consoleName('Masaya'); // Masaya
```

### Arrow Function
Có thể convert function thành `=>`.

```js
// ES5
$('.hoge').on('change', function(event) {
  console.log(event);
});

// ES6
$('.hoge').on('change', (event) => {
  console.log(event);
});
// Về nội dung, nếu là công thức thì có thể giản lược `{}`, nhưng kết quả của hàm số sẽ được `return`.
$('.hoge').on('change', (event) => console.log(event));
```

**※Lưu ý**

Việc sử dụng `this` giữa `function` và `=>` thì khác nhau.
```js
class StatusCode {
  constructor(statusCode) {
    this.statusCode = statusCode;
    
    setTimeout(() => {
      console.log(this.statusCode, 'OK');
    }, 1000);
    
    setTimeout(() => console.log(this.statusCode, 'OK'), 1000);
  
    self = this;
    setTimeout(function() {
      if (this.statusCode === undefined) console.log('`this.statusCode` is `undefined` in function');
      console.log(self.statusCode, 'OK');
    }, 1000);
  }
}

const statusCode = new StatusCode(200);
```
`this` ở bên trong `Allow Function` thì refer `this` ở `StatusCode` class, còn `function()` thì đang refer đến `this` của global object. Do `statusCode` đang định nghĩa ở `this` của global object nên sẽ bị `undefined`.


### Enhanced Object Literals
Object literal đã được extend.
```js
// ES5
var obj = { "name": "Masaya Kamakura" };

// ES6
// Nếu key name và tên biến của value là như nhau thì có thể giản lược
function getNameObject(name) {
  return { name };
}
console.log(getNameObject('Masaya Kamakura')); // {"name":"Masaya Kamakura"}

// Có thể khai báo động key mane.
function getNameObject(name) {
  const nameKey = 'fullName';
  return { [nameKey]: name }; // Sử dụng biến số bàng key.
}
console.log(getNameObject('Masaya Kamakura')); // {"fullName":"Masaya Kamakura"}

function getNameObject(name) {
  return { [(() => 'fullName')()]: name }; // Sử dụng hàm số cho key
}
console.log(getNameObject('Masaya Kamakura')); // {"fullName":"Masaya Kamakura"}
```

### Spread
Triển khai array và trả về.
Rất hay được sử dụng trong việc thêm element vào array và tạo array mới.
```js
const list = [1, 2, 3];
console.log(...list); // 1 2 3

// Thêm element vào phần đầu
const newList = [0, ...list];
console.log(newList); // [0, 1, 2, 3]
```

### Import/Export
- `import`: Đọc external module
- `export`: Làm cho có thể sử dụng ở external module

```js
// a.js
import * as fuga from "b"; // Đọc tất cả các module đã được export
import { hoge1, hoge2 } from "b"; // Đọc hoge1, hoge2
import foo from "b"; // Nếu không gán `{}` thì sẽ đọc module đã được export default
```

```js
// b.js
export default function foo() {};
export function hoge1() {};
export function hoge2() {};
```

`{}` cần trong trường hợp khi `export` thì không có `default`. Vì nó sẽ đọc tất cả module `* as fuge` nên sẽ rất tiện lợi,
nhưng hãy không sử dụng (lý do: sẽ làm tăng file size sau khi transpile）.

### Object.assign(target, ...sources)
Copy tất cả proterty mà từ 1 `source` object trở lên sở hữu vào `target`. Return value sẽ trở thành `target` object.
```js
const todo = {
  id: 1,
  text: 'catch up es6',
  status: 'pending'
};

const newTodo = Object.assign({}, todo, {status: 'progress'});
console.log(newTodo);
// { "id": 1, "text": "catch up es6", "status": "progress" }
```

## Bổ sung: Là chức năng mong muốn sẽ được dùng tích cực trên ES5

### Array.prototype.forEach()
Với mỗi element của array chạy 1 lần hàm số được gán.
```js
const data = [1, 2, 3, 4, 5];
data.forEach((val) => console.log(val));
```

Nếu là câu for thì cần biến loop, và nếu như vào sâu thêm các layer ở dưới thì tính dễ đọc càng giảm nên hãy tránh sử dụng.

### Array.prototype.map()
Đối với tất cả các element của array, gọi hàm số được gán, sau đó generate array mới từ kết quả đó.
Giống giống `Array.prototype.forEach()` nhưng khác nhau ở chỗ generate ra array mới.
```js
const data = [1, 2, 3, 4, 5];
const square = data.map((val) => val * val);
console.log(square); // [1, 4, 9, 16, 25]
```

### Array.prototype.filter()
Đối với các array element, chạy hàm test được gán làm tham số, sau đó generate ra array mới từ tất cả các array element thỏa mãn việc đó.
```js
const data = [1, 2, 3, 4, 5];
// Chỉ trả về kết quả đã bình phương chỉ trong trường hợp element nhỏ hơn 4
const filterSquare = data.filter((val) => val < 4)
                         .map((val) => val * val);
console.log(filterSquare); // [1, 4, 9]
```

### Array.prototype.reduce()
Đối với 2 array element kề nhau (từ trái sang phải) thì apply đồng thời hàm số và chọn giá trị đơn lẻ.
```js
const data = [1, 2, 3, 4, 5];

const sum = data.reduce((pre, current) => pre + current);
console.log(sum); // 15

const max = data.reduce((pre, current) => Math.max(pre, current));
console.log(max); // 5
```

**Ví dụ về sum**

||pre|current|return|
|-|-|-|
|1st|1|2|3|
|2nd|3|3|6|
|3rd|6|4|10|
|4th|10|5|15|
Kết quả: 15

**Ví dụ về max**

||pre|current|return|
|-|-|-|
|1st|1|2|2|
|2nd|2|3|3|
|3rd|3|4|4|
|4th|4|5|5|
Kết quả: 5

## Kết thúc
Chức năng được giới thiệu ở tài liệu này chỉ là 1 phần trong spec của ES6. Với các bạn muốn học kỹ hơn nữa, tôi sẽ cung cấp tài liệu tham khảo.
Ngoài ra, tôi cũng đã giới thiệu các hàm số tiện dùng trên ES5. Do có nhiều lúc có thể viết được mà không cần sử dụng `for`,`if`,`switch` v.v. nên hãy suy nghĩ xem có thể viết bằng chức năng của ES5 hay không.

## Tài liệu tham khảo
- https://babeljs.io/
- https://babeljs.io/docs/learn-es2015/
- http://sssslide.com/www.slideshare.net/teppeis/es6-in-practice
- https://kangax.github.io/compat-table/es6/
- https://developer.mozilla.org/ja/docs/Web/JavaScript
- http://azu.github.io/promises-book/