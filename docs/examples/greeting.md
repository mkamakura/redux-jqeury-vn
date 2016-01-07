# Greeting

## Lời mở đầu
Ở sample lần này, sẽ truyền data của màn hình bằng `store.dispatch()`, và giải thích xử lý sẽ làm data đó thành `state`. Sẽ chỉ giải thích sự khác biệt với  `Counter` thôi, nên nếu có chỗ nào chưa hiểu, hãy tham khảo giải thích của `Counter`.

## Component
```js
this.$inputName = this.$selector.find('input[name=name]');
this.$selector.find('.submit').on('click', () => this.dispatch(actions.updateName(this.$inputName.val())));
```

Do sẽ đưa data vào tham số `ActionCreator` nên có thể truyền data.

## Action

```js
export const UPDATE_NAME = 'UPDATE_NAME';

export const updateName = createAction(UPDATE_NAME, (name) => name);
```
Truyền `name` vào `Reducer` bằng `createAction`.

## Reducer

[import:6-8](../../examples/greeting/js/reducers/GreetingReducer.js)

Giá trị nhận lấy từ `ActionCreator` ở trong object `action.payload`.