---
bố cục: bài
title: Làm nước sốt cho máy chủ · Phiên bản ứng dụng doanh nghiệp TurboMini
các thẻ: [Máy ​​chủ, PHP, WeChat]
---

Những việc đơn giản nên tự làm <! - more ->

# Nguyên nhân
Vào đầu tháng này, vì nước sốt Máy chủ sắp ngừng hoạt động và phiên bản Turbo cần tiền, tôi đã đặc biệt viết [Nước sốt máy chủ · phiên bản beta TurboMini] (/ 2021/02/02 / serverchan.html). Tuy nhiên, Theo người đã phát triển nước sốt Máy chủ Mọi người nói rằng WeChat sẽ tải xuống các thông báo mẫu thay vì cố tình lừa mọi người làm điều này. Sau một khoảng thời gian, nhà phát triển nói rằng anh ta có thể sử dụng WeChat của công ty hoặc các kênh khác để tiếp tục làm điều đó. Nhân tiện, anh ta đã cấp cho tài khoản thông thường một chút quyền sử dụng phiên bản Turbo và giá có vẻ giảm một chút ?
Nhưng vấn đề là chúng ta chỉ sử dụng Server sauce vì đăng ký tài khoản dịch vụ rất rắc rối, xác thực WeChat yêu cầu main nên chúng ta mới sử dụng. Người sử dụng cái này chắc cũng là nhà phát triển đúng không? Sau đó, tại sao lại sử dụng nước sốt máy chủ của một loạt các quảng cáo nếu tất cả các tài nguyên đều do chúng ta sản xuất? Và muốn sử dụng tốt thì phải tốn tiền, đều là nhà phát triển, không cần phải đóng thuế IQ kiểu này đúng không?
Nhưng thấy rằng nó cũng đã đề xuất một số cách cho chúng ta, thì không cần phải nói quá nhiều về nó.

# Làm thế nào để thực hiện?
Tôi cũng đã xem qua tài liệu phát triển của WeChat doanh nghiệp, tài liệu này tương tự như tài liệu phát triển của tài khoản chính thức, vì vậy hôm nay tôi vẫn giải quyết vấn đề trong một câu:

```php
<?php
$cid='企业ID';
$agentId='应用ID/AgentId';
$secret='应用Secret';
$userid='@all';//用户ID，不知道可以不改
$title='标题';
$content='内容';
file_get_contents('https://qyapi.weixin.qq.com/cgi-bin/message/send?access_token='.json_decode(file_get_contents('https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid='.$cid.'&corpsecret='.$secret),true)['access_token'],false,stream_context_create(['http' => array('method'=>'POST','header'=>"content-type: application/json; charset=UTF-8",'content'=>'{"touser":"'.$userid.'","msgtype":"text","agentid":'.$agentId.',"text":{"content":"'.$title.'\n'.$content.'"}}')]));
```
Ở góc độ trải nghiệm, trải nghiệm phiên bản ứng dụng doanh nghiệp này vẫn tốt, so với số thử nghiệm có thể hiển thị trên trang chủ trước, tuy có cấp phụ nhưng có thể tùy chỉnh biểu tượng và tên hai bên. và số lượng lệnh gọi API cũng được yêu cầu. Nó nhiều hơn số thử nghiệm và nó khá tốt để sử dụng. Nhược điểm duy nhất của việc so sánh với số thử nghiệm là hơi rắc rối khi cấu hình lần đầu tiên .
Ngoài ra, khi viết cái này, tôi thấy API này vẫn khác với API của tài khoản thử nghiệm. Bài đăng của tài khoản thử nghiệm có thể viết thường khi gửi yêu cầu đăng bài, nhưng POST của API WeChat của rác này công ty phải được viết hoa, nếu không nó sẽ là 400. Tôi đã mất nhiều thời gian để gỡ lỗi nó.

# Làm thế nào để cấu hình?
Tương thích với nước sốt Máy chủ và các thông số yêu cầu cũng nhiều như các thông số được yêu cầu bởi nước sốt máy chủ, vì vậy cấu hình cũng hoàn toàn tương thích. Nhưng xét thấy nước sốt máy chủ quá kém đến mức không thể chịu được yêu cầu thậm chí 5kw mỗi tháng, tôi sẽ viết lại phương pháp cấu hình ở đây:
## Bước đầu tiên là đăng ký công ty
Mở [trang web chính thức WeChat của công ty] (https://work.weixin.qq.com/) bằng máy tính để đăng ký công ty
## Bước thứ hai, tạo một ứng dụng
Sau khi đăng ký thành công, bấm vào "Quản lý Doanh nghiệp" để vào giao diện quản lý, chọn "Quản lý Ứng dụng" → "Tự xây dựng" → "Tạo Ứng dụng"
Điền tên ứng dụng tùy thích, chẳng hạn như "Robot của Mayx". Chỉ cần tìm một trong các biểu trưng của ứng dụng. Bạn có thể chọn phạm vi hiển thị của riêng mình. Nếu bạn muốn đẩy nó cho người khác, hãy chọn công ty.
Sau khi tạo xong, vào trang chi tiết ứng dụng, bạn có thể lấy ID ứng dụng (agentid), ứng dụng Secret (bí mật), copy và điền mã.
## Bước thứ ba, lấy ID công ty
Vào trang "[Doanh nghiệp của tôi] (https://work.weixin.qq.com/wework_admin/frame#profile)", kéo xuống dưới cùng, bạn có thể thấy ID công ty, sao chép và điền vào trên cùng.
Nếu bạn không biết cách điền UID đẩy, chỉ cần điền vào `@ all` và đẩy nó cho tất cả nhân viên của công ty.
## Bước thứ tư, đẩy tin nhắn lên WeChat
Đi tới "Doanh nghiệp của tôi" → "[WeChat Plugin] (https://work.weixin.qq.com/wework_admin/frame#profile/wxPlugin)", kéo nó xuống và quét mã QR, và bạn sẽ nhận được thông báo sau khi bạn theo dõi Tin tức.
Bạn có thể thay đổi biểu tượng tại đây nếu bạn không cho rằng nó có vẻ đẹp.

# Những gì có thể được cải thiện
Trước hết, phiên bản hiện tại là để gửi tin nhắn trực tiếp, vì vậy nó không hỗ trợ Markdown, và nó trông rất xấu. Thực ra mình đọc trong tài liệu thấy có thể gửi trực tiếp tin nhắn Markdown nhưng trường hợp này WeChat không nhận được ...
Trên thực tế, phiên bản beta, sau khi đọc tài liệu, tôi nghĩ sẽ rất tuyệt nếu tôi có thể viết nội dung trong thông báo đồ họa, thật không may, thông báo đồ họa yêu cầu hình ảnh tiêu đề, không thể sử dụng được. Phiên bản WeChat Vấn đề tương tự ...
Ngoài ra mình cũng thấy trong tài liệu có thông báo thẻ văn bản rất hay, nhưng có một vấn đề là không hiểu sao lại bắt buộc phải có URL của nó, thì không làm được đâu. ..…
Tất nhiên, nó không khó để làm điều đó, chỉ cần vào [tài liệu chính thức] (https://work.weixin.qq.com/api/doc/90000/90135/90236) cho chính bạn, và nó không quá phức tạp.
Ngoài ra, ngay cả khi thông báo mẫu không thể được sử dụng trong phiên bản beta, nó không phải là không thể đẩy nó. Sử dụng [giao diện xem trước hàng loạt] có tốt không (https://developers.weixin.qq.com/ doc / offiaccount / Message_Management / Batch_Sends_and_Originality_Checks.html)? Mặc dù có giới hạn 100 lần / ngày nhưng không phải là không có, hơn nữa còn có thể giảm áp lực cho máy chủ Nước sốt. Mình nghĩ cái gọi là quyên góp và bảo trì là để kiếm tiền. Vậy quảng cáo nhiều như vậy cũng đủ lâu rồi để trả phí máy chủ. Tôi cũng là máy chủ duy trì trang web, bạn không biết chi phí yêu cầu là bao nhiêu?
Sau đó, hãy xem [Thông báo về tin nhắn mẫu ngoại tuyến] (https://developers.weixin.qq.com/community/develop/doc/000a4e1df800d82acb9b7fb5e5b001) do WeChat gửi, có khả năng cao là tính năng này sẽ không ngoại tuyến, but only the grayscale Nó chỉ là thử nghiệm, nó có thể chỉ thêm các hoạt động khác như ủy quyền.

# Tổng hợp
Trên thực tế, nếu không có Server Sauce với quá nhiều quảng cáo, thu phí dưới danh nghĩa quyên góp và quá nhiều hạn chế, thì nó thực sự sẽ là một sản phẩm tốt, và nó cũng cho tôi động lực để đọc các tài liệu phát triển WeChat , điều đó khiến tôi vẫn còn một chút trong kỳ nghỉ. Làm việc. Chỉ là vì tài khoản dịch vụ quan trọng nhất đã biến mất, nên các nhà phát triển sẽ bỏ nó đi.
