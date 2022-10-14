---
published: true
title: Giới thiệu API OpenGL
description: API cơ bản dùng để lập trình đồ họa
tags: graphic
toc: true
---


# Giới thiệu API OpenGL

OpenGL (Open Graphics Library) là một API đa ngôn ngữ, đa nền tảng để kết xuất đồ họa vector 2D và 3D. API thường được sử dụng để tương tác với GPU (Graphics Processing Unit), nhằm đạt được tốc độ kết xuất phần cứng.

Silicon Graphics, Inc. (SGI) bắt đầu phát triển OpenGL vào năm 1991 và phát hành vào ngày 30 tháng 6 năm 1992, các ứng dụng sử dụng nó rộng rãi trong các lĩnh vực thiết kế có sự hỗ trợ của máy tính (CAD), thực tế ảo, trực quan khoa học, trực quan hóa thông tin, mô phỏng chuyến bay và trò chơi điện tử, OpenGL được quản lý bởi tập đoàn công nghệ phi lợi nhuận Khronos Group.

Sau khi API OpenGL ra đời thì một loạt các API/framework đồ họa ra đời như : JMonkeyEngine, LWJGL, JOGL, DirectX,... đều dựa trên nguyên lí hoạt động của API OpenGL. Sau này các công ty công nghệ đều lấy API OpenGL làm nền tảng để phát triển các framework đồ họa khác.

# Tổng quan về OpenGL Pipeline

Trước khi nói về pipeline (quy trình) của openGL, ta hãy nói về pipeline của máy tính trong việc sử lý đồ hoạ trước. Hình sau cho ta thấy hình ảnh trên màn hình được máy tính sử lý như thế nào :

![img]({{ '/assets/images/cpu_gpu_2x.png' | relative_url }}){: .center-image }

Như hình trên máy tính gồm 2 thành phần core là CPU và GPU khi ứng dụng chạy trên máy tính chúng sẽ gọi qua API/Framework openGL để yêu cầu GPU sử lí. Có thể nói sự quan trọng của GPU không khác gì CPU, GPU đảm nhiệm vai trò tính toán, sử lí gấp bội lần so với CPU. Ngày nay khi mua máy tính để làm những công việc về đồ họa, người ta thường yêu cầu mua thêm Card đồ họa rời để làm việc tối ưu hơn là vậy. Vì Card đồ họa rời chứa GPU hoạt động độc lập, chuyên xử lý tất cả dữ liệu về hình ảnh hơn Card đồ họa được tích hợp trên bo mạch chủ. Trong bài viết này mình chỉ đề cập tới API OpenGL thôi. Vì các framework dùng để lập trình đồ họa đều phát triển dựa trên API OpenGL. Hiểu dược API openGL hoạt động như thế nào thì sẽ hiểu được các framework đồ họa khác hoạt động như thế nào.

 Hình sau cho ta thấy pipeline của một API OpenGL:

![img]({{ '/assets/images/OpenGL_Pipeline.jpg' | relative_url }}){: .center-image }

Như đã nói GPU là bộ phận trên card đồ họa hoạt động độc lập với CPU, như vậy khi chương trình chạy trên máy tính muốn gọi qua GPU thì phải thông qua CPU, tương tự CPU phải thông qua API OpenGL để gọi GPU xử lí. Quá trình đó cứ lặp đi lặp lại không ngừng đến khi chương trình kết thúc.

Như hình trên là lược đồ do Henry Ford đề xuất cách mà API OpenGL xử lý dữ liệu đầu vào như thế nào:

- Display List: Dùng để chứa tất cả dữ liệu (vertex, pixel,các lệnh xử lý) phục vụ cho việc sử lý tính toán của các bước sau này, nó cũng được coi là bộ nhớ cache cho GPU.
- Evaluator: Là bước tính toán các đường cong và măt phẳng hình học bằng các đa thức toán học nhờ vào dữ liệu vertex đầu vào.
- Per Vertex Operations & Primitive Assembly: Là bước tính toán các primitive (point, line, polygon) dựa vào dữ liệu vertex đầu vào. Các vertex sẽ được xử lý và các primitive được cắt xén vào viewport để chuẩn bị cho bước kế tiếp.
- Rasterization: Là bước chuyển đổi dữ liệu primitive và pixel thành các loạt các fragment. Mỗi một fragment tương ứng với một pixel trong framebuffer. Sau đó tính toán các fragment liên quan bằng cách sử dụng mô tả 2 chiều của point, line, polygon. Mỗi phần tử (fragment not pixel) được sinh ra sẽ đưa vào giai đoạn kế tiếp.
- Per Fragment Operations: Là bước thực hiện các trộn màu và làm một số thao tác khác như :Blending, Antialiasing, Fog,... trên dữ liệu fragment (chứa thông tin tọa độ, màu sắc,... của pixel) được sinh ra sau bước Rasterization trước khi nó được chuyển thành pixel và đưa vào Frame Buffer.
- Frame Buffer: Nơi lưu trữ lượng dữ liệu khác nhau trên mỗi pixel, nhưng trong một buffer nhất định mỗi pixel được gán cùng một lượng dữ liệu.
- Pixel Operations: Là một loạt các bước tính toán (scale, bias, mapping and clamping) dựa vào dữ liệu pixel được lưu trong Frame Buffer và dữ liệu đầu ra sẽ được đóng gói thành một định dạng thích hợp và được lưu trong bộ nhớ máy tính.
- Texture Memory: Dùng để lưu các giá trị màu và lưu dữ liệu 1 hoặc 2 chiều các giá trị màu dạng bitmap.

**Lưu ý: Mỗi bước xử lý của API OpenGL đều phải thông qua GPU, yêu cầu GPU xử lý, như vậy có thể kết luận pipeline xử lý đồ hoạ của GPU cũng chính là pipeline của API OpenGL.**

>Trường hợp dữ liệu vào ở dạng pixel không phải vertex, nó sẽ được đưa thẳng vào giai đoạn xử lý pixel. Sau giai đoạn này, dữ liệu ở dạng pixel sẽ được lưu trữ vào texture memory để đưa vào giai đoạn Per Fragment Operations hoặc được đưa vào Rasterization như dữ liệu dạng vertex.

# Một số thuật ngữ trong OpenGL

- Vertex: là một cấu trúc diễn tả cho khái niệm điểm (point) trong không gian 3 chiều.

- Pixel: là một khái niệm diễn tả cho đơn vị điểm xuất hiện trên thiết bị đồ họa (màn hình, giấy in).

- Point: Xác định bởi 1 vertex

- Line: Xác định bởi 2 vertex

- Polygon: Xác định từ 3 vertex trở lên

- Primitive: Là 1 đối tượng không gian 3 chiều được định nghĩa bởi một nhóm các vertex (có thể là điểm, đoạn thẳng, tam giác hoặc đa giác)

Trong OpenGL ES, chỉ giới hạn các primitive như : point, line và polygon.


> Vậy một đối tượng đồ họa phức tạp mà mình thấy trên máy tính được render như thế nếu sử dụng API OpenGL ?

Câu trả lời là chúng được tạo bởi nhiều đối tượng primitive. Có thể thấy các đối tượng đồ họa trong game được tạo bởi hàng trăm, hàng ngàn, thậm chí hàng triệu các Primitive cấu thành. Hình dưới là đuôi rồng được tạo bởi các đối tượng Primitive để tạo ra đối tượng đồ họa bắt mắt sống động hơn.

![img]({{ '/assets/images/3d-openGL.png' | relative_url }}){: .center-image }

Ở bài viết này mình đã giới thiệu sơ lược về kiến trúc của API OpenGL cũng như cách mà các đối tượng đồ họa được tạo ra như thế nào thông qua API OpenGL. Cảm ơn các bạn đã đọc.



