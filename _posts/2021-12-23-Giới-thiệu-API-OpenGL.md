---
published: true
title: Giới thiệu API OpenGL
description: OpenGL (Open Graphics Library) là một giao diện lập trình ứng dụng (API) đa ngôn ngữ, đa nền tảng để kết xuất đồ họa vector 2D và 3D. API thường được sử dụng để tương tác với đơn vị xử lý đồ họa (GPU), nhằm đạt được tốc độ kết xuất phần cứng ...
tags: graphic

---

## Giới thiệu API OpenGL
OpenGL (Open Graphics Library) là một giao diện lập trình ứng dụng (API) đa ngôn ngữ, đa nền tảng để kết xuất đồ họa vector 2D và 3D. API thường được sử dụng để tương tác với đơn vị xử lý đồ họa (GPU), nhằm đạt được tốc độ kết xuất phần cứng.

Silicon Graphics, Inc. (SGI) bắt đầu phát triển OpenGL vào năm 1991 và phát hành vào ngày 30 tháng 6 năm 1992, các ứng dụng sử dụng nó rộng rãi trong các lĩnh vực thiết kế có sự hỗ trợ của máy tính (CAD), thực tế ảo, trực quan khoa học, trực quan hóa thông tin, mô phỏng chuyến bay và trò chơi điện tử. Kể từ năm 2006, OpenGL được quản lý bởi tập đoàn công nghệ phi lợi nhuận Khronos Group.
## Tổng quan về OpenGL Architecture
Trước khi nói về kiến trúc của openGL, ta hãy nói về kiến trúc của máy tính để sử lý đồ hoạ trước. Hình sau cho ta thấy hình ảnh trên màn hình được máy tính sử lý như thế nào :

![img]({{ '/assets/images/cpu_gpu_2x.png' | relative_url }}){: .center-image }

Như hình trên máy tính gồm 2 thành phần core là CPU và GPU khi ứng dụng chạy trên máy tính chúng sẽ gọi qua API/Framework openGL để yêu cầu GPU sử lí. Có thể nói sự quan trọng của GPU không khác gì CPU, GPU đảm nhiệm vai trò tính toán, sử lí gấp bội lần so với CPU. Ngày nay khi mua máy tính để làm những công việc về đồ họa, người ta thường yêu cầu mua thêm Card đồ họa rời để làm việc tối ưu hơn là vậy. Vì Card đồ họa rời chứa GPU hoạt động độc lập, chuyên xử lý tất cả dữ liệu về hình ảnh hơn Card đồ họa được tích hợp trên bo mạch chủ. Trong bài viết này mình chỉ đề cập tới API OpenGL thôi. Vì các framework dùng để lập trình đồ họa đều dựa vào API OpenGL. Hiểu dược API openGL hoạt động như thế nào thì sẽ hiểu được các framework đồ họa khác.

 Hình sau cho ta thấy kiến trúc của một API OpenGL:

![img]({{ '/assets/images/opengl-architecture.jpg' | relative_url }}){: .center-image }

Y


