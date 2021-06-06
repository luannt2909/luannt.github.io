
# Tôi đã phỏng vấn Backend Golang như thế nào:
Phần trước tôi đã giới thiệu về hành trình của tôi ờ VNG cũng như quá trình phỏng vấn ở 2 công ty là Amanotes và Rakuna.
Nhưng vẫn chưa kết thúc ở đó, ngoài 2 công ty ấy ra tôi còn phỏng vấn ở 2 công ty khác nữa, tôi xin tiếp tục như sau:

## 3. Công ty VinID và người quen cũ ở VNG:
Ở VinID tôi có một số đồng nghiệp cũ từ thời làm ở ZaloPay - VNG, tôi có nhờ các anh refer vào VinID để tìm kiếm cơ hội mới 
cho mình.
May mắn thay sau 2 ngày thì tôi cũng được mời phỏng vấn tại văn phòng của VinID ở HCM 
(lúc này đã đổi tên thành One Mount Group).

Bước vào văn phòng hơi sớm hơn thời gian một tí, chị HR đã đón tôi với nụ cười tươi rói, sau một hồi nói chuyện thì tôi
mới biết chị ấy cũng là cựu starter ở VNG luôn, nên cũng dễ chia sẻ với nhau hơn.

Sau một hồi thì tôi được gặp Head of Tech của VinID, ai mà ngờ được đó lại là anh lớn trước cũng làm ở ZaloPay luôn,
nên thành ra toàn là người quen cả các ông ạ, anh ấy tên Sơn :))

Buổi phỏng vấn bắt đầu, anh Sơn đi vào thẳng vấn đề luôn.
- A Sơn: "Em ở ZaloPay hiện tại đã và đang làm được những gì, show hết hàng ra anh xem nào".
- Tôi: "Dạ, thế để em cho anh xem một tác phẩm nghệ thuật của em."

Tay cầm bút vẽ lên bảng sơ đồ hệ thống mà tôi đang làm, đó là thiết kế hệ thống phục vụ tính năng gửi các reminder cho user 
ZaloPay như thực hiện KYC, thanh toán điện nước, liên kết ngân hàng, ...

---
Xong xuôi thì nhìn nó cũng ra gì và này nọ, cách thiết kế theo hướng microservice khá ổn cho việc scale up hệ thống.
Quan sát một hồi, A Sơn mới bắt đầu đào sâu vào hệ thống của tôi.
Vì có sử dụng queue để giao tiếp giữa các services, và đó tôi đang dùng Kafka, A Sơn hỏi tôi về Queue và cụ thể là Kafka.
+ Tại sao lại sử dụng Kafka mà không phải là cái khác như rabbitMQ hay activeMQ?
+ Kafka em đang sử dụng được config bao nhiêu partition em có biết không? và tại sao lại phải partition cho Kafka?
+ Làm cách nào để các services của em không consume các message trùng nhau?
+ Cơ chế consume, ack, commit của Kafka như thế nào?

May mắn thay, lúc trước tôi phải viết module về queue Kafka nên tôi cũng có tìm hiểu sâu về Kafka, nên thành ra tôi trả lời
các câu hỏi này cũng ổn các ông ạ.

---
Thế là vượt ải thứ nhất, A Sơn bắt đầu hỏi tiếp về cách Authentication của service mà tội expose API ra cho các bên khác gọi:
cụ thể:
+ Service của em có authentication không? và đang triển khai nó như thế nào?
+ Việc authentication này có ưu điểm và khuyết điểm như thế nào? trong các tình huống cụ thể ra sao?

> **NOTE**
Ở đây, vì là server to server(S2S) nên chúng tôi authentication bằng cách
tạo signature dựa vào các params và secret key, vd: `hash256(user_id | timestamp | secret_key)`. Phía client mỗi request 
lúc nào cũng phải gửi lên clientID + signature, đầu server cũng sẽ làm tương tự như client, sau đó sẽ so sánh 2 signatures
xem có giống nhau không?

Lại một lần nữa tôi cũng qua ải này khá ổn.

---

Đến ai thứ 3 là về database tôi lưu trữ, cụ thể với hệ thống reminder như thế thì sẽ phải lưu trữ rất nhiều reminder cho từng
user, thì tôi đang thiết kế và lưu trữ như thế nào?

Hệ thống của tôi đang dùng là mySQL, và cách partition của tôi đang là lưu theo tháng, đảm bảo tại mỗi tháng đó luôn chỉ 
chứa các reminder còn hạn mà thôi.
> Cách này khá ổn, nhưng lại hơi dở một cái là tôi phải có một cron job đi migrate data từ tháng cũ sang tháng mới.

Đến giờ, tôi cũng đang thiết kế một hệ thống gửi notifications cho user, thì tôi đã dùng mongoDB để giải quyết bài toán này
rất tốt :v

Ải database này thì tôi cũng nghĩ mình vượt qua tàm tạm, vì bản thân tôi cũng không nghĩ cách thiết kế này sẽ tối ưu lắm.

--- 
Sau cùng, A Sơn có hỏi tôi thêm một số câu hỏi như:
+ Tại sao dùng RESTAPI mà không dùng gRPC để giao tiếp giữa các internal services?
+ Tại sao dùng K8S lại gặp khó khăn trong việc triển khai gRPC?

Túm váy lại thì tôi cũng trả lời đâu đó được 70-80% các ông ạ, sau đó thì đến lượt tôi hỏi về các benefit, business, career path 
của tôi ở đây, rồi nhanh chóng kết thúc interview, vì lúc đó cũng hơn 18h chiều rồi (phỏng vấn lúc 16h30)

## 4.Công ty Sendo và ... là công ty của tôi hiện giờ :v

Không giống như các công ty ở trên phải phỏng vấn từ 1h-2h, tôi phỏng vấn ở Sendo khá nhanh (đâu đó 20-30p), và nhanh chóng
nhận được offer của công ty luôn.

Nói về Sendo thì lại là công ty mà tôi không đánh giá cao lắm so với các công ty trước, vì nói về khoản phúc lợi, môi trường 
làm việc thì tôi không ưng lắm, nhưng bù lại, còn người bên đây rất nice, nên thành ra lại là bến đỗ tiếp theo của tôi.

Quay lại buổi phỏng vấn với Sendo, tôi được hẹn phỏng vấn lúc 17h chiều, sau ngày phỏng vấn bên VinID ấy :v
Gặp 2 anh tech lead ở Sendo, các anh hỏi tôi về quá trình làm việc ở VNG, học hỏi được những gì? có khó khăn gì?
quản lý thời gian ra sao?
> Các cái này thì tôi trả lời gọn gàng luôn, vì xuất thân từ Fresher bên VNG, đi lên và làm việc được 3 năm ở VNG, nên tôi
cũng có cái gọi là trải nghiệm

Đến phần technical, 2 anh cũng hỏi tôi về tech stacks mà tôi đang sử dụng hiện tại, tại sao lại sử dụng nó như:
database, queue, cache, ...

Cụ thể thì các câu hỏi không quá chuyên sâu, nên thành ra không quá khó khăn với tôi.

Phần sau là các anh giới thiệu về cơ cấu ở Sendo, business tôi sẽ làm nếu pass phỏng vấn, cũng như
phần tương tác từ tôi như mong muốn gì khi được làm việc ở Sendo.

Mọi thứ đều chóng vánh chỉ trong 30p là xong, tôi cũng khá hài lòng về buổi phỏng vấn hôm nay.

---
Sau thời gian cân nhắc giữa 2 offers của VinID và Sendo, các benefit, môi trường làm việc các kiểu, ... Thì tôi đã
lựa chọn Sendo làm bến đỗ của tôi, vì:
+ Cảm nhận của tôi về con người ở Sendo khá tốt
+ Sự tò mò của tôi về Ecommerce, cũng như được làm việc với sản phẩm hàng triệu user.
+ Gần nhà tôi ở Quận 7 :v

## Kết chuyện:
Đấy, tôi cũng đã kể lại câu chuyện phỏng vấn của tôi khi rời bến VNG rồi ấy các ông ạ.

Thực ra bước chân ra ngoài mới thấy mọi thứ nó đều rất khác biệt với lúc các ông nghĩ, có chỗ tốt có chỗ không, được cái này
mất cái kia.

Sau cùng thì tôi cũng không hối hận với lựa chọn này của tôi, vì đến bây giờ, tôi đã học hỏi cũng như trưởng thành
hơn rất nhiều đấy ^_^

