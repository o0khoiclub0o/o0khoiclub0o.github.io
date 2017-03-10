---
layout: post
title:  "Sử dụng Date/Time trong API"
description: "Format datetime thế nào cho đúng? Unix time hay ISO 8601? Nên dùng time zone thế nào cho đúng?"
date:   2017-03-10 15:25:00 +0700
---

## Format datetime thế nào cho đúng?
Có 2 cách thường được dùng nhất là [Unix time](https://en.wikipedia.org/wiki/Unix_time) và [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Cả 2 đều có ưu điểm của nó, và được dùng phổ biến trong thiết kế API.

- Ví dụ API sử dụng `Unix time`
~~~
{
    "id": "2",
    "title": "Sử dụng Date/Time trong API",
    "created_at": 1489139406
}
~~~

- Ví dụ API sử dụng `ISO 8601`
~~~
{
    "id": "2",
    "title": "Sử dụng Date/Time trong API",
    "created_at": "2017-03-10T09:50:06.000Z"
}
~~~

### Vậy nên chọn cái nào?
Đương nhiên là `ISO 8601`. Nó ưu điểm hơn hẳn `Unix time`. Lần đầu tiên mình biết đến `ISO 8601` là do làm Rails. Đó là format mặc định khi gọi `to_json`. Thực ra thì từ lúc mình chuyển từ PHP qua Rails, mình học được khá nhiều convention/best practice.

Ưu điểm lớn nhất của `ISO 8601` là nó `human readable` hơn. Đây là điểm khá quan trọng trong API. Xem ví dụ ở trên.

Một ưu điểm lớn nữa là `ISO 8601` có hỗ trợ time zone, và hỗ trợ kiểu date without time `YYYY-MM-DD`. Trong nhiều trường hợp, việc lưu thêm time là không cần thiết và có thể gây hiểu lầm. Ví dụ API sử dụng format `YYYY-MM-DD`:
~~~
{
    "id": "1",
    "name": "Khôi",
    "birthday": "1990-12-20"
}
~~~

## Nên dùng time zone thế nào cho đúng?

Đầu nhận input date time từ người dùng, chấp nhận cho người dùng nhập bất kỳ format, time zone, miễn nó là `ISO 8601`. `ISO 8601` là chuẩn phổ biến nên dù cho bạn dùng ngôn ngữ nào thì việc parse lại về object Datetime ở phía backend cũng rất dễ dàng. Flexible trong đầu nhận sẽ dễ hơn cho app gọi API.

Response trả về nên là UTC, và để app tự convert về timezone của app.

Có 1 vấn đề về timezone mà mình thấy nhiều người hay làm sai là nhầm lẫn giữa giờ UTC và giờ local. Ví dụ giờ local, `2017-03-10 15:25:00 +0700`, đúng ra phải lưu trong database là `2017-03-10 08:25:00` thì lại lưu `2017-03-10 15:25:00`.

## Kết
- Dùng `ISO 8601` thay vì `Unix time`
- Nhận params là timezone bất kỳ, lưu trong database UTC, trả về UTC

## Link tham khảo
- Unix time: [https://en.wikipedia.org/wiki/Unix_time](https://en.wikipedia.org/wiki/Unix_time)
- ISO 8601: [https://en.wikipedia.org/wiki/ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)
- Designing a REST API: Unix time vs ISO-8601: [https://nbsoftsolutions.com/blog/designing-a-rest-api-unix-time-vs-iso-8601](https://nbsoftsolutions.com/blog/designing-a-rest-api-unix-time-vs-iso-8601)
- The 5 laws of API dates and times: [http://apiux.com/2013/03/20/5-laws-api-dates-and-times/](http://apiux.com/2013/03/20/5-laws-api-dates-and-times/)