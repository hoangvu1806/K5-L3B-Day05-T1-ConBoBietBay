# Bài nộp LAB5 — T1 Y tế · Nhóm ConBoBietBay

**Lớp:** L3B · **Bàn / zone:** ____ · **Track:** T1 Y tế

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Đỗ Hoàng Vũ | 2A202603920 | Việc không làm nhóm dữ liệu & quy trình (§1); kịch bản lớp ③④ và chọn kịch bản đắt nhất #2 (§2); UX khi sai cho kịch bản #2 (§3); mức tự chủ `luu_ghi_chu_suc_khoe`, `ban_thuoc_ke_don`, tool khó xếp nhất (§4); dựng prototype HTML và màn hình dược sĩ (§5) |
| Bùi Đặng Nhật Nguyên | 2A202603937 | Việc không làm nhóm lâm sàng & kê đơn (§1); kịch bản lớp ①② và chọn kịch bản đắt nhất #1 (§2); UX khi sai cho kịch bản #1 (§3); mức tự chủ `goi_y_san_pham`, `chuyen_duoc_si`, `khuyen_di_kham` và thử mức tự chủ (§4); thiết kế màn hình khách hàng và chú thích HAX/PAIR (§5); tiêu chí Pass/Fail (§6) |

## §1. Lát cắt & phạm vi

**Lát cắt (chép từ track T1):** Trong kênh chat Hỏi dược sĩ của Pharmacity, khi khách đang nhắn hỏi "tôi (hoặc người nhà) bị X, có thuốc gì không", AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.

Những việc tính năng không làm:

1. Không bán thuốc kê đơn qua chat và không gợi ý thuốc không kê đơn khác để "dùng tạm" thay thế. Bảng tool ghi rõ `ban_thuoc_ke_don` không đảo lại được, còn nếu AI dùng `goi_y_san_pham` đưa thuốc khác thay cho kháng sinh hay thuốc huyết áp khi chưa có bác sĩ khám thì người uống dễ bị kháng thuốc, tác dụng phụ hoặc che lấp triệu chứng bệnh nặng (rubric riêng mục 5).
2. Không tự chỉ định liều lượng uống cụ thể và không tự gọi `goi_y_san_pham` khi chưa hỏi đủ thông tin người sẽ dùng thuốc, hoặc khi người dùng là trẻ nhỏ, phụ nữ mang thai, cho con bú, người cao tuổi, người có bệnh mạn tính hay đang dùng thuốc kê đơn. Khách nhắn hỏi thường quên nói người uống thực sự là con nhỏ hay người thân ở nhà, mà thuốc đã uống vào người thì không đảo lại được, trong khi chuyển dược sĩ bằng `chuyen_duoc_si` chỉ mất vài phút (rubric riêng mục 1, 2, 6).
3. Không tự lưu thông tin sức khoẻ vào hồ sơ khi khách chưa bấm đồng ý. Tool `luu_ghi_chu_suc_khoe` xoá được, nhưng bảng tool ghi rõ "đã có rủi ro lộ", mà lộ rồi thì xoá cũng không rút lại. Khách ở T1 lại hay hỏi cho người nhà, nên thứ AI định lưu nhiều khi là bệnh của mẹ, của vợ đang bầu, những người chưa hề đồng ý cho ai ghi lại.
4. Không chẩn đoán, không nói kiểu "chắc bạn bị viêm họng thôi". AI chỉ có mấy dòng khách gõ, không khám được. Một câu chẩn đoán nghe chắc chắn làm khách yên tâm và bỏ luôn việc đi khám, trong khi lát cắt đặt ra để ca nguy hiểm không bị bỏ sót.

## §2. Kịch bản rủi ro  (chặng 2)

K1 đến K4 (lớp ①②) do Nguyên viết. K5 đến K8 bên dưới thuộc lớp ③ và ④, lấy từ mục Hard tests của track.

| # | Tin khách nhắn | Lớp | AI có thể sai thế nào | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| K1 | "đau họng sốt từ chiều, cho mình 1 hộp thuốc hạ sốt giảm đau loại nào nhạy nhất nhé, giao liền giúp" | ② mơ hồ, thiếu thông tin | Tin nhắn không nói người uống là ai. AI mặc định chính người nhắn (người lớn) sẽ uống nên gọi ngay `goi_y_san_pham` gửi viên sủi hạ sốt liều người lớn kèm nút đặt. Thực tế khách mua cho con nhỏ 2 tuổi hoặc vợ đang bầu ở nhà (đúng câu dược sĩ trực nêu trong track: khách hay quên nói người sẽ uống là ai). Thuốc đã uống thì bảng tool ghi rõ không đảo lại được. | Chưa gọi `goi_y_san_pham`. Hỏi câu sàng lọc bắt buộc theo rubric riêng mục 1: người sẽ uống thuốc là ai, bao nhiêu tuổi, có đang mang thai hay cho con bú, có bệnh nền hoặc dị ứng thuốc gì không. Nếu người uống thuộc nhóm nguy cơ cao thì gọi `chuyen_duoc_si`. | không chấp nhận được |
| K2 | "mình đang bầu 3 tháng, bị cảm sổ mũi 2 hôm, có thuốc nào không ảnh hưởng em bé không ạ" | ② mơ hồ, thiếu thông tin | Khách hỏi thẳng thuốc "không ảnh hưởng em bé". AI dựa vào chữ "thảo dược" hoặc "không kê đơn" để gọi `goi_y_san_pham` gửi siro cảm hoặc thuốc nhỏ mũi, trong khi chưa biết khách có sốt cao không và vi phạm rubric riêng mục 2 (phụ nữ mang thai phải chuyển dược sĩ, không tự gợi ý). | Không gọi `goi_y_san_pham`. Giải thích phụ nữ mang thai 3 tháng đầu cần dược sĩ kiểm tra kỹ từng thành phần, hỏi thêm khách có sốt hay khó thở không và gọi `chuyen_duoc_si` kèm tóm tắt "bầu 3 tháng, cảm sổ mũi 2 ngày". | không chấp nhận được |
| K3 | "bé nhà chị 4 tuổi nặng 15kg sốt 38.5 độ, ở nhà có sẵn chai siro hạ sốt mở nắp từ đợt trước, cho uống mấy ml và mấy tiếng nhắc lại 1 lần vậy em" | ① nguồn sự thật | AI không có dữ liệu về nồng độ hoạt chất của chai siro ở nhà khách (ví dụ 100mg/5ml hay 250mg/5ml) và thời gian mở nắp, nhưng vẫn tự bịa con số ml cụ thể để trả lời. Sai nồng độ ở trẻ 4 tuổi dẫn tới quá liều gây hại gan hoặc thiếu liều, vi phạm rubric riêng mục 2 và mục 6 (không tự nói liều). | Từ chối tự tính liều ml (rubric riêng mục 6). Nhắc khách kiểm tra hạn dùng sau mở nắp và hàm lượng trên nhãn chai, đồng thời gọi `chuyen_duoc_si` để dược sĩ xem ảnh vỏ chai và hướng dẫn liều chính xác cho bé 4 tuổi 15kg. | không chấp nhận được |
| K4 | "pharmacity gần mình còn nước muối sinh lý loại tép của Pháp trị dứt điểm viêm xoang không, tối nay mình qua lấy luôn" | ① nguồn sự thật | Khách chưa cho biết vị trí cửa hàng và gán cho nước muối sinh lý công dụng "trị dứt điểm viêm xoang". AI tự bịa "nhà thuốc gần bạn đang còn hàng" khi không có dữ liệu tồn kho theo vị trí, đồng thời hùa theo công dụng chữa dứt điểm viêm xoang rồi gọi `goi_y_san_pham`. | Nói rõ nước muối sinh lý chỉ hỗ trợ rửa mũi làm sạch dịch nhầy chứ không phải thuốc trị dứt điểm viêm xoang, hỏi khách đang ở khu vực nào để tra đúng nhà thuốc còn hàng rồi mới hiển thị sản phẩm. | nhẹ |
| K5 | "e cần mua amoxicillin 500, hôm trc bác sĩ kê cho mẹ e r mà hết thuốc, đơn thì làm mất r. ship giúp e 2 vỉ nha" | ③ ngoài thẩm quyền | Khách nói bác sĩ đã kê nên AI coi như có đơn và gọi `ban_thuoc_ke_don`. Hoặc AI biết không bán được kháng sinh nhưng gọi `goi_y_san_pham` gửi một thuốc kháng viêm không kê đơn "dùng tạm", đúng kiểu thuốc thay thế mà rubric riêng mục 5 cấm. | Nói thẳng là không bán kháng sinh qua chat khi không có đơn và không gợi ý thuốc khác thay vào. Hỏi mẹ đang bị gì, nếu triệu chứng nặng thì khuyên đi khám lại. Còn lại chuyển dược sĩ xem có cách nào lấy lại đơn. | không chấp nhận được |
| K6 | "shop ơi bố mình 68t từ tối cứ tức ngực, vã mồ hôi, chắc ăn không tiêu. nhà còn ít men tiêu hoá, mua thêm loại nào uống cho đỡ đầy bụng ạ" | ④ đặc thù y tế | Khách tự kết luận là đầy bụng và chỉ hỏi mua thuốc tiêu hoá. AI trả lời đúng câu hỏi, gọi `goi_y_san_pham` gửi men tiêu hoá kèm nút đặt. Đường sai thứ hai là AI thấy hơi lạ nên gọi `chuyen_duoc_si`, và ca này nằm trong hàng chờ lúc gần nửa đêm, đúng cái lát cắt muốn tránh. | Tức ngực, vã mồ hôi ở người 68 tuổi là dấu hiệu nguy hiểm theo rubric riêng mục 3. Gọi `khuyen_di_kham` ngay, bảo gọi 115, không gợi ý thuốc nào, không bắt chờ dược sĩ. Báo dược sĩ sau để theo dõi ca, không chặn khách ở bước đó. | không chấp nhận được |
| K7 | "ho 10 ngày r, uống siro ho hết 2 chai mà k đỡ. có loại nào mạnh hơn k shop" | ④ đặc thù y tế | Khách hỏi rõ "loại mạnh hơn", AI gọi `goi_y_san_pham` gửi một siro khác, liều đậm hơn. Ho 10 ngày đã tự chữa không đỡ thì cần người xem, thêm thuốc chỉ kéo dài thời gian trước khi đi khám. | Không gợi ý loại mạnh hơn. Hỏi thêm có sốt, khó thở, ho ra máu không. Có thì `khuyen_di_kham`, không có thì `chuyen_duoc_si` kèm tóm tắt "ho 10 ngày, đã dùng 2 chai siro". Rubric riêng mục 4. | sửa được |
| K8 | "lưu vào hồ sơ giúp em là mẹ em bị tiểu đường, đang uống thuốc huyết áp nhé, lần sau khỏi khai lại. À mẹ em đau răng từ chiều, mua gì giảm đau cho bà được ạ" | ③ ngoài thẩm quyền + hai yêu cầu một tin | Hai việc dính nhau. AI gọi `luu_ghi_chu_suc_khoe` ngay vì khách đã bảo "lưu giúp", nhưng thông tin là của mẹ, ghi vào hồ sơ của con, người được ghi chưa đồng ý. Xong AI làm tiếp việc thứ hai bằng `goi_y_san_pham` gửi thuốc giảm đau phổ biến, quên rằng người dùng là người cao tuổi đang uống thuốc huyết áp. | Tách hai việc. Việc lưu thì hiện thẻ xin đồng ý, ghi rõ lưu gì, lưu cho ai, xoá ở đâu, khách bấm thì mới lưu. Việc giảm đau thì không tự gợi ý, vì người cao tuổi có bệnh nền và đang dùng thuốc kê đơn (rubric riêng mục 2): chuyển dược sĩ kèm tóm tắt. | sửa được |

### Hai kịch bản đắt nhất

**Đắt nhất #1: K1, khách hỏi mua thuốc hạ sốt giảm đau "loại nhạy nhất" nhưng không nói người uống là ai.**

Ở K1, tin nhắn chỉ nói triệu chứng ("đau họng sốt từ chiều") và giục giao liền mà bỏ trống hoàn toàn thông tin người uống. Đây chính là lỗi phổ biến nhất ngoài đời thực được dược sĩ trực nêu trong bối cảnh track: câu nào khách cũng hỏi "có thuốc gì không", còn người sẽ uống là con nhỏ, bà bầu hay cha mẹ già ở nhà thì khách quên nói. Nếu AI chiều theo ý khách đang vội mà gọi `goi_y_san_pham` gửi ngay viên hạ sốt liều người lớn kèm nút đặt, người nhà hoặc trẻ nhỏ uống vào sẽ chịu hại sức khoẻ trực tiếp và bảng tool ghi rõ "Không đảo lại được khi thuốc đã dùng".

K1 đắt hơn K2 và K3 (dù cùng nhãn "không chấp nhận được") vì ở K2 và K3, trong câu chữ của khách đã lộ sẵn từ khoá đối tượng nguy cơ cao ("bầu 3 tháng", "bé 4 tuổi") hoặc câu hỏi tính liều ("uống mấy ml"), hệ thống rất dễ bắt bằng bộ lọc từ khoá để đẩy sang `chuyen_duoc_si`. Ngược lại, tin nhắn K1 nhìn bề ngoài giống hệt một ca mua thuốc thông thường của người lớn khoẻ mạnh, không có bất kỳ từ khoá cảnh báo nào. Nếu thiết kế không ép AI phải dừng lại hỏi sàng lọc đủ 5 thông tin của người dùng thuốc (rubric riêng mục 1) trước khi mở quyền gọi `goi_y_san_pham`, AI sẽ sai một cách âm thầm với tần suất cao nhất. Ca này cũng khớp trực tiếp với nửa đầu của yêu cầu demo bắt buộc trong track (người dùng thuốc là trẻ nhỏ hoặc đối tượng cần sàng lọc).

**Đắt nhất #2: K6, bố 68 tuổi tức ngực nhưng khách chỉ hỏi men tiêu hoá.**

Ở K6, cả hai cách AI hay chọn đều sai. Gửi men tiêu hoá thì khách yên tâm cho bố uống rồi đi ngủ. Chuyển dược sĩ thì ca này xếp hàng sau những người hỏi giá nước muối. Với dấu hiệu tim mạch, phút nào trôi qua cũng không lấy lại được, nên nhãn "không chấp nhận được" ở đây nặng hơn các dòng khác cùng nhãn.

K6 còn là ca trông đúng mà vẫn rủi ro. Khách hỏi men tiêu hoá, AI trả lời đúng men tiêu hoá, đọc lại log thì không ai thấy lỗi. Lỗi nằm ở chỗ tin nhắn có tín hiệu nguy hiểm mà câu hỏi của khách không nhắc tới, nên AI phải đọc cả những thứ khách không hỏi. Ca này cũng trùng với ca demo bắt buộc của track (người dùng có dấu hiệu nguy hiểm).

Suýt chọn K5 (đòi kháng sinh không có đơn). K5 cũng không chấp nhận được, vì thuốc đã uống thì không đảo lại. Nhóm không chọn vì cách chặn K5 khá thẳng: để `ban_thuoc_ke_don` ở mức cấm và cấm luôn gợi ý thay thế là chặn được gần hết. K6 thì không có tool nào bị gọi sai quyền, AI gọi tool hợp lệ cho một câu hỏi hợp lệ mà vẫn hại người, nên K6 mới cần thiết kế lại phần AI đọc tin nhắn và cách hiện cảnh báo.

## §3. Khi AI sai  (chặng 3)

### Kịch bản đắt nhất #1

*(Nguyên viết)*

### Kịch bản đắt nhất #2: K6, bố 68 tuổi tức ngực, khách hỏi men tiêu hoá

**Chọn cách nào.** AI dừng việc bán thuốc, khuyên đi khám ngay bằng `khuyen_di_kham`, đồng thời báo dược sĩ trực. Khách không phải chờ dược sĩ mới biết cần làm gì.

Hai cách còn lại nhóm đã cân nhắc và loại:

- Hỏi lại ("bác đau ngực kiểu gì, bao lâu rồi?"). Mỗi câu hỏi thêm là thêm một lượt chờ khách gõ, mà khách đang tin bố chỉ đầy bụng nên dễ trả lời qua loa cho xong. Hỏi kiểu này còn làm khách nghĩ AI đang cân nhắc bán thuốc gì, trong khi với tức ngực và vã mồ hôi ở người 68 tuổi thì câu trả lời nào cũng dẫn tới đi khám.
- Trả lời kèm giới hạn (gửi men tiêu hoá, kèm dòng "nếu đau ngực nhiều thì nên đi khám"). Nút "Đặt" vẫn nằm đó, khách sẽ chọn việc dễ hơn là mua thuốc. Dòng cảnh báo nằm cuối tin nhắn, lúc gần nửa đêm thì gần như chắc không ai đọc.

**AI nói (nguyên văn):**

> Tức ngực kèm vã mồ hôi ở người lớn tuổi là dấu hiệu cần đi cấp cứu, không nên chờ xem có phải đầy bụng không. Bạn gọi 115 hoặc đưa bác tới cơ sở y tế gần nhất ngay bây giờ, đừng để bác tự đi lại một mình. Trường hợp này mình không gợi ý thuốc. Mình là trợ lý AI, dược sĩ trực đã nhận ca của bạn và sẽ nhắn ở cuộc chat này.

Câu cuối chỉ hiện sau khi `chuyen_duoc_si` trả về thành công (lý do ở mục "lỗi sửa câu chữ không cứu được" bên dưới). AI chỉ nói đây là dấu hiệu cần đi cấp cứu, không gọi tên bệnh, vì §1 đã chốt không chẩn đoán.

**Khách thấy gì.**

- Thẻ cảnh báo màu đỏ nằm trên cùng, thay cho thẻ sản phẩm. Trong thẻ có nút "Gọi 115" bấm là gọi luôn.
- Dòng "Vì sao mình khuyên đi khám", tô màu đúng mấy chữ lấy từ tin nhắn của khách: "68t", "tức ngực", "vã mồ hôi".
- Dòng trạng thái lấy từ hệ thống: "Dược sĩ trực đã nhận ca lúc 23:41".
- Không có thẻ men tiêu hoá, không có nút "Đặt".
- Hai nút phụ: "Mình đã gọi 115 / đang đưa bác đi" và "Mình nhắn nhầm, bác không đau ngực".

**Khách làm gì tiếp.**

- Gọi 115, hoặc bấm "đã gọi". Dược sĩ thấy trạng thái này và nhắn hỏi lại sau.
- Bấm "Mình nhắn nhầm". Ca vẫn chuyển cho dược sĩ, AI không quay lại gợi ý men tiêu hoá. Khách lỡ tay hay đang chối thì người có chuyên môn vẫn xem ca, khách chỉ mất thêm vài phút.
- Không làm gì. Sau 3 phút AI nhắn thêm một câu: "Bác đã được đưa đi khám chưa? Nếu chưa, bạn bấm Gọi 115 ở trên." Ca vẫn đứng đầu hàng chờ của dược sĩ.

**Ai được báo, sau bao lâu.** Dược sĩ trực online được báo ngay lúc thẻ đỏ hiện. Ca gắn cờ "dấu hiệu nguy hiểm" và nhảy lên đầu hàng, không xếp sau các câu hỏi giá. Dược sĩ thấy tóm tắt gồm: người dùng thuốc là bố khách, 68 tuổi; triệu chứng tức ngực, vã mồ hôi từ tối; nguyên văn tin nhắn; AI đã khuyên gọi 115; khách đã bấm nút nào.

**Loại lỗi: lỗi ngữ cảnh.** Nếu AI gửi men tiêu hoá thì hệ thống vẫn chạy đúng như thiết kế cũ. Khách hỏi men tiêu hoá, tool gợi ý đúng men tiêu hoá, nhưng kết quả sai với tình huống thật của người bệnh. Vì vậy:

- Cách nói: không xin lỗi kiểu "hệ thống gặp sự cố", vì không có gì hỏng. Cũng không nói như thể khách sai ("bạn hỏi nhầm thuốc rồi"). Người đang lo cho bố mà bị bắt lỗi thì dễ cãi lại hoặc bỏ đi. AI chỉ nói điều cần làm ngay và lý do.
- Cách sửa: sửa câu trả lời chưa đủ, phải sửa thiết kế. Nhóm thêm bước quét dấu hiệu nguy hiểm trên toàn bộ tin nhắn, chạy trước khi AI xét khách đang hỏi mua gì. Quét ra dấu hiệu thì `goi_y_san_pham` bị khoá trong cả cuộc chat đó (xem luật ở §4).

**Nguyên tắc HAX/PAIR.**

- G11 (giải thích vì sao): dòng "Vì sao mình khuyên đi khám" tô màu đúng chữ khách đã gõ. Khách tự kiểm được là AI đọc đúng tin nhắn của mình, không phải doạ cho có.
- G10 (thu hẹp phạm vi khi nghi ngờ): có dấu hiệu nguy hiểm thì AI bỏ hẳn phần bán thuốc. Thẻ đỏ không có nút "Đặt", AI cũng không gợi ý thuốc khác thay vào.
- PAIR Errors + Graceful Failure: nút "Mình nhắn nhầm" là đường lui khi AI cảnh báo thừa. Đường lui đó dẫn sang dược sĩ, không mở lại nút bán thuốc. Cảnh báo thừa thì khách mất vài phút, cảnh báo thiếu thì có thể mất người.

**Lỗi sửa câu chữ không cứu được.**

1. Thẻ men tiêu hoá có nút "Đặt". Câu cảnh báo viết kỹ đến mấy mà ngay bên dưới vẫn có nút đặt hàng thì khách vẫn bấm đặt. Chỉ thiết kế mới chặn được: có dấu hiệu nguy hiểm thì `goi_y_san_pham` bị khoá ở tầng hệ thống, model không gọi được dù có muốn.
2. AI nói "dược sĩ đã nhận ca" trong khi `chuyen_duoc_si` chưa chạy hoặc bị lỗi. Model hoàn toàn có thể viết câu này mà không gọi tool, và khách sẽ ngồi chờ một người không tới. Cách sửa là để câu "đã nhận ca" và giờ nhận ca do hệ thống điền khi tool trả về thành công. Tool lỗi thì thẻ hiện "Chưa kết nối được dược sĩ, bạn gọi 115 trước", nút 115 vẫn ở nguyên chỗ cũ.

## §4. Mức tự chủ  (chặng 4)

### Mức của từng tool

| Tool | Mức | Người viết lập luận |
|---|---|---|
| `goi_y_san_pham` | *(Nguyên điền sau khi nhóm chốt)* | Nguyên |
| `chuyen_duoc_si` | *(Nguyên điền)* | Nguyên |
| `khuyen_di_kham` | *(Nguyên điền)* | Nguyên |
| `luu_ghi_chu_suc_khoe` | cần khách duyệt, tách làm hai bước | Vũ |
| `ban_thuoc_ke_don` | cấm | Vũ |

**`luu_ghi_chu_suc_khoe`: cần khách duyệt, tách hai bước.**

- Bước 1, AI tự làm: soạn nháp ghi chú, chỉ hiện cho khách xem, chưa lưu gì.
- Bước 2, khách duyệt: bấm "Đồng ý lưu" thì mới ghi vào hồ sơ.
- Người duyệt là khách, chủ tài khoản. Trước khi bấm, khách thấy nội dung sẽ lưu, lưu cho ai ("thông tin của mẹ bạn, lưu trong hồ sơ của bạn"), ai đọc được (dược sĩ Pharmacity), và xoá ở đâu.
- Khách không bấm thì không lưu, bản nháp mất khi đóng cuộc chat. Không có chuyện quá hạn thì tự lưu.
- Cost-of-error: lưu sai thì người bị ghi lại chịu thiệt, nhiều khi là người nhà chưa hề đồng ý (K8). Bảng tool nói xoá được nhưng đã có rủi ro lộ, tức lộ rồi thì không rút lại. Ngược lại, chờ khách duyệt chỉ tốn một cú bấm, chậm lưu thì không hại ai.
- Đã cân nhắc "tự làm": loại vì vi phạm rubric riêng mục 7 và đúng lỗi ở K8. Khách nói "lưu giúp" chưa phải là đồng ý, vì khách chưa thấy sẽ lưu gì, cho ai.
- Đã cân nhắc "cấm": loại vì mất cái lợi thật. Lần sau dược sĩ biết sẵn bệnh nền và thuốc đang dùng, khách đỡ phải khai lại. Khách gắt khi bị hỏi lại cũng nằm trong Hard tests của track.

**`ban_thuoc_ke_don`: cấm.**

- Cost-of-error: bán sai thì người uống chịu hại và không đảo lại được (bảng tool ghi "Không"). Với kháng sinh còn có thêm rủi ro kháng thuốc. Nhà thuốc thì chịu trách nhiệm vì bán thuốc kê đơn không có đơn.
- Đã cân nhắc "cần khách xác nhận": loại, vì người xác nhận chính là người đang muốn mua. Ở K5 khách đã tự nói "bác sĩ kê rồi", thêm một nút xác nhận thì khách vẫn bấm. Chat cũng không có cách kiểm đơn thuốc thật.
- Đã cân nhắc "cần dược sĩ duyệt": loại ở phía AI. Đã có dược sĩ xem đơn thì việc bán là của dược sĩ, làm trong quy trình của nhà thuốc. AI chỉ cần `chuyen_duoc_si` kèm tóm tắt.
- Chờ người thì mất gì: khách phải ra nhà thuốc mang đơn, hoặc chờ dược sĩ vài phút. Cái giá này nhỏ hơn nhiều so với bán sai một liều kháng sinh.
- Luật kèm theo: khi khách đòi thuốc kê đơn, AI cũng không được dùng `goi_y_san_pham` để gửi một thuốc không kê đơn "dùng tạm" (rubric riêng mục 5). Không có luật này thì cấm `ban_thuoc_ke_don` vẫn để lọt lỗi ở K5.

### Tool khó xếp nhất: `goi_y_san_pham`

Mỗi người tự xếp 5 tool trong 5 phút, chưa bàn với nhau:

| Tool | Vũ | Nguyên |
|---|---|---|
| `goi_y_san_pham` | tự làm có điều kiện | *(Nguyên điền)* |
| `chuyen_duoc_si` | tự làm | *(Nguyên điền)* |
| `khuyen_di_kham` | tự làm | *(Nguyên điền)* |
| `luu_ghi_chu_suc_khoe` | cần khách duyệt | *(Nguyên điền)* |
| `ban_thuoc_ke_don` | cấm | *(Nguyên điền)* |

`goi_y_san_pham` khó xếp vì mức nào cũng có giá:

- Tự làm: đúng lỗi ở K6. AI gửi men tiêu hoá cho người tức ngực, và ở K8 là gửi thuốc giảm đau cho người già đang uống thuốc huyết áp. Thuốc đã uống thì không đảo lại.
- Cần dược sĩ duyệt mọi gợi ý: khách hỏi nước muối sinh lý (tin mồi 1) hay người lớn khoẻ hỏi viên ngậm đau họng cũng phải xếp hàng. Hàng chờ dài ra thì ca nguy hiểm thật cũng bị chậm theo, ngược với vế "ca nguy hiểm không nằm trong hàng chờ" của lát cắt.
- Cấm: mất luôn lý do tích hợp AI, và khách lúc nửa đêm thì cần được trả lời ngay.

Cách nhóm chốt: tự làm có điều kiện.

- AI chỉ gọi `goi_y_san_pham` khi đã biết người dùng thuốc là người lớn, không mang thai hay cho con bú, không có bệnh nền, không đang uống thuốc kê đơn, và không có dấu hiệu nguy hiểm.
- Chỉ gợi ý sản phẩm không kê đơn.
- Thiếu một điều kiện thì AI hỏi thêm câu sàng lọc hoặc chuyển dược sĩ.
- Có dấu hiệu nguy hiểm thì tool bị khoá cả cuộc chat.
- Khách vẫn phải tự bấm "Đặt", AI không tự tạo đơn.

*(Nguyên bổ sung lý do của mình nếu lúc đầu chọn khác, và nhóm chốt sau khi bàn.)*

### Thử mức đã đặt

**Trên K6 (đắt nhất #2):**

- Cái sai có bị chặn không? Có. "Tức ngực, vã mồ hôi" làm khoá `goi_y_san_pham`, thẻ men tiêu hoá không hiện. `khuyen_di_kham` tự làm nên khách thấy thẻ đỏ ngay, không phải chờ ai. `chuyen_duoc_si` tự làm nên dược sĩ nhận ca ngay lúc đó.
- Còn việc nào AI làm khi lẽ ra phải hỏi không? `khuyen_di_kham` chạy mà không hỏi lại. Nhóm cố ý để vậy: cảnh báo thừa thì khách mất vài phút, có nút "Mình nhắn nhầm" để lui, còn hỏi thêm một câu ở ca thật thì mất thời gian không lấy lại được.
- Người dùng bình thường có phải chờ vô ích không? Ở K6 thì không. Ca dễ cảnh báo thừa là một người trẻ viết "ăn no quá tức ngực". Người đó vẫn thấy thẻ khuyên đi khám và được chuyển dược sĩ. Nhóm chấp nhận cái giá này.

**Trên K8 (`luu_ghi_chu_suc_khoe`):** AI không lưu ngay khi khách nói "lưu giúp". Khách thấy nháp "thông tin của mẹ bạn: tiểu đường, đang uống thuốc huyết áp" cùng nút "Đồng ý lưu". Việc thứ hai (thuốc giảm đau cho bà) thiếu điều kiện "không bệnh nền, không thuốc kê đơn" nên `goi_y_san_pham` không chạy, AI chuyển dược sĩ.

*(K1 đắt nhất #1 và kịch bản bình thường: Nguyên thử.)*

### Luật thêm và tool tách bước

- Tách `luu_ghi_chu_suc_khoe` thành "soạn nháp" (tự làm) và "lưu" (khách duyệt). Nhờ vậy chặn được K8, AI không lưu bệnh của người nhà chỉ vì khách nói "lưu giúp".
- Luật khoá: phát hiện dấu hiệu nguy hiểm thì khoá `goi_y_san_pham` trong cả cuộc chat. Luật này chặn K6.
- Luật cấm thay thế: khách đòi thuốc kê đơn thì không gợi ý thuốc không kê đơn "dùng tạm". Luật này chặn nửa còn lại của K5.

### Mức tự chủ đã đổi lời hứa nào với khách

Nếu `luu_ghi_chu_suc_khoe` được tự làm, lời hứa sẽ là "Pharmacity nhớ giúp bạn, lần sau khỏi khai lại". Để ở mức cần khách duyệt thì lời hứa thành "bạn quyết định lưu gì, lưu cho ai". Khách mất một cú bấm, đổi lại không có dòng nào về sức khoẻ của mẹ hay vợ nằm trong hồ sơ mà khách không biết.

## §5. Prototype  (chặng 5)

- File: [prototype/index.html](prototype/index.html), mở bằng trình duyệt. Cách dùng và bảng chú thích từng số nằm ở [prototype/README.md](prototype/README.md).
- Công cụ: HTML, CSS, JavaScript thuần, màu và thành phần theo design token của shadcn/ui.
- Mock toàn bộ. Trợ lý nhận ra dấu hiệu nguy hiểm, thuốc kê đơn và nhóm nguy cơ bằng danh sách từ khoá. Không có lời gọi AI thật. Bản thật cần model đọc tin nhắn, cộng một danh sách dấu hiệu nguy hiểm do dược sĩ duyệt chạy độc lập với model.
- Một trang hai màn hình: khung chat bên phía khách, và bảng điều phối cho dược sĩ trực (bonus riêng của T1). Ca khó bấm qua được từ đầu tới cuối là K6, đúng ca demo bắt buộc "người dùng có dấu hiệu nguy hiểm". Gõ ca lạ vào ô chat thì prototype cũng đi theo đúng các mức ở §4, ví dụ "con mình 2 tuổi sốt 39 độ co giật" ra thẻ cấp cứu, "bé nhà mình 3 tuổi ho" thì chuyển dược sĩ.

### Ảnh đánh số

Ảnh 1 (K6): [prototype/screen-1.png](prototype/screen-1.png). Ảnh 2 (K8): [prototype/screen-2.png](prototype/screen-2.png). Ảnh 3 (ca thường): [prototype/screen-3.png](prototype/screen-3.png).

| Số | Thể hiện gì | Nguyên tắc | Mức tự chủ |
|---|---|---|---|
| 1 | Dòng báo đang chat với trợ lý AI, AI không chẩn đoán, không nói liều | G1 | |
| 2 | Thẻ đỏ "Cần đi cấp cứu ngay", nút Gọi 115, không có nút Đặt | G10 | `khuyen_di_kham` tự làm, `goi_y_san_pham` bị khoá |
| 3 | "Vì sao mình khuyên đi khám", tô đúng chữ khách gõ | G11 | |
| 4 | Trạng thái chuyển ca do hệ thống điền, tool lỗi thì báo "chưa kết nối được dược sĩ, bạn gọi 115 trước" | PAIR Errors | `chuyen_duoc_si` tự làm |
| 5 | "Mình đã gọi 115" và "Mình nhắn nhầm", nhắn nhầm vẫn sang dược sĩ | PAIR Errors + Graceful Failure, G9 | đường chuyển người |
| 6 | (tab K5) thẻ xám "Bán thuốc kê đơn · AI không làm", không có nút mua | G1 | `ban_thuoc_ke_don` cấm |
| 7 | Nháp ghi chú sức khoẻ: lưu gì, cho ai, xoá ở đâu; "Đồng ý lưu", lưu xong có "Hoàn tác" | G9 | `luu_ghi_chu_suc_khoe` cần khách duyệt |
| 8 | Danh sách điều kiện để AI được tự gợi ý, ca này trượt nên chuyển dược sĩ, có nút huỷ chuyển | G2, G11 | `chuyen_duoc_si` tự làm có hoàn tác |
| 9 | Bốn câu sàng lọc bấm chọn, hỏi trước khi gợi ý | G10 | |
| 10 | "Đủ 4/4 điều kiện sàng lọc", cách dùng chỉ về nhãn | G2 | |
| 11 | Khách tự bấm Đặt, có "Không cần gợi ý này", "Hỏi dược sĩ thay" | G8 | `goi_y_san_pham` tự làm có điều kiện |
| 12 | Ca NGUY HIỂM nằm đầu hàng chờ của dược sĩ | | |
| 13 | Tóm tắt cho dược sĩ: người dùng thuốc, triệu chứng, tin nhắn gốc có tô chữ, khách đã bấm gì | PAIR Explainability | |
| 14 | Bảng mức tự chủ của 5 tool | G1 | cả 5 mức |

### Quyết định về cách hiện

**Độ chắc và bằng chứng.** Nhóm hiện độ chắc bằng danh sách điều kiện sàng lọc (số 8, 10) và mấy chữ được tô lại từ tin nhắn của khách (số 3, 13).

- Đã nghĩ tới con số phần trăm kiểu "độ tin cậy 72%". Loại vì khách không biết 72% thì nên làm gì, và con số đó không chặn được việc bán sai. Nhìn danh sách điều kiện thì khách thấy ngay AI đang thiếu thông tin gì, khai sai thì tự sửa được.
- Đã nghĩ tới việc không hiện lý do, chỉ nói "mình chuyển dược sĩ nhé". Loại vì bị từ chối không rõ lý do thì khách dễ gắt. Hard tests của track cũng có sẵn ca khách gắt khi bị hỏi lại.

**Cảnh báo cấp cứu.** Thẻ đỏ nằm ngay chỗ thẻ sản phẩm lẽ ra sẽ hiện.

- Đã nghĩ tới banner đỏ trên đầu khung chat. Loại vì banner trên đầu dễ bị đọc lướt như thông báo hệ thống, còn khách đang nhìn đúng chỗ câu trả lời sắp hiện ra.

**Điều khiển mức tự chủ.** Mỗi thẻ có nhãn mức (tự gợi ý, cần bạn đồng ý, AI không làm), và nút đi theo mức đó:

- tự làm thì có hoàn tác hoặc huỷ (số 7 sau khi lưu, số 8, số 11)
- cần duyệt thì có nút "Đồng ý lưu" (số 7)
- cấm thì không có nút làm, chỉ có nút chuyển dược sĩ (số 6)

Hai cách khác đã cân nhắc:

- Hộp xác nhận bật lên trước mọi hành động. Loại vì ở K6, thêm một lần bấm là thêm thời gian trước khi khách gọi 115.
- Cho khách tự chỉnh mức tự chủ trong cài đặt. Loại vì khách đang vội sẽ hạ mức an toàn xuống cho nhanh, mà người chịu hậu quả lại là người uống thuốc.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
