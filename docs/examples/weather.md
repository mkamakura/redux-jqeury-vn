# Weather

## Mở đầu
Chúng tôi giải thích xử lý get data khi asynchronize từ server rồi phản ánh vào màn hình. Về các xử lý không phải xử lý asynchronize, hãy tham khảo giải thích ở file `Counter` và `Greeting`...
Trường hợp sử dụng xử lý asynchronize ở Redux, middle ware có tên `redux-thunk` sẽ được sử dụng.

## Action

[import](../../examples/weather/js/actions/ActionCreator.js)

Thực hiện asynchronize bằng hàm `updateWeather()`, tùy theo kết quả của việc này, thực hiện `dispatch` normal case `action` và abnoarmal case `action`.