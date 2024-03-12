---
title: "Devlog #3: Cài Đặt Ban Đầu Cho Một Flutter Project Mới"
author: code on sunday
description: Bài viết này sẽ được cập nhật thêm để tạo thành một danh sách tổng hợp hỗ trợ cài đặt Flutter project mới nhanh chóng
date: 2024-03-12T09:23:10+07:00
tags:
  - HybridFlutterIDE
---
## Tạo Nhanh Flutter Project Mới Trong VSCode

Đầu tiên, mở Command Palette của VSCode bằng tổ hợp `Ctrl Shift P` (Windows) hoặc `Cmd Shift P` (MacOS)

Sau đó gõ `flutter new` và chọn kết quả gợi ý đầu tiên `Flutter: New project`.

![](images/Pasted%20image%2020240312064558.png)

Tiếp theo, chọn loại template mà bạn muốn dựa vào để tạo Flutter project mới. Hãy chọn dòng thứ 2: `Empty Application`.

![](images/Pasted%20image%2020240312064928.png)

Tiếp theo, chọn folder cha để chứa folder project mà bạn chuẩn bị tạo.

Cuối cùng, điền tên của project, đồng thời cũng là tên của folder project sẽ được tạo ra bên trong folder cha mà bạn vừa chọn.

Chú ý tên project chỉ có thể gồm kí tự alphabet và dấu `_`.

Bằng cách này, bạn sẽ tạo ra được một Flutter project mới chứa ít code nhất có thể, hoàn toàn không có những comment và code ví dụ thừa thãi.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MainApp());
}

class MainApp extends StatelessWidget {
  const MainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: Scaffold(
        body: Center(
          child: Text('Hello World!'),
        ),
      ),
    );
  }
}
```

## Sử Dụng very_good_analysis

Mặc định, trong Flutter project mới đã áp dụng sẵn bộ analysis options của flutter_lints để giúp phát hiện những chỗ code xấu (không tuân thủ conventions, practices) trong project.

Nhưng bản thân mình thấy vẫn còn chưa đủ. Ví dụ, khi gọi một hàm trả về kiểu `Future`, nhiều khi mình quên không `await`. Nếu đang test ngay chỗ đó thì sẽ biết và sửa được ngay, nhưng nếu để bẵng đi một vài giờ, có khi sẽ không nhớ đến nữa và lại phải ngồi debug để biết tại sao code không chạy như kì vọng.

Vì vậy, mình thêm package `very_good_analysis` để có một bộ analysis options được trang bị sẵn đầy đủ hơn. Như ở trường hợp trên, nó sẽ nhắc nhở mình chưa `await` ngay chỗ gọi hàm đó.

![](images/Pasted%20image%2020240312070341.png)

## Một Số Package Khác

| Mục đích           | Package                                                                                                                                      |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Quản lý trạng thái | flutter_bloc                                                                                                                                 |
| UI theme           | carbon_design_system (đang tự phát triển trong [devlog #1](generate-color-tokens/index) và [devlog #2](devlog-2-creating-text-styles/index)) |
| Icons              | carbon_icons                                                                                                                                 |
Mình sẽ bổ sung các package khác sau khi hoàn thành một vài giao diện đầu tiên. Còn trước hết, mình sẽ phải vẽ phác thảo UI đã. Tiếp tục theo dõi trong bài tiếp nhé.
