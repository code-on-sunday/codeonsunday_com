---
title: "Đắn Đo Mãi, Cuối Cùng Vẫn Chọn Bloc Cho Dự Án Flutter Tại Công Ty"
date: 2024-02-08T06:31:25+07:00
description: "Khi khởi động một dự án Flutter cho công ty, mình đã mất khá nhiều công sức để quyết định nên sử dụng công cụ quản lý trạng thái nào (State Management). Trong Flutter, có 3 ứng viên sáng giá cho vị trí này là: Provider, Riverpod, và Bloc. Mình đã tìm hiểu và thử nghiệm cả 3 thư viện này theo những tiêu chí dưới đây. Chúng ta cùng xem kĩ hơn về từng tiêu chí này để hiểu lí do tại sao mình chọn Bloc cho dự án này."
---

Khi khởi động một dự án Flutter cho công ty, mình đã mất khá nhiều công sức để quyết định nên sử dụng công cụ **quản lý trạng thái** nào (State Management).

**State Management** là một thành phần quan trọng của mọi dự án front-end. Các thư viện state management đều có những functions mà chúng ta phải gọi chúng ngay trong phần code giao diện.

Đối với mình, điều này đồng nghĩa với việc, dự án sẽ tích hợp rất sâu với thư viện state management mà chúng ta chọn.

Điều đó cũng có nghĩa là dự án sẽ _phụ thuộc_ vào thư viện đó rất nhiều (**coupling**), và hầu như chúng ta không nên chuyển đổi sang thư viện khác sau này vì sẽ mất rất nhiều thời gian và công sức.

Vậy nên, chọn thư viện state management là một dạng quyết định một lần và mãi mãi, dẫn tới phải cân nhắc rất kỹ lưỡng.

Trong Flutter, có 3 ứng viên sáng giá cho vị trí này là: **Provider**, **Riverpod**, và **Bloc**.

Mình đã tìm hiểu và thử nghiệm cả 3 thư viện này theo những tiêu chí dưới đây. Chúng ta cùng xem kĩ hơn về từng tiêu chí này để hiểu lí do tại sao mình chọn **Bloc** cho dự án này.

Có một lưu ý nhỏ là, phân tích dưới đây dựa trên một dự án cụ thể. Khi gặp một dự án khác, bạn hãy tự định hình yêu cầu của dự án là gì và tự phân tích từng tiêu chí để chọn ra thư viện phù hợp nhất.

## 1. Các Ứng Viên State Management: Provider, Riverpod, Bloc

Đầu tiên, tại sao mình lại chọn ra 3 ông thư viện này?

### a. Provider:

Mặc dù tác giả của Provider (**Remi Rousselet**) cũng chính là tác giả của **Riverpod**, và hiện nay Remi tập trung vào phát triển Riverpod hơn dẫn tới nguy cơ Provider sẽ bị bỏ rơi (**abandoned**) trong tương lai, nhưng mình vẫn đưa nó vào danh sách vì tính đơn giản và "chính thống" của nó.

Trong Flutter, cơ chế có sẵn trong SDK để phục vụ mục đích chia sẻ trạng thái (**state**) giữa các widget trên cây chính là **InheritedWidget**.

Flutter SDK cung cấp một vài functions để một widget ở vị trí bất kì trên cây có thể lấy ra được một InheritedWidget ở vị trí ông cha (**ancestor**) của nó trên cây. Đồng thời, nó cũng có thể đăng ký để nhận thông báo khi dữ liệu trong InheritedWidget thay đổi, từ đó cập nhật lại giao diện để hiển thị dữ liệu mới.

Đó chính là những tính năng cơ bản của một công cụ State Management, và InheritedWidget đáp ứng tốt điều này.

Tuy nhiên, mỗi lần cài đặt một InheritedWidget, chúng ta phải viết rất nhiều code (**boilerplate**). Hơn nữa, nó cũng quá đơn giản, khiến developers phải tự bổ sung những tính năng cần thiết khác.

Đó là lí do **Provider** ra đời. Nó gói lại InheritedWidget, cung cấp cách tiếp cận mới đơn giản hơn, và cũng cung cấp thêm một số tính năng mà InheritedWidget không có.

Như vậy, trong tương lai, dù Provider có bị bỏ rơi thì chúng ta vẫn có thể kéo code (**fork**) của nó về để tự maintain tiếp. Hơn nữa, do nguyên lý hoạt động đơn giản, nên thực ra Provider đã đạt đến độ ổn định cao rồi, khả năng bị lỗi là rất thấp.

### b. Riverpod:

Như Remi Rousselet đã nói, Riverpod là một thư viện state management mới, được xây dựng với mục tiêu **giải quyết những vấn đề mà Provider gặp phải**. Ví dụ như vấn đề phải phụ thuộc vào BuildContext thì mới truy cập được trạng thái chung.

Về bản chất, Riverpod tạo ra một cơ chế _cây element_ mới, khác hẳn với cây element của Flutter. Đó là lí do nếu sử dụng Riverpod, chúng ta phải tạo ra widget bằng cách kế thừa các loại widget mới chứ không phải **StatelessWidget** hay **StatefulWidget** thông thường.

Chính nhờ cơ chế này mà Riverpod cho phép truy cập đến trạng thái chung từ bất kỳ đâu mà không cần phụ thuộc vào BuildContext.

Vì khả năng mạnh mẽ này mà mình đưa Riverpod vào danh sách ứng viên.

### c. Bloc:

Bloc là một thư viện state management rất phổ biến trong cộng đồng Flutter. Nó được xây dựng dựa trên **BLoC** pattern (Business Logic Component) do Google gợi ý trong một bài thuyết trình về Flutter.

Bloc không chỉ cung cấp các công cụ quản lý state, nó còn gợi ý chúng ta cấu trúc dự án theo BLoC pattern với 3 tầng riêng biệt: **Presentation**, **Business Logic**, và **Data** giúp dự án có cấu trúc rõ ràng, dễ bảo trì.

Nhìn chung, Bloc phức tạp hơn Provider, nhưng vẫn gần gũi với Flutter vì nó không cần phải tạo ra một cơ chế mới như Riverpod. Ngoài ra, nó cũng cực kỳ phổ biến. Đó là những lí do mình đưa Bloc vào danh sách ứng viên.

## 2. Có Dễ Học Không? (Learning Curve)

Để có căn cứ đánh giá các lựa chọn, mình dựa vào tình hình thực tế của dự án. Trong team, ngoại trừ mình ra, tất cả đều chưa từng làm việc với Flutter trước đó. Mặc dù ở giai đoạn đầu, chỉ có 2 người trong team khởi động dự án này, trong đó có mình, nhưng sau một thời gian ngắn, khả năng cao sẽ có thêm người tham gia để tăng tốc độ phát triển.

Vì vậy, khả năng dễ học của thư viện state management là một tiêu chí quan trọng. Mọi người không những mất thời gian học Dart và Flutter, mà còn phải học thêm về thư viện state management. Tốn quá nhiều thời gian học sẽ là một điểm trừ lớn.

- **Provider**: 5/5

  Dễ học nhất phải kể đến Provider. Thông thường chúng ta sẽ kết hợp Provider với **ChangeNotifier** để quản lý trạng thái. Như vậy, để sử dụng Provider, developers chỉ cần nắm được 2 khái niệm cơ bản này là đủ.

- **Riverpod**: 2/5

  Riverpod chỉ hơi khó hơn một chút vì nó có nhiều khái niệm hơn. Nhưng lí do mình chỉ chấm 2 điểm là bởi vì khi đặt trong bối cảnh hầu hết mọi người đều chưa biết Flutter, thì việc dung nạp một lúc cả Flutter và Riverpod sẽ trở nên khó hơn gấp đôi do phải học cả 2 thứ mới trong thời gian quá ngắn.

- **Bloc**: 4/5

  Bloc hoạt động dựa trên Stream. Developers phải học về Stream một chút, tiếp đó về cấu trúc phân tầng và khái niệm về Event và State, nên tất nhiên là khó hơn Provider nhưng lại không khó bằng Riverpod. Đồng thời, Bloc đã có cộng đồng lớn, tồn tại lâu, nên có rất nhiều tài liệu và ví dụ mẫu trên mạng, giúp developers dễ dàng học hơn. Nó cũng không làm thay đổi bản chất của Flutter nên không tạo áp lực lên người mới.

## 3. Có Cấu Trúc Không?

Cấu trúc dự án là một tiêu chí quan trọng khác mà mình cân nhắc. Khi hầu hết mọi người đều chưa có kinh nghiệm với Flutter, việc có một cấu trúc dự án rõ ràng, thậm chí nếu cấu trúc đó quen thuộc với họ ở một framework khác, sẽ giúp họ bắt kịp nhanh hơn nhiều, vì nó làm giảm tải lượng kiến thức mà họ phải tiếp thu.

Nhiều khi, chỉ cần biết class này nằm ở tầng nào là developers đã đoán được chức năng và cách sử dụng của nó, dù không hiểu cụ thể các khái niệm bên trong thì vẫn có thể viết, sử dụng và bảo trì nó được.

- **Provider**: 2/5

  Provider không gợi ý cấu trúc dự án nào cả. Developers có thể đặt ChangeNotifier ở bất kì đâu trong dự án, và không có quy tắc nào cả. Điều này dẫn tới việc dự án có thể trở nên rối rắm nếu không có người quản lý cẩn thận.

  Đặc biệt khi đặt trong bối cảnh deadline gấp gáp, không có cả thời gian và nguồn lực để review và hướng dẫn tỉ mỉ cho các thành viên, thì việc không có cấu trúc sẵn là một điểm trừ lớn.

- **Riverpod**: 4/5

  Riverpod tách biệt **Provider** và **Notifier** ra khỏi widget và có thể truy cập dễ dàng từ bên trong widget. Điều này giúp tạo ra một tầng riêng biệt để quản lý trạng thái. Tuy nhiên, trên trang chủ của Riverpod, không có mô hình cấu trúc cụ thể nào được gợi ý cả.

  Như Andrea Bizzotto, một blogger cực kì chuyên sâu về Flutter, cũng đã viết trong một bài blog của mình rằng, [Riverpod không có định hướng cấu trúc nào cả](https://codewithandrea.com/articles/flutter-app-architecture-riverpod-introduction/#riverpod-is-not-very-opinionated).

  Tất nhiên, có một chút còn hơn không, nên mình đánh giá 4 điểm cho Riverpod.

- **Bloc**: 5/5

  Ngay trên trang chủ của Bloc đã giới thiệu về mô hình BLoC kèm theo giải thích kĩ lưỡng. Ngoài ra, cả trang chủ và cộng đồng cũng đã tạo ra rất nhiều ví dụ mẫu và hướng dẫn về cách cấu trúc dự án theo mô hình BLoC.

  Như vậy, nếu có gì chưa rõ, developers hoàn toàn có thể tự mình tìm hiểu, tiết kiệm được rất nhiều thời gian. Mình cho 5 điểm hoàn toàn xứng đáng.

## 4. Có Mạnh Mẽ Không?

Thực ra, không cần tới thư viện thì chúng ta vẫn có thể tự code ra những thành phần phục vụ nhu cầu của mình. Nhưng nếu thư viện cung cấp sẵn những công cụ đó thì tội gì phải tự làm lại từ đầu đúng không?

- **Provider**: 4/5

  Chính vì quá đơn giản nên Provider không có nhiều công cụ hỗ trợ.

  Ví dụ, **ChangeNotifier** chỉ là cơ chế gọi hàm callback của listeners để thông báo về sự thay đổi trạng thái. Nó không thể cung cấp nhiều **operators** mạnh mẽ để xử lý dữ liệu như **Stream**. Tất nhiên chúng ta hoàn toàn có thể đặt một **StreamController** vào trong ChangeNotifier để tạo ra một Stream, nhưng như đã nói, ở đây mình đang đánh giá mức độ sẵn có các công cụ hỗ trợ.

  Ngoài ra, khi muốn trigger một hành động nào đó khi trạng thái thay đổi, chúng ta phải tạo ra **StatefulWidget** để gọi hàm **addListener** và **removeListener**, hoặc phải tạo ra một widget riêng để tái sử dụng.

  Tất nhiên, Provider có trang bị sẵn nhiều công cụ để test và debug, đây là một điểm mạnh giúp cho điểm của nó không quá thấp.

- **Riverpod**: 5/5

  Khỏi phải nói, Riverpod thực sự rất mạnh mẽ. Ngay trên trang chủ của nó, chúng ta đã thấy nêu ra rất nhiều trường hợp có thể sử dụng Riverpod để giải quyết rất nhanh gọn.

  Ví dụ, cơ chế xử lí lỗi rất rõ ràng, cơ chế cache được trang bị sẵn, có thể debounce và cancel network requests dễ dàng, tích hợp với Stream cũng rất đơn giản. Ngoài ra, công cụ để test và debug cũng rất mạnh mẽ.

- **Bloc**: 3/5

  Bloc cũng không hề kém khi tích hợp sẵn Stream, cung cấp nhiều operators mạnh mẽ để xử lý dữ liệu. Nó cũng có nhiều widget sẵn để tái sử dụng khi cần trigger một hành động nào đó khi trạng thái thay đổi. Cơ chế xử lí lỗi cũng rất tiện lợi. Ngoài ra, cũng có rất nhiều công cụ để test và debug.

  Tuy nhiên, nó thiếu cơ chế cache và cancel network requests sẵn có như Riverpod. Ngoài ra, Bloc nổi tiếng vì phải viết nhiều boilerplate, mặc dù có thể khắc phục bằng việc sử dụng extension cho VSCode và tạo ra một số base class để tái sử dụng, hoặc sử dụng code generator.

  Đó là một điểm trừ, vì thế, mình đánh giá nó chỉ được 3 điểm ở mục này.

## 5. Tổng Kết.

Sau khi cộng điểm lại, mình có bảng xếp hạng như sau:

- **Provider**: 11/15
- **Riverpod**: 11/15
- **Bloc**: 12/15

Như vậy, với số điểm cao nhất, mình đã chọn **Bloc** cho dự án này. Điều này không có nghĩa là Provider và Riverpod không tốt, mà chỉ đơn giản là với dự án cụ thể này, Bloc phù hợp nhất.

Hi vọng anh em có thể tìm ra thư viện phù hợp nhất với dự án của mình sau khi đọc bài viết này. Good luck!
