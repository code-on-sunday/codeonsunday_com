---
title: Tận Dụng Carbon Design System Figma Kit Để Tự Động Sinh Code Cho Hệ Thống Màu Trong App
author: code on sunday
date: 2024-03-11T09:10:58+07:00
tags:
  - HybridFlutterIDE
description: Để chuẩn bị cho việc implement một design system, mình cần chuẩn bị một số nguyên tố cơ bản như là bảng màu và chữ viết.
---
Mình dự định sử dụng [Carbon Design System]([carbondesignsystem.com](https://carbondesignsystem.com/)) làm theme cho sản phẩm Hybrid Flutter IDE sắp tới. Đó là một design system có hệ thống tài liệu rất đầy đủ (có lẽ là đầy đủ hơn cả Material Design System). 

Mình cũng thích phong cách gọn gàng, đơn giản, chuyên nghiệp của system này. Nó rất phù hợp để làm những giao diện phục vụ cho mục đích tăng hiệu suất làm việc, như giao diện của một IDE cho Flutter developer chẳng hạn.

Để chuẩn bị cho việc implement một design system, mình cần chuẩn bị một số nguyên tố cơ bản như là bảng màu và chữ viết.

Trong bài viết này, mình sẽ trình bày tóm gọn cách mà mình đã xây dựng hệ thống màu sắc trong code dựa vào Carbon Design System như thế nào.
## Color Tokens Là Gì?

Để tránh việc phải dùng từng giá trị màu cụ thể (đen, vàng, đỏ, vân vân) trong design, các design systems sử dụng khái niệm `color token` làm trung gian.

Ví dụ, định nghĩa một color token tên là `$background`. Token này sẽ có giá trị cụ thể tùy vào theme đang sử dụng là gì.
- Light theme: `$background = white`
- Dark theme: `$background = black`

Sau đó, khi viết document hoặc làm design cho một cái nút, họ sẽ đặt màu nền của nút là `$background` token chứ không nói rõ là đen hay trắng.

Như vậy, nếu sau này họ không muốn dùng màu trắng cho background trong light theme nữa, họ không cần phải sửa lại document hoặc design ở tất cả mọi nơi dùng tới button. Họ chỉ việc sửa giá trị tương ứng của `$background` ở một nơi thôi.

Nghe rất giống cách chúng ta sử dụng biến đúng không? Chính là nó đấy, chỉ là một cách gọi khác ở bên ngoài lĩnh vực lập trình thôi.

Như vậy, nếu chúng ta cũng định nghĩa một biến là `background` trong code và gán cho nó giá trị `white` hay `black` tùy vào theme mode, chúng ta cũng thu được lợi ích tương tự trong việc decouple giữa nơi sử dụng màu và nơi định nghĩa màu.

Việc của mình là làm thế nào đưa hết tất cả các color tokens của từng theme có trong Carbon Design System vào trong code.

## Làm Thế Nào Để Sinh Code Tự Động Cho Color Tokens?

Danh sách color tokens khá dài, lại còn chia thành 4 themes như trong hình. Mình không muốn ngồi khai báo từng biến, việc đó hơi mất thời gian và có vẻ không ngầu cho lắm.

Vì vậy, mình sẽ tìm cách để tự động chuyển hóa toàn bộ định nghĩa color tokens này vào code theo đúng format của Dart.

![Carbon colors](images/Pasted%20image%2020240309085121.png)

Nhờ kinh nghiệm sử dụng Figma, mình tìm ngay ra được bộ kit của Carbon Design System. Nó chứa design của tất cả các thành phần trong system, và tất nhiên cũng chứa một thành phần mình đang cần tìm đó là các color tokens.

Những tokens này được định nghĩa dưới dạng các `biến` của Figma. Chính là khái niệm `biến` trong code, nhưng hiển thị dưới dạng UI.

https://www.figma.com/file/TXscEfrRKi7XKdjLW9S2tu/(v11)-All-themes---Carbon-Design-System-(Community)?type=design&node-id=169-0&mode=design&t=69bxO1lxCgV4ANV4-0

![Figma variables](images/Pasted%20image%2020240309085844.png)
## Làm Thế Nào Để Convert Tất Cả Biến Trong Figma Thành Dạng Text?

Nếu chỉ là các biến trên UI như thế này, mình vẫn phải ngồi copy bằng tay từng cái một. Rất may mắn, mình đã tìm thấy một plugin của Figma cho phép convert tất cả biến này sang dạng JSON. Như vậy là ngon lắm rồi, vì sau đó mình có thể viết code để đọc file JSON và sinh ra code mới.

https://www.figma.com/community/plugin/1256972111705530093

Sau khi export hết các biến màu sắc bằng plugin nói trên, mình thu được một file JSON trông như mã hóa này.

![](images/Pasted%20image%2020240309091600.png)

Phải bình tĩnh đã. Sau động tác collapse đơn giản, file JSON đã được thu gọn lại chỉ còn hiển thị 5 trường thông tin ở tầng cao nhất như thế này. Đã có thể lờ mờ đoán ra ý nghĩa của chúng rồi.

![](images/Pasted%20image%2020240309091851.png)

Trường thông tin chứ nhiều dữ liệu nhất là `variables`. Vì vậy, mình chỉ lọc ra 2 phần tử trong danh sách `variables`, đưa chúng cùng với những trường thông tin còn lại sang một file JSON mới để tạo ra một phiên bản đơn giản hóa hơn nhiều.

Bằng cách này, mình có thể mang nó sang làm ví dụ để yêu cầu ChatGPT viết cho mình một đoạn code để convert dữ liệu trong file JSON này sang code trong ngôn ngữ Dart.

https://chat.openai.com/share/0b3fbac5-9552-49da-bd58-acd6d311ae8e

## Biến JSON Thành Các Color Objects

Thực tế thì code của ChatGPT viết ra chưa thể hoàn chỉnh. Mình lấy nó làm nền và bổ sung một số chỗ để xử lí những trường hợp còn thiếu như sau:
- Những biến trong file JSON nếu không phải là kiểu `color` thì sẽ không ghi vào code 
- Một số biến trong file JSON lại được gán bằng một biến khác, cần phải xử lí để lấy được giá trị thực.
- Tên của các biến trong file JSON phải được chuyển về dạng camelCase theo đúng convention của Dart.

## Kết Quả

Sau khi sửa đoạn code hoàn chỉnh, mình đã sinh ra được một abstract class chứa tất cả tên của color tokens như thế này.

```dart
abstract class CDSThemeColors {
  Color get background;
  Color get backgroundHover;
  Color get backgroundActive;
  Color get backgroundSelected;
  Color get backgroundSelectedHover;
  Color get backgroundInverse;
  Color get backgroundInverseHover;
  Color get backgroundBrand;
  Color get layer01;
  Color get layer02;
  Color get layer03;
  Color get layerHover01;
  Color get layerHover02;
  Color get layerHover03;
  Color get layerActive01;
  Color get layerActive02;
  // More ...
}
```

Và mình cũng sinh ra được 4 derived class chứa giá trị cụ thể của từng color token theo từng theme.

```dart
class WhiteThemeColors implements CDSThemeColors {
  @override
  Color get background => Color.fromARGB(255, 255, 255, 255);
  @override
  Color get backgroundHover => Color.fromARGB(141, 141, 141, 30);
  @override
  Color get backgroundActive => Color.fromARGB(141, 141, 141, 127);
  // More ...
}
```

```dart
class Gray10ThemeColors implements CDSThemeColors {
  // Implementation
}
```

```dart
class Gray90ThemeColors implements CDSThemeColors {
  // Implementation
}
```

```dart
class Gray100ThemeColors implements CDSThemeColors {
  // Implementation
}
```

Toàn bộ source code có thể xem tại đây:

[carbon-design-system/tokens_generator at master · code-on-sunday/carbon-design-system (github.com)](https://github.com/code-on-sunday/carbon-design-system/tree/master/tokens_generator)