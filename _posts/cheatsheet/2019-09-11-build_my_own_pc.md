---
title: "Kinh nghiệm tự build PC"
excerpt: "Tổng hợp một số kinh nghiệm khi tự build PC."
categories:
  - cheatsheet
tags:
  - tutorial
  - TODO
published: false
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---

# Tóm tắt

Post này tổng hợp một số kinh nghiệm khi tự build PC. Cũng như nhiều bạn khi tự build PC, tôi cũng tốn quá nhiều thời gian phân tích, tìm kiếm, so sánh. Thành quả cuối cùng vẫn là một con máy tính to tổ bố, dùng tàm tạm. Qui ra thì chỉ tiết kiệm được một tý so với việc mua con PC đã lắp sẵn. 

Hy vọng manual nhỏ này sẽ giúp các bạn build được một bộ PC ưng ý mà không tốn quá nhiều thời gian vô ích. 

# Chi tiết 

Manual sẽ gồm các bước sau

1. Xem xét ngân sách
2. Xác định rõ nhu cầu bản thân
3. Các bộ phận thiết yếu
4. Chọn main (motherboard)
5. Chọn vỏ máy tính (PC case)
6. Chọn chip (CPU)
7. Chọn RAM (memory)
8. Chọn ổ cứng 
9. Chọn Card màn hình (GPU)
10. Chọn nguồn (Power supply unit :PSU)
11. Chọn các thiết bị khác 
12. Bắt đầu lắp đặt
13. Một số câu hỏi thường gặp 

## 1. Xem xét ngân sách

Bước đầu tiên, bạn phải xác định là mình muốn chi bao nhiêu tiền cho bộ PC mới. Tôi tự phân ra thành các bậc như sau

1. ngân sách < 15 triệu : 
2. 15 triệu < ngân sách < 25 triệu
3. 25 triệu < ngân sách < 40 triệu
4. 40 triệu < ngân sách < vài trăm triệu

Tôi gọi 4 mức phân cấp ngân sách trên là : nghèo, vừa phải, giàu, rất giàu. 

Một điều cần chú ý ở đây. Các nhà sản xuất linh kiện (main, chip, case, ...) hiểu rất rõ sự phân cấp trong túi tiền của người tiêu dùng. Cho nên các sản phẩm của họ cũng chia thành 4 phân khúc : hạ, trung, thượng, doanh nghiệp (enterprise). 

Sau khi xác định túi tiền bản thân, bạn sẽ hiểu được bạn nằm trong phân khúc khách hành nào. Từ đây bạn sẽ chọn lịnh kiện (part) tương ứng với phân khúc đó. 

## 2. Xác định rõ nhu cầu bản thân

Các nhu cầu cần xác định rõ

1. Mục đích sử dụng : mua để làm các tác vụ văn phòng, xem phim, lướt web, chơi game, lập trình, ....
2. Nhu cầu về thẩm mĩ: nhỏ, gọn hay to lớn kềnh càng, dây nhợ loằng ngoằng cũng được hay không dây cho thẩm mĩ
3. Nhu cầu về nghe nhìn: màn hình to hay bé, âm thanh trung thực sống động hay rè rè một chút cũng được

Tôi cũng phân mục đích sử dụng ra làm các cấp và tiêu chí mua sắm như sau

1. Làm văn phòng, xem phim, lướt web : mua đồ dành cho phân khúc nghèo
2. Chơi game, lập trình : mua đồ dành cho phân khúc vừa phải
3. CAD, dựng phim, Compiling, DeepLearning : mua đồ cho phân khúc giàu và rất giàu 

Về thẩm mĩ, nghe nhìn thì tôi phân như sau 

1. Làm văn phòng, xem phim, lướt web : nên mua máy cỡ nhỏ, dùng phím chuột không dây, màn hình to một chút
2. Chơi game, lập trình, CAD, dựng phim, Compiling, DeepLearning : mua máy cỡ vừa phải, phím chuột có dây, màn hình càng nhiều càng tốt 

## 3. Các bộ phận thiết yếu

Các bộ phận của một bộ PC hoàn chỉnh

1. Motherboard/Main
2. CPU/Chip
3. Memory
4. Ổ cứng
5. Bộ nguồn
6. Case
7. Màn hình
8. Phím, chuột

Các bộ phận khác, có cũng được không có cũng chẳng sao

1. Đầu đọc đĩa
2. Card màn hình (GPU)
3. Các loại card rời : card âm thanh, card LAN, card wifi, ....
4. Loa
5. Tai nghe

Đến đây thì bạn đã mường tượng ra được mình nên mua cái gì, nên mua như thế nào rồi. Bước tiếp theo tôi sẽ chỉ bạn cách tìm linh kiện phù hợp trong thời gian ngắn nhất. 

## 4. Chọn main (motherboard)

Main (Motherboard) là gì ? Main (Motherboard) là thiết bị để kết nối (connect) tất cả các thiết bị với nhau. 

Chỉ cần gõ Motherboard trên google, bạn sẽ có hằng hà vô số giải thích từ dễ tới khó và bạn không cần thiết phải quan tâm những thứ đó. Bạn chỉ cần hiểu 2 thứ sau

2. Chipset : quyết định số lượng thiết bị có thể kết nối và tốc độ của các kết nối đấy.
1. Kích cỡ của main (Form factor)

Chú ý: có hai hãng sản xuất Chip lớn nhất là Intel, AMD. khi bạn chọn chipset của Intel thì phải mua chip của Intel và ngược lại. Không thể dùng lẫn lộn. 

### 4.1 Chọn Chipset

Trong 3 yếu tố trên thì Chipset là yếu tố quan trọng nhất. Một mainboard có chipset xịn thì sẽ gắn được nhiều thiết bị hơn, băng thông truyền tải dữ liệu rộng hơn. Một mainboard có chipset xịn thì gắn được ít thiết bị hơn, băng thông  truyền tải dữ liệu hẹp hơn. Trong một số trường hợp khi thiết bị cuối của bạn (CPU, RAM, ...) quá xịn, băng thông sẽ bị bóp lại. Dẫn đến cảnh, mua đồ xịn mà chạy như rùa. 

Các chipset cũng được chia thành các rank khác nhau. Tùy theo túi tiền mà chúng ta nên chọn một chipset phù hợp. 

**Intel**

1. Các chipset bắt đầu bằng chữ H: H110, H170, H270, H310, H370 . Đây là dòng dành cho Home user, người dùng ít tiền. 
2. Các chipset bắt đầu bằng chữ B: B150, B250, B360, H365 . Đây là dòng dành cho Business user, người dùng có túi tiền vừa phải. 
2. Các chipset bắt đầu bằng chữ cái khác như Z, Q như: Z270, Z370. Đây là dòng cao cấp, dành cho người dùng lắm tiền nhiều của. 

![Intel Chipset](https://cdn.wccftech.com/wp-content/uploads/2018/05/Intel-Chipset-Roadmap.png)

**AMD**

1. Các chipset bắt đầu bằng chữ A: A320 . Đây là dòng dành cho Home user, người dùng ít tiền. 
2. Các chipset bắt đầu bằng chữ B: B350, B450 . Đây là dòng dành cho Business user, người dùng có túi tiền vừa phải. 
3. Các chipset bắt đầu bằng chữ X như: X370, X470, X570. Đây là dòng cao cấp, dành cho người dùng lắm tiền nhiều của. 

![AMD Chipset](https://cdn.wccftech.com/wp-content/uploads/2018/05/AMD-Chipset-Roadmap-2018.png)

Tùy theo nhu cầu công việc mà tôi phân như sau: 

1. Làm văn phòng, xem phim, lướt web : dòng H
2. Chơi game, lập trình: Dòng B hoặc Z 
3. CAD, dựng phim, Compiling, DeepLearning : Dòng Z hoặc cao hơn

### 4.2 Chọn  Kích cỡ của main

Main (Motherboard) có các kích cỡ như hình dưới. 

![motherboard form factor](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2c/Motherboards_form_factors.svg/719px-Motherboards_form_factors.svg.png)

Tùy theo nhu cầu công việc mà tôi phân như sau: 

1. Làm văn phòng, xem phim, lướt web : Mini-ITX 
2. Chơi game, lập trình' Micro-ATX 
3. CAD, dựng phim, Compiling, DeepLearning : ATX hoặc to hơn 

## 5. Chọn vỏ máy tính (PC case)

Sau khi quyết định kích cỡ Motherboard thì chúng ta bắt đầu chọn PC case tương ứng với kích cỡ đó. 

## 6. Chọn chip (CPU)


## 7. Chọn RAM (memory)
## 8. Chọn ổ cứng 
## 10. Chọn nguồn (Power supply unit :PSU)

# Case study 

## 

End
