Title: Viết một chatbot đơn giản với Python3
Date: 2018-05-25
Author: tung491
Tags: Python, chatbot
Category: Trang chủ

Đầu tiên, bạn cần hiểu chatbot là gì? Chatbot là một chương trình thực hiện cuộc hội thoại qua phương pháp gửi nhận văn bản hoặc các object như hình ảnh, file, ...

Có bao giờ sắp đến giao thừa hay một dịp gì đấy mà bạn muốn nhắn tin cho nhiều người vào một mốc thời gian (VD: 12h đêm) mà bạn không thể dậy được vào lúc đó, hoặc bạn quá lười để làm một việc lặp đi lặp lại? Câu trả lời của mình khi đó là viết một chatbot và hẹn giờ cho nó.

Thôi chém nữa, bắt đầu thôi!

Trong bài viết này mình sử dụng 2 lib của bên thứ 3 là [fbchat](https://fbchat.readthedocs.io/en/master/), [schedule](https://schedule.readthedocs.io/en/stable/) do đó bạn cần tạo virtualenv trước tiên, sau đó dùng pip để cài 2 lib trên. Sau đó tạo một file chatbot.py hay một tên nào đó tuỳ bạn.

Đầu tiên, import những lib mình cần đã 🎉

```
import logging
import os
import time
from threading import Thread
from fbchat import Client
from fbchat.models import Message, ThreadType
import schedule
```

Sau đó tạo một class `Bot` kế thừa `Client` :

`class Bot(Client):`

Tạo 1 function trong class `Bot` để thực hiện gửi tin nhắn, dưới đây là code của mình:

<script src="https://gist.github.com/tung491/102398d8599156c727b99e68c3680d8a.js"></script>

Và để nhận được tin nhắn từ những người gửi cho mình cho mình , ta viết function `onMessage` trong class `Bot` và xử lí các tin nhắn đó:

<script src="https://gist.github.com/tung491/a88b859645c8926548ffbe3eb044959b.js"></script>

Tham khảo thêm tại https://fbchat.readthedocs.io/en/master/

Tiếp đến, trong bài viết này mình sử dụng lib schedule để hẹn giờ chạy chương trình. Chương trình của mình chỉ chạy 1 lần duy nhất do đó đầu tiên chúng ta viết function `job_that_executes_once` :

Job cần thực hiện trong này đó là:

`Bot(os.environ['USERNAME_'], os.environ['PASSWORD']).do_something()`

Class `Bot` kế thừa `Client` do đó 2 args cần truyền vào đó là username và password của Facebook của bạn. Do đó bạn cần set value cho 2 var `USERNAME_` và `PASSWORD` bằng câu lệnh `export var=value`. Lưu ý `USERNAME_` chứ không phải `USERNAME`.

Bây giờ còn một công việc duy nhất là hẹn giờ cho job làm việc thôi!

<script src="https://gist.github.com/tung491/8ea57a2e68d620d2496d7534a1072fc3.js"></script>

Thay đổi `00:00` bằng thời gian mà bạn muốn hẹn giờ.

Để nhận được message, ta sử dụng function `listen` từ `Client` , về cơ bản `listen` khi chạy sẽ truyền các arguments vào `onMessage` mỗi lần Facebook bạn có event mới (VD: có người nhắn cho bạn, bạn nhắn cho người khác hoặc tin nhắn trong nhóm, ...):

<script src="https://gist.github.com/tung491/74c94628addd91c584c8ed216509ae7f.js"></script>

Ở function main, mình sử dụng lib threading để chạy song song 2 job là reply_msg và send_msg :

<script src="https://gist.github.com/tung491/c591d1028b092966c893396e84fd043b.js"></script>

Cuối cùng cũng xong 🎉.Sau tất cả, đây là một con chatbot hoàn chỉnh :

<script src="https://gist.github.com/tung491/6e9fce902bbc90217b84e18fce231ef6.js"></script>

Bây giờ chỉ cần chạy thôi. Và đây là thành quả:

![img]: (/home/dosontung007/Downloads/imageedit_2_6324647083.png)

Nếu bạn muốn chạy luôn mà không cần hẹn giờ thì chỉ cần xoá function `job_that_executes_once` và thay function `send_msg` bằng:

```
def send_msg():
    Bot(os.environ['USERNAME_'],os.environ['PASSWORD']).do_something()
```

HẾT.

TUNG491 at http://pymi.vn/
