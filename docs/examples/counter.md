# Counter

## Mở đầu
Trong ví dụ lần này, chúng tôi tập trung giải thích sao cho hiểu được data flow.
Thực tế, các bạn vừa xem sample code vừa đọc giải thích thì sẽ hiểu sâu hơn.

Trình tự đọc code sau khi phát sinh event là như sau. Hãy nhớ sẵn trình tự này trong đầu và bắt đầu nghiên cứu nhé.

**`store.dispatch()`->`Action`->`Reducer`->`Componet:render()`**

## Component

### BaseComponent
- Tất cả `Component` được tạo với tiền đề kế thừa `BaseComponent`

[import](../../examples/utils/BaseComponent.js)

`constructor`
```js
constructor(selector, store, ...stateNames)
```
- `selector`: ID hoặc class name của DOM của `Component`
- `store`: `rootReducer`
- `...stateNames`: `state` giám sát （chuỗi ký tự）

- Cung cấp chức năng chạy hàm `render()` đã implement ở class con nếu có thay đổi ở `state` đã chỉ định bởi `...stateNames`

### constructor()
**Vai trò**
- Set của event handler
- Trường hợp có selector sử dụng bởi `render()` thì định nghĩa sẵn

```js
this.$result = this.$selector.find('.js-result');
this.$selector.find('.js-increment').on('click', () => this.dispatch(actions.increment()));
this.$selector.find('.js-decrement').on('click', () => this.dispatch(actions.decrement()));
```

`super(selector, store, 'result');` thì đang được định nghĩa sao cho nếu có thay đổi ở `result` của `state` thì sẽ chạy hàm `render()`.
Ở set của event handler, đang implement sao cho nếu phát sinh event `click` thì sẽ chạy hàm `store.dispatch()`.

### render()
**Vai trò**
- Thực hiện vẽ lại màn hình theo sự thay đổi của `state`

```
render() {
  this.$result.text(this.state.result);
}
```

## Action
**Vai trò**
- Đang truyền `ActionType` cho `Reducer`

*ActionType*
[import:3-4](../../examples/counter/js/actions/ActionCreator.js)

*ActionCreator*
[import:6-7](../../examples/counter/js/actions/ActionCreator.js)

### Reducer
**Vai trò**
- Trả `state` mới tùy theo `ActionType` đã nhận về từ `Action`

[import:4-9](../../examples/counter/js/reducers/CounterReducer.js)

Là cơ chế trong đó nếu giá trị của `state` bị thay đổi thì sẽ chạy hàm `store.subscribe()` được định nghĩa ở `constructor` của `BaseComponent`

## Kết thúc
Sample lần này chỉ có chức năng tối thiểu cần thiết nhất thôi, phục vụ mục đích hiểu được data flow.
Ở sample khác thì sẽ tiếp tục giải thích sẽ làm như thế nào trong trường hợp truyền data cho action rồi phản ánh vào màn hình, thực hiện xử lý asynchronize, nên các bạn tiếp tục cố gắng nhé.
