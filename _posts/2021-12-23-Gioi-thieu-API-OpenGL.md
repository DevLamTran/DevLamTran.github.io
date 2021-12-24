---
published: true
title: Giới thiệu API OpenGL
description: >-
  OpenGL (Open Graphics Library) là một giao diện lập trình ứng dụng (API) đa
  ngôn ngữ, đa nền tảng để kết xuất đồ họa vector 2D và 3D. API thường được sử
  dụng để tương tác với đơn vị xử lý đồ họa (GPU), nhằm đạt được tốc độ kết xuất
  phần cứng ...
tags: graphic
comments: true
toc: true
---

## Giới thiệu API OpenGL
OpenGL (Open Graphics Library) là một API đa ngôn ngữ, đa nền tảng để kết xuất đồ họa vector 2D và 3D. API thường được sử dụng để lập trình tương tác với đơn vị xử lý đồ họa (GPU), nhằm đạt được tốc độ kết xuất phần cứng.

Silicon Graphics, Inc. (SGI) bắt đầu phát triển OpenGL vào năm 1991 và phát hành vào ngày 30 tháng 6 năm 1992, các ứng dụng sử dụng nó rộng rãi trong các lĩnh vực thiết kế có sự hỗ trợ của máy tính (CAD), thực tế ảo, trực quan khoa học, trực quan hóa thông tin, mô phỏng chuyến bay và trò chơi điện tử. Kể từ năm 2006, OpenGL được quản lý bởi tập đoàn công nghệ phi lợi nhuận Khronos Group.
## Tổng quan về OpenGL Architecture
Trước khi nói về kiến trúc của openGL, ta hãy nói về kiến trúc của máy tính để sử lý đồ hoạ trước. Hình sau cho ta thấy hình ảnh trên màn hình được máy tính sử lý như thế nào :

![img]({{ '/assets/images/cpu_gpu_2x.png' | relative_url }}){: .center-image }

Như hình trên máy tính gồm 2 thành phần core là CPU và GPU khi ứng dụng chạy trên máy tính chúng sẽ gọi qua API/Framework openGL để yêu cầu GPU sử lí. Có thể nói sự quan trọng của GPU không khác gì CPU, GPU đảm nhiệm vai trò tính toán, sử lí gấp bội lần so với CPU. Ngày nay khi mua máy tính để làm những công việc về đồ họa, người ta thường yêu cầu mua thêm Card đồ họa rời để làm việc tối ưu hơn là vậy. Vì Card đồ họa rời chứa GPU hoạt động độc lập, chuyên xử lý tất cả dữ liệu về hình ảnh hơn Card đồ họa được tích hợp trên bo mạch chủ. Trong bài viết này mình chỉ đề cập tới API OpenGL thôi. Vì các framework dùng để lập trình đồ họa đều dựa vào API OpenGL. Hiểu dược API openGL hoạt động như thế nào thì sẽ hiểu được các framework đồ họa khác.

 Hình sau cho ta thấy kiến trúc của một API OpenGL:

![img]({{ '/assets/images/OpenGL-Architecture.jpg' | relative_url }}){: .center-image }

Như đã nói GPU là bộ phận trên card đồ họa hoạt động độc lập với CPU, như vậy khi chương trình chạy trên máy tính muốn gọi qua GPU thì phải thông qua CPU, tương tự CPU phải thông qua API OpenGL để gọi GPU xử lí. Quá trình đó cứ lặp đi lặp lại không ngừng đến khi chương trình kết thúc.

Như hình trên ta thấy API OpenGL có các thành phần chính tương tác với nhau:

- Display List: Là bộ nhớ cache - nơi lưu lại một số lệnh để xử lý.
- Polynomial Evaluator: Là nơi tính toán các đường cong và măt phẳng hình học bằng cách đánh giá các đa thức của dữ liệu đưa vào.
- Per Vertex Operations & Primitive Assembly: Là nơi xử lý các primitive (điểm, đoạn, đa giác) được mô tả bởi các vertex. Các vertex sẽ được xử lý và các primitive được cắt xén vào viewport để chuẩn bị cho bước kế tiếp.
- Rasterization: Là nơi sinh ra một loạt các địa chỉ framebuffer và các giá trị liên quan bằng cách sử dụng mô tả 2 chiều của điểm, đoạn, đa giác. Mỗi phần tử (fragment not pixel) được sinh ra sẽ đưa vào giai đoạn kế tiếp.
- Per Fragment Operations: Là nơi thực hiện các trộn màu cho các pixel và làm một số thao tác khác sẽ được thực hiện trên dữ liệu fragment (chứa thông tin tọa độ, màu sắc,... của pixel) được sinh ra sau bước Rasterization trước khi nó được chuyển thành pixel và đưa vào Frame Buffer.
- Frame Buffer: Nơi lưu trữ lượng dữ liệu khác nhau trên mỗi pixel, nhưng trong một buffer nhất định mỗi pixel được gán cùng một lượng dữ liệu.
- Pixel Operations: Nơi tính toán dựa vào dữ liệu được lưu trong Frame Buffer và hiển thị pixel lên màn hình.
- Texture Memory: Là nơi lưu dữ liệu 1 hoặc 2 chiều các giá trị màu dạng bitmap.

Trường hợp dữ liệu vào ở dạng pixel không phải vertex, nó sẽ được đưa thẳng vào giai đoạn xử lý pixel. Sau giai đoạn này, dữ liệu ở dạng pixel sẽ được lưu trữ vào texture memory để đưa vào giai đoạn Per Fragment Operations hoặc được đưa vào Rasterization như dữ liệu dạng vertex.

## Một số thuật ngữ trong OpenGL

Vertex: là một cấu trúc diễn tả cho khái niệm điểm (point) trong không gian 3 chiều.

Pixel: là một khái niệm diễn tả cho đơn vị điểm xuất hiện trên thiết bị đồ họa (màn hình, giấy in).

Line: Xác định bởi 2 vertex

Triangle: Xác định bởi 3 vertex

Primitive: Là 1 đối tượng không gian 3 chiều được định nghĩa bởi một nhóm các vertex (có thể là điểm, đoạn thẳng, tam giác hoặc đa giác). Trong OpenGL ES, primitive giới hạn ở điểm, đoạn thẳng và tam giác.

Vậy một mô hình bất kì mà mình thấy trên máy tính là được tạo bởi các Primitive. Có thể thấy các đối tượng đồ họa trong game được tạo bởi hàng trăm, hàng ngàn các Primitive cấu thành. Hình dưới là đuôi rồng được tạo bởi các đối tượng Primitive để tạo ra đối tượng đồ họa bắt mắt sống động hơn.

![img]({{ '/assets/images/3d-openGL.png' | relative_url }}){: .center-image }

Ở bài viết này mình đã giới thiệu sơ lược về kiến trúc của API OpenGL cũng như cách mà các đối tượng đồ họa được tạo ra như thế nào thông qua API OpenGL. Cảm ơn các bạn đã đọc.


{% if page.comments == true and site.disqus.shortname %}
{% include disqus.html %}
{% endif %}
