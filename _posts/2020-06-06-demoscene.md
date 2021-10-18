---
bố cục: bài
title: Khám phá Demoscene
các thẻ: [Demoscene]
---
Chúng ta không thể bước vào lĩnh vực của những kẻ to lớn ...... <! - more ->

# Lời tựa
Tôi đang nhìn lại lịch sử của chính mình sau khi tôi không có thời gian gần đây. Điều đó thực sự rất buồn ... Tôi đã tìm tòi và nghiên cứu trong khoảng 14 năm. Làm thế nào mà tôi đã trở thành Năm sau một vài năm ...
Trong những tập tin tôi để lại trước đây, tôi tìm thấy một số điều thú vị, chẳng hạn như tập tin 64KiB có tên [.the.product] (http://www.theproduct.de/) rất phổ biến trên Internet vào thời điểm đó. Animation, truyền thuyết kể rằng nó sử dụng một thuật toán nén ngoài trái đất để nén một thứ nhiều GiB thành 64KiB, điều này thực sự đáng kinh ngạc, và có một thứ khác được gọi là [kkrieger] (https://files.scene.org/view/parties / Trò chơi 3D năm 2004 / breakpoint04 / 96kgame / kkrieger-beta.zip) cũng chỉ được sử dụng 92KiB.
Cả hai tác phẩm này đều đến từ một tổ chức có tên là Farbrausch. Có thể tính quảng bá của tổ chức này tốt hơn. Mặc dù các Demoscene khác cũng rất tốt nhưng chúng không được nhiều người biết đến. Chỉ có Demoscene của tổ chức này được lưu hành rộng rãi trên Internet. ...... (nhưng có lẽ vì Breakpoint's Party tương đối nổi tiếng)
Ngoài ra, những người thích chơi xử lý hàng loạt trong thời kỳ Windows XP cũng phải biết một hoạt ảnh có tên [OMNISCENT] (https://files.scene.org/view/parties/1997/mekka97/in4k/snc_omni.zip), Đặt một đống thứ lộn xộn vào chương trình gỡ lỗi và nhận một chương trình nhỏ 4KiB, bạn có thể phát một hoạt hình 3D trông giống như đang ở trong một con tàu vũ trụ.
Tôi không biết những gì được đề cập ở trên là gì vào thời điểm đó. Cho đến bây giờ, tôi biết rằng đó là những thứ được gọi là Demoscene.

# Demoscene là gì
Theo Encyclopedia, Demoscene là một loại tiểu văn hóa nghệ thuật máy tính, và tên tiếng Trung của nó là Yanjing. Những người chơi trò chơi này thích sử dụng máy tính để hiển thị một số video với âm thanh trông tuyệt vời. Bởi vì những hình ảnh này được hiển thị trực tiếp, chúng chiếm rất ít không gian.
Nguyên tắc này cũng giống như ảnh bitmap và đồ họa vector, nhạc sóng và nhạc MIDI, các chương trình luôn chiếm ít dung lượng hơn dữ liệu, vì vậy Demoscene thường sử dụng các chương trình rất nhỏ để thể hiện các cảnh rất phức tạp. Tuy nhiên những ai đã từng làm svg cũng nên biết rằng đối với cùng một hình ảnh thì làm minh họa vector khó hơn làm bitmap, đó là lý do Demoscene là thứ mà chỉ có các ông lớn mới chơi được.
Mặc dù nói cái này là văn hóa máy tính nhưng theo tôi cái này là do nhà toán học viết chương trình làm ra, chuyện này nực cười như NOI một đống bài toán thì phải gọi là cuộc thi tin học ...

# Triển khai Demoscene
Trước đây, Demoscene thường được viết bằng assembly, mặc dù chương trình được viết bằng assembly nên nhỏ, nhưng nó quá phức tạp để sử dụng tốt một số tài nguyên phần cứng.
Bởi vì Demoscene chủ yếu thể hiện âm nhạc và video, những thứ này làm cho card đồ họa hoạt động hiệu quả hơn CPU. Ví dụ như OMNISCENT mà mình đã đề cập ở trên, chương trình này hoàn toàn sử dụng tài nguyên tính toán của CPU, tuy sử dụng 4KiB, nhìn thì có vẻ nhỏ nhưng thực tế nó vẫn rất lãng phí dung lượng. Gần đây, khi tôi truy cập [scene.org] (https://scene.org/), tôi thấy một Demoscene mạnh hơn được gọi là [nâng lên] (https://files.scene.org/view/parties/2009/breakpoint09 /in4k/rgba_tbc_eleised.zip). Nó sử dụng 4KiB để hiển thị mùa xuân, mùa hè, mùa thu và mùa đông của một ngọn núi, hình ảnh không chỉ tinh tế hơn mà thời lượng cũng dài hơn.
Tôi đã xem xét và họ cũng phát hành [mã nguồn] (https://files.scene.org/view/resources/code/sources/rgba_tbc_elended_source.zip), có vẻ như được viết bằng C ++ và lắp ráp, sử dụng DirectX9 của Microsoft để gọi card đồ họa. Nhìn vào các mã khác, nhiều thứ được diễn đạt trong một câu. Ví dụ, mặt trời chỉ cần một câu `" + pow (saturate (mul (e, q [3])), 16) * float3 (.4, .3 , .1) "`, đám mây cũng chỉ có một câu duy nhất `" + .1 * f (s + q [3] .w * .2,10) "`. Bằng cách này, không có gì ngạc nhiên khi toàn bộ chương trình quá nhỏ, một công thức thể hiện một mô hình, và nhóm các nhà toán học giả làm lập trình viên này thực sự đủ mạnh.
Ngoài ra, cũng có một số mô hình 3D phức tạp thường được sử dụng trong toán học để tạo công thức, chẳng hạn như Romanesco Broccoli, và những thứ lộn xộn như Fractal. ) ~~.

# Tái bút
Vì Demoscene có những yêu cầu cao như vậy đối với toán học, nên tự nhiên tôi không thể bước vào nó. Nhưng thật tốt khi đánh giá cao công việc của họ.
Ngoài những bản demo (Giới thiệu) đã đề cập ở trên, tôi cũng tìm thấy một số bản khá thú vị, ví dụ như trong một bữa tiệc tên là Bản sửa đổi năm nay, có một bữa tiệc tên là [SyncCord] (https://files.scene.org/view/ party / 2020 /revision20/pc-4k-intro/synccord_nusanvalden.zip) cũng là 4KiB, và hiệu ứng cũng rất tốt.
Tất nhiên, nó không bị giới hạn ở 4KiB hoặc 64KiB. Những cái lớn hơn có nhiều chỗ hơn cho hiệu suất, chẳng hạn như cái năm 2007 có tên [debris] (https://files.scene.org/view/parties/2007 / breakpoint07 /demo/fr-041_debris.zip) là một bộ phim bom tấn, dung lượng chỉ 177KiB, và nó cũng là tác phẩm của Farbrausch.
Bạn có thể truy cập [scene.org] (https://scene.org/) để tìm thêm các tác phẩm khác.
Ngoài những chương trình này, cũng có một số trang web được viết bởi JS chỉ có 1KiB. Bạn có thể tìm thấy chúng trong [js1k.com] (https://js1k.com/). Tất nhiên, những trang này và những người viết chương trình thực Mọi người không thể so sánh được chút nào, chỉ nghĩ rằng 1KiB JS có thể làm những thứ đó cũng cảm thấy rất thú vị.
