---
layout: post
title: MySQL Partitioning - Tăng tốc độ truy vấn tables hàng triệu dòng.
tags: [MySQL]
---

khi số lượng dòng quá lớn, hãy lấy ví dụ một bảng Transaction lưu lại thông tin về các giao dịch của một hệ thống ngân hàng. Làm sao bạn có thể lấy ra 1 dòng tùy ý với điều kiện cho trước khi số lượng bản ghi quá nhiều? Bạn có thể trả lời, đơn giản là đánh index. Vâng, câu trả lời không sai. Nhưng vấn đề là liệu còn có cách nào khác để tối ưu tốc độ truy vấn thêm nữa? Thì partition được sinh ra như một giải pháp bổ sung cho vấn đề hiệu năng khi truy vấn. Dĩ nhiên, tăng tốc được chừng nào hay chừng đó, với hệ thống càng lớn, điều này sẽ càng thấy rõ. Ý tưởng của partition đó chính là chia 1 bảng (ở đây là bảng Transaction) thành nhiều bảng con về mặt logic theo một điều kiện nào đó, mục đích là để giảm thiểu số lượng dòng cần truy vấn cho câu truy vấn. Thay vì quét 1 triệu dòng để tìm kết quả thì ta sẽ chia 1 triệu dòng đó thành 10 phần, mỗi phần 100 ngàn dòng, và hiển nhiên, kết quả truy vấn sẽ nhanh hơn rất nhiều

Updating..





