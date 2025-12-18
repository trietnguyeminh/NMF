# NMF
first prj
Tiêu đề: Phân tích Ma trận Không âm & Ứng dụng 1Slide 1, 2, 3 2
 
Nội dung trình bày của nhóm sẽ bao gồm 4 phần chính6:
Vấn đề của PCA và Giải pháp NMF
Các dữ liệu thực tế như hình ảnh (pixel) hay văn bản (tần suất từ) đều được biểu diễn dưới dạng ma trận không âm11. Để giảm chiều dữ liệu này, cách tiếp cận kinh điển là sử dụng SVD hoặc PCA12.
Tuy nhiên, các phương pháp này tồn tại một hạn chế lớn về khả năng diễn giải (Interpretability)13. PCA cho phép các vector cơ sở và hệ số mang giá trị âm14. Điều này dẫn đến việc tái tạo dữ liệu thông qua các phép cộng trừ phức tạp, khiến các thành phần triệt tiêu lẫn nhau (cancellation effect)15.
Về mặt toán học, sai số có thể nhỏ, nhưng các đặc trưng trích xuất được (như Eigenfaces) thường mang tính toàn thể (holistic) và rất khó để con người hiểu được ý nghĩa vật lý của chúng16.
Xuất phát từ hạn chế đó, NMF ra đời17. Phương pháp này áp đặt ràng buộc không âm, buộc thuật toán phải học các đặc trưng cục bộ (parts-based), giúp dữ liệu được biểu diễn một cách tự nhiên hơn thông qua các phép cộng gộp thuần túy18.
Slide 4, 5, 6: Vấn đề của PCA vs. Giải pháp NMF 19
Để hiểu rõ động lực ra đời của NMF, chúng ta cần nhìn lại bản chất toán học của việc phân rã ma trận. Cả PCA và NMF đều biểu diễn dữ liệu gốc V dưới dạng một tổ hợp tuyến tính (Linear Combination) của các vector cơ sở. Tuy nhiên, sự khác biệt nằm ở các ràng buộc:
•	Trong PCA (hay SVD), các vector cơ sở (Eigenvectors) phải thỏa mãn tính trực giao và hướng tới phương sai lớn nhất. Quan trọng hơn, các hệ số này có thể mang giá trị âm. Điều này dẫn đến một hiện tượng gọi là sự triệt tiêu (complex cancellation). Để tái tạo một bức ảnh khuôn mặt, PCA có thể lấy một khuôn mặt mẫu, rồi cộng thêm một thành phần, sau đó trừ đi một thành phần khác để xóa bỏ các chi tiết thừa. Chính vì cơ chế cộng-trừ phức tạp này, các vector cơ sở của PCA (như hình bên trái) mang tính toàn thể (holistic)– chúng trông giống như những khuôn mặt méo mó, chồng chập lên nhau, không mang một ý nghĩa vật lý cụ thể nào mà con người có thể gọi tên.
•	NMF giải quyết vấn đề này bằng một ràng buộc tự nhiên: tất cả ma trận V, W, và H đều phải lớn hơn hoặc bằng 024. Khi không cho phép phép trừ, chúng ta chỉ có thể tái tạo dữ liệu bằng cách cộng gộp (additive parts). Hãy tưởng tượng: Nếu bạn không thể ’trừ’ đi cái mũi từ một khuôn mặt, thì cách duy nhất để biểu diễn khuôn mặt đó là bạn phải có một vector cơ sở chứa sẵn cái mũi, một vector khác chứa sẵn con mắt, và ghép chúng lại. Đây chính là lý do tại sao NMF (hình bên phải) có khả năng học được các biểu diễn dựa trên bộ phận (parts-based representation). Các vector cơ sở học được trở nên thưa (sparse) và khu trú tại các vùng đặc trưng (như mắt, mũi, miệng). Điều này không chỉ đúng về mặt toán học mà còn tương đồng với cơ chế nhận thức của não bộ con người.
________________________________________
PHẦN 2: CƠ SỞ TOÁN HỌC & PHÂN TÍCH HÀM MẤT MÁT (6 Phút)
Slide 7, 8, 9: Bài toán tối ưu hóa 28
Về cơ bản, NMF là bài toán tìm kiếm sự xấp xỉ cho ma trận dữ liệu $V$ (kích thước $m \times n$) bằng tích của hai ma trận hạng thấp (low-rank):
•	Ma trận W (mk) Chứa các cơ sở đặc trưng (basis vectors)29.
•	Ma trận H (kn): Chứa các hệ số kích hoạt (coefficients) tương ứng30.
$k$ là số chiều ẩn, thường nhỏ hơn rất nhiều so với m và n, giúp nén dữ liệu hiệu quả
Thách thức cốt lõi khi giải bài toán Phân tích ma trận không âm (NMF) nằm ở Hàm Mục Tiêu (Loss Function) D(V||WH). Mục tiêu là tìm W ≥ 0 và H ≥ 0 để tối thiểu hóa Loss funct
Thách Thức Toán Học và Giải Pháp
•	Hàm mục tiêu của bài toán này là không lồi (non-convex) trên toàn cục (tức là khi xét đồng thời cả hai ma trận W và H). 
•	Tính chất: Một hàm lồi giống như một cái bát, chỉ có một Cực Tiểu Toàn Cục (Global Minimum). Ngược lại, tính không lồi tạo ra một bề mặt phức tạp với nhiều Cực Tiểu Địa Phương (Local Minima). 
•	Hậu quả: Chính vì tính không lồi này, chúng ta không có lời giải đóng (closed-form solution). Thuật toán tối ưu hóa có thể bị kẹt tại một cực tiểu địa phương bất kỳ, khiến cho chất lượng lời giải không được đảm bảo.
•	Giải Pháp: Khai Thác Tính Bi-Convexity: Để khắc phục, chúng ta khai thác tính chất Lồi Riêng Lẻ (Bi-convexity)36. Hàm mục tiêu là lồi trên $W$ khi ta cố định $H$, và ngược lại, nó là lồi trên $H$ khi ta cố định $W$37. Tính chất này cho phép chúng ta xây dựng các Giải Thuật Lặp (Iterative Updates) bằng cách tối ưu hóa luân phiên $W$ và $H$38. hàm mục tiêu sẽ luôn giảm trong mỗi bước và hội tụ về một cực tiểu địa phương	
Chìa Khóa Chất Lượng: Lựa chọn Độ đo Khác biệt (Divergence)
Chất lượng của kết quả NMF được quyết định bởi việc lựa chọn độ đo sự khác biệt Hàm Mất Mát39. Mỗi hàm $D$ tương ứng với một giả định thống kê khác nhau về dữ liệu đầu vào và nhiễu40404040.
Slide 10: Phân tích Toán học 3 Hàm Divergence 41
1. Chuẩn Frobenius (Frobenius Norm) - $L_2$
•	Bản chất Toán học: Đây là độ đo sai số kinh điển, thực chất là tổng bình phương sai số (Least Squares)—tức là khoảng cách Euclid giữa hai ma trận10.
•	Cơ sở Thống kê (MLE): Việc tối ưu hóa hàm này tương đương với bài toán Ước lượng Hợp lý Cực đại (MLE) khi giả định nhiễu cộng (Additive Noise) tuân theo Phân phối Chuẩn (Gaussian)11.
o	Giải thích: Nhiễu Cộng ($\epsilon$) được cộng thẳng vào tín hiệu gốc ($V \approx WH + \epsilon$). Phân phối Chuẩn là phân phối đối xứng, tập trung sai số quanh 0.
•	Hậu quả Thao tác: Vì công thức chứa phép bình phương $(...)^2$ 12, nó trừng phạt cực nặng các sai số lớn (outliers)13. Để giảm thiểu tổng sai số, thuật toán có xu hướng 'dàn đều sai số' ra các điểm lân cận14.
•	Đánh giá: Điều này dẫn đến hiệu ứng làm mượt (smoothing), khiến kết quả tái tạo thường bị nhòe và mất đi độ sắc nét ở các cạnh chi tiết15.
2.  Kullback-Leibler (KL) Divergence
•	Bản chất Toán học: Đây là độ đo độ lệch thông tin (Relative Entropy)16. Khác với Frobenius, hàm này là bất đối xứng17.
Hình phạt:
•	Phạt cực nặng (tien vo cung) khi $WH$ đánh giá quá thấp (khi $WH \to 0$).
•	Hình phạt cho việc đánh giá quá cao ($WH > V$) cũng tăng nhưng mức độ nhẹ hơn nhiều so với bên trái.
o		
•	Cơ sở Thống kê (MLE): Hàm KL tương ứng với giả định dữ liệu tuân theo Phân phối Poisson18.
o	Giải thích: Phân phối Poisson chuyên dùng để mô tả các sự kiện đếm được (count events), như tần suất từ xuất hiện trong văn bản19.
•	Hậu quả Thao tác: Hàm KL khuyến khích tính thưa (sparsity)20. Về mặt vật lý, nó giúp thuật toán tách biệt rõ ràng phần 'nền' (giá trị 0) và phần 'tín hiệu' (nét chữ).
•	Đánh giá: Đây là lựa chọn tối ưu cho dữ liệu thưa (sparse) như ảnh chữ viết tay hoặc văn bản 21, tạo ra các đặc trưng parts-based sắc nét hơn so với Frobenius22.
3.  Itakura-Saito (IS) Divergence
•	Bản chất Toán học: Hàm này đo sự khác biệt dựa trên tỷ lệ (ratio) thay vì hiệu số23.
•	Cơ sở Thống kê (MLE): Hàm IS tương ứng với giả định nhiễu nhân (Multiplicative Noise) tuân theo Phân phối Gamma24.
o	Giải thích: Nhiễu Nhân là khi cường độ nhiễu tỷ lệ thuận với cường độ tín hiệu gốc.
•	Hậu quả Thao tác: Đặc tính quan trọng nhất là Bất biến theo tỷ lệ (Scale Invariance)25. Ví dụ, sai số giữa (1 và 10) được coi trọng ngang bằng sai số giữa (10 và 100) vì tỷ lệ của chúng đều là 1026.
•	Đánh giá:
o	Ưu điểm: Rất mạnh trong xử lý âm thanh (do tai người nghe theo thang logarit)27.
o	Nhược điểm chí mạng: Công thức chứa phép chia cho ma trận tái tạo $WH$28. Với dữ liệu ảnh có nhiều điểm đen (pixel $\approx 0$), mẫu số tiến về 0 khiến biểu thức tiến tới vô cùng29. Điều này gây ra sự mất ổn định số học (Numerical Instability) và lỗi chia cho 030.

PHẦN 3: CHỨNG MINH & TRIỂN KHAI THUẬT TOÁN (5 Phút)
Slide 11, 12, 13: Từ Gradient Descent đến Multiplicative Update Rules (MUR) 58
Sau khi chọn được hàm mất mát, chúng ta đối mặt với bài toán tối ưu hóa có ràng buộc: Tìm $W, H$ sao cho hàm lỗi $J$ nhỏ nhất, nhưng với điều kiện $W, H \ge 0$59.
Thông thường, chúng ta nghĩ ngay đến Gradient Descent60. Tuy nhiên, phép trừ vector gradient trong GD có thể đẩy giá trị của $H$ xuống vùng âm61. Nếu dùng GD thuần túy, chúng ta phải liên tục thực hiện phép chiếu (Projected Gradient Descent) để gán các giá trị âm về 0, làm thuật toán chậm và kém hiệu quả62.
Để giải quyết, tôi sử dụng phương pháp Cập nhật nhân (Multiplicative Update Rules - MUR) của Lee & Seung63. Đây là cách chứng minh công thức này dựa trên quan điểm về Learning Rate thích ứng:

        
Kết luận: Chúng ta đã biến phép trừ thành phép nhân75. Nếu khởi tạo $W, H$ dương, quy tắc cập nhật này đảm bảo kết quả luôn dương mãi mãi mà không cần kiểm tra điều kiện biên76. Đồng thời, khi đạo hàm bằng 0 $(\nabla^{+}=\nabla^{-})$, tỉ số này bằng 1, $H$ sẽ không đổi, nghĩa là thuật toán đã hội tụ tại điểm cực trị77.
Thách thức Kỹ thuật và giải pháp 78
Việc chứng minh toán học rất đẹp, nhưng khi triển khai thực tế (Implementation) trên máy tính, tôi gặp phải vấn đề về Tính ổn định số học (Numerical Stability)79.
•	Thách thức thực tế: Dữ liệu ảnh rất thưa (sparse)80. Khi thuật toán hoạt động, ma trận tái tạo $WH$ có thể tiến dần về 081.
o	Với IS Divergence, công thức cập nhật có phép chia cho $(WH)^{2}$. Khi mẫu số này tiến về 0, máy tính sẽ gặp lỗi ZeroDivisionError82.
o	Với KL Divergence, công thức chứa phép toán $log(WH)$. Khi $WH$ tiến về 0, $log(WH)$ sẽ là âm vô cùng, khiến máy tính trả về NaN (Not a Number), làm thuật toán sụp đổ83.
•	Giải pháp Kỹ thuật - Smoothing: Để khắc phục, tôi áp dụng kỹ thuật làm trơn (Smoothing), một chuẩn mực trong tính toán số84. Tôi cộng thêm một hằng số cực nhỏ $\epsilon$ (Machine Epsilon, thường là $10^{-9}$ hoặc $10^{-16}$) vào mẫu số hoặc bên trong hàm $log$85.
(Chỉ vào code) 86
Ví dụ trong hàm update_kl: numerator W.T @ (V/ (WH+ epsilon)) (Tránh chia cho 0) 87
Việc thêm $\epsilon$ này đóng vai trò như một bộ điều hòa (regularizer) số học, đảm bảo các phép toán luôn xác định mà sai số gây ra là không đáng kể, giúp thuật toán hội tụ ổn định ngay cả trên các tập dữ liệu cực thưa88.

