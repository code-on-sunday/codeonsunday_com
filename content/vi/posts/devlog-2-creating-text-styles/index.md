---
title: "Devlog #2: Tạo Một Loạt TextStyle Trong Flutter Bằng ChatGPT"
author: code on sunday
date: 2024-03-12T06:31:49+07:00
description: Trong buổi số 2, mình sẽ cài đặt một loạt TextStyle objects dựa trên thông số được ghi trong tài liệu của Carbon Design System.
tags:
  - HybridFlutterIDE
---
Trong buổi số 2, mình sẽ cài đặt một loạt TextStyle objects dựa trên thông số được ghi trong tài liệu của Carbon Design System.
## Lên Kế Hoạch Trước Khi Triển Khai

Trước khi đi vào triển khai, mình dành khoảng 30 phút để nghiên cứu tài liệu về Text (trong này gọi là Typography) của Carbon Design System.
Mỗi style trong này cũng giống như TextStyle bên Flutter, bao gồm kích cỡ, chiều cao dòng, độ dày, khoảng cách chữ. Mỗi style có một tên riêng, giống như color token mình đã nói đến ở [buổi số 1](../generate-color-tokens/index).

```
body-compact-01  
Type: IBM Plex Sans  
Size: 14px / .875rem  
Line height: 18px / 1.125rem  
Weight: 400 / Regular  
Letter spacing: .16px  
Type set: Productive
```

Tuy nhiên, có một số style lại có nhiều bộ thông số tương ứng với cùng một token, tùy vào breakpoint hiện tại là gì. Mình tạm gọi là responsive style. Ví dụ:
- Ở cỡ màn hình < 672 (breakpoint `sm`)
```
fluid-display-03  
Type: IBM Plex Sans  
Size: 42px / 2.625rem  
Line height: 50px / 3.125rem  
Weight: 300 / Light  
Letter spacing: 0px  
Type set: Expressive
```
- Ở cỡ màn hình < 1056 (breakpoint `md`)
```
fluid-display-03  
Type: IBM Plex Sans  
Size: 54px / 3.375rem  
Line height: 64px / 4rem  
Weight: 300 / Light  
Letter spacing: 0px  
Type set: Expressive
```

Nếu các responsive style này được sử dụng nhiều trong design, mình chắc chắn phải định nghĩa chúng đầy đủ. Tuy nhiên, với nhu cầu hiện tại, có lẽ mình chỉ cần những style cố định là đủ. Quyết định như thế này sẽ giúp mình giảm thời gian làm việc đi khá nhiều.

## Sử Dụng ChatGPT Để Code TextStyle

Bây giờ, việc của mình đơn giản là đọc từng bộ thông số tương ứng với từng style để viết code Flutter tương ứng. Công việc này tuy đơn giản nhưng khá là chán, tại sao mình không dùng ChatGPT chứ?

![](images/Pasted%20image%2020240311215252.png)

Chỉ bằng 2 câu prompt đơn giản, mình đã lấy được code cho tất cả Utility styles.

Tham khảo tại đây: https://chat.openai.com/share/cce03137-0d4b-49ab-a3ff-91647383be38

Code sẽ trông như thế này:

```dart
extension TextStyleGetter on BuildContext {
  TextStyle get code01 => TextStyle(
        fontFamily: 'IBM Plex Mono',
        fontSize: 12.0,
        height: 16.0 / 12.0,
        fontWeight: FontWeight.w400,
        letterSpacing: 0.32,
      );
}
```

Ở đây, mình trả về TextStyle thông qua extension của BuildContext để sau này nếu cần trả về những styles khác nhau phụ thuộc vào kích thước màn hình thì không cần phải sửa code ở những nơi đang sử dụng những styles này.

Sau một hồi copy paste, mình đã tạo ra được một danh sách dài các TextStyle cần thiết, vậy là đã sẵn sàng để bắt tay vào làm những widget tuyệt đẹp ở buổi thứ 3 rồi!

Source code xem tại đây: [code-on-sunday/carbon-design-system (github.com)](https://github.com/code-on-sunday/carbon-design-system)