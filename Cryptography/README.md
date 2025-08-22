# Cryptography

## **[1] Kiến thức cơ bản**
### **[1.1] Encoding và Encryption**
- Encoding và Encryption là 2 khái niệm khác nhau, để phân biệt, chúng ta cần dựa vào mục đích và đặc điểm nhận diện như sau:
- Encoding:
    - Mục đích: Biểu diễn dữ liệu dưới một định dạng khác (thường là văn bản) để dễ dàng truyền tải hoặc lưu trữ.
    - Đặc điểm:
        - Có thể đảo ngược (decode) dễ dàng, không cần khóa.
        - Ai cũng có thể giải mã nếu biết chuẩn encoding.
    - Các loại phổ biến:
        - Base Family: Base64, Base32, Base16 (Hex) là những dạng rất thường gặp. Đặc điểm nhận dạng Base64 là các ký tự A-Z, a-z, 0-9, +, / và có thể có ký tự đệm = ở cuối.
        - URL Encoding: Biểu diễn các ký tự đặc biệt bằng ký tự % theo sau là mã hex.
        - Mã hóa Javascript: Các dạng mã hóa gây rối (obfuscation) như jjencode (chỉ dùng ký hiệu) và aaencode (dùng emoji).
- Encryption:
    - Mục đích: Biến dữ liệu gốc (plaintext) thành dữ liệu không đọc được (ciphertext) để bảo vệ thông tin, chỉ người có khóa mới giải mã được.
    - Đặc điểm:
        - Cần có thuật toán và khóa bí mật/khóa công khai để mã hóa/giải mã.
        - Không phải ai cũng giải mã được, trừ khi có khóa hợp lệ.
        - Nhằm đảm bảo tính bảo mật, tính toàn vẹn, tính xác thực.
    - Các loại phổ biến:
        - AES, DES (mã hóa đối xứng).
        - RSA, ECC (mã hóa bất đối xứng).

### **[1.2] Mật mã cổ điển (Classical Ciphers)**
- Đây là những hệ mật mã đơn giản, thường xuất hiện trong các bài khởi động và dễ bị phá vỡ bằng các phương pháp phân tích tần suất hoặc vét cạn.
- Caesar Cipher: Mã hóa dịch chuyển, mỗi ký tự được thay thế bằng một ký tự khác cách nó một khoảng cố định trong bảng chữ cái.
- Vigenère Cipher: Là dạng nâng cao của Caesar, sử dụng một từ khóa (keyword) để tạo ra các độ dịch chuyển khác nhau cho mỗi ký tự, khiến việc phân tích tần suất khó hơn.
- Mật mã thay thế cố định (Fixed Substitution): Các hệ mật như Bacon's Cipher (mỗi ký tự được biểu diễn bằng một chuỗi 5 ký tự 'a' hoặc 'b') và Pigpen Cipher (sử dụng lưới biểu tượng).
- Mật mã hoán vị (Shift Ciphers): Như Fence Cipher (mật mã hàng rào), sắp xếp lại thứ tự các ký tự.

### **[1.3] Mật mã khối (Block Ciphers)**
- Là các hệ mật mã đối xứng, mã hóa dữ liệu theo từng khối (block) có kích thước cố định.
- Các chế độ hoạt động (Modes of Operations):
    - ECB (Electronic Code Book): Đơn giản nhất, mỗi khối được mã hóa độc lập. Điểm yếu chết người là các khối plaintext giống nhau sẽ tạo ra các khối ciphertext giống nhau, làm lộ mẫu dữ liệu.
    - CBC (Cipher Block Chaining): An toàn hơn, mỗi khối plaintext được XOR với khối ciphertext trước đó rồi mới mã hóa. Cần một vector khởi tạo (IV) cho khối đầu tiên.
- Các thuật toán tiêu biểu:
    - DES: Chuẩn cũ, sử dụng cấu trúc Feistel, khóa 56-bit, hiện đã không còn an toàn.
    - AES (Rijndael): Chuẩn hiện đại và phổ biến nhất. Gồm các bước: SubBytes (thay thế S-box), ShiftRows (dịch hàng), MixColumns (trộn cột), và AddRoundKey (XOR với khóa).

### **[1.4] Mật mã dòng (Stream Ciphers)**
- Mã hóa dữ liệu theo từng bit hoặc byte bằng cách XOR plaintext với một dòng khóa (keystream) được tạo ra từ một bộ tạo số giả ngẫu nhiên (PRNG).
- Điểm yếu cốt lõi: Nếu dòng khóa có thể dự đoán được, hệ mật sẽ bị phá vỡ.
- Các ví dụ trong CTF:
    - LCG (Linear Congruential Generator): Một PRNG đơn giản, nếu biết được vài giá trị đầu ra và các tham số, có thể dự đoán toàn bộ chuỗi.
    - LFSR (Linear Feedback Shift Register): Cũng là một cơ chế tạo chuỗi có thể dự đoán.
    - RC4: Từng rất phổ biến nhưng có nhiều điểm yếu đã được phát hiện (ví dụ: weak keys).

### **[1.5] Mật mã khóa công khai (Public Key Cryptography)**
- Sử dụng một cặp khóa: khóa công khai (public key) để mã hóa và khóa bí mật (private key) để giải mã.
- RSA: Là "ông vua" của mật mã khóa công khai trong CTF. An toàn dựa trên độ khó của việc phân tích một số lớn n thành hai thừa số nguyên tố p và q.
- Các dạng tấn công RSA thường gặp:
    - N nhỏ: Dễ dàng phân tích n thành p và q.
    - Hàm mũ e nhỏ (Low Public Exponent): Khi e nhỏ (ví dụ e=3) và plaintext m cũng nhỏ, m^e có thể nhỏ hơn n. Ciphertext c = m^e, chỉ cần lấy căn bậc e của c là ra m.
    - Common Modulus Attack: Hai bản mã được tạo ra từ cùng một n nhưng khác e.
    - Hastad's Broadcast Attack: Cùng một plaintext m được mã hóa với cùng e nhưng khác n.
    - Wiener's Attack: Khi khóa bí mật d quá nhỏ.
- Hệ mật khác: ElGamal và ECC (Mật mã đường cong Elliptic), dựa trên độ khó của bài toán Logarithm rời rạc.

## **[2] Các tools thường sử dụng**
### **[2.1] Tool có sẵn**
- Tool chuyển đổi các hệ đếm: [kt.gy](https://kt.gy/tools.html#conv/)
- Tool mã hoá/giải mã các dạng mật mã cổ điển: [dcode.fr](https://www.dcode.fr/en)

### **[2.2] Các thư viện Python thường dùng cho Crypto**
- Thư viện toán học: `numpy`, `gmpy2`
    - Thư viện này cho phép làm việc hiệu quả với ma trận và mảng, đặc biệt là dữ liệu ma trận và mảng lớn với tốc độ xử lý nhanh hơn nhiều lần so với code thuần.
- Module xử lý số lớn: `Crypto`
    - Thư viện xử lý số lớn: `Crypto.Util.number`
    - Thư viện mã hoá/giải mã các thuật toán mã hoá đối xứng, mã hoá dòng: `Crypto.Cipher`