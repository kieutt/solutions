# Gợi Ý
**Code mẫu: https://ideone.com/9ZZAkr**

## Phân tích

**Bài toán yêu cầu:**
- Đếm số cấu hình khác nhau đạt được sau **đúng q thao tác**
- Mỗi thao tác có thể là 'r' hoặc 'b' với các quy tắc khác nhau
- Kết quả modulo 10^9 + 7

## Ý tưởng

### 1. **Nhận Diện "Masks" **
```
Ví dụ: "rrbwwbbrrr" và "rbbbwbbbbr" là TƯƠNG ĐƯƠNG
→ Cả hai có cùng chuỗi mặt nạ: "rbwbr"
```
- Loại bỏ các ký tự lặp liên tiếp
- Chỉ quan tâm đến thứ tự xuất hiện của r, b, w

### 2. **Independent Segments (Đoạn độc lập)**
- Các đoạn mặt nạ được ngăn cách bởi 'w' là **độc lập**
- Ví dụ: "rbwr" có 2 đoạn độc lập: "rb" và "r"
- Tính số thao tác cho mỗi đoạn riêng biệt

### 3. **Dynamic Programming**

Code sử dụng mảng 3 chiều:
```cpp
ways[m][num1][x]
```
Ý nghĩa:
- `m`: số điểm "rảnh" còn lại
- `num1`: số đoạn có độ dài 1 (chỉ gồm 'r')
- `x`: tổng độ dài tối thiểu các đoạn

### 4. **Brute Force tối ưu**

Code duyệt tất cả các mặt nạ khả thi:
```cpp
void brute(int pos, int mnsum, int num, int lst)
```
- `pos`: vị trí hiện tại trong mặt nạ
- `mnsum`: tổng độ dài tối thiểu
- `num`: số đoạn độc lập
- `lst`: độ dài tối thiểu của đoạn trước

## Thực hiện

### Bước 1: Tiền xử lý
```cpp
// Tính toán:
- Giai thừa gt[i]
- Nghịch đảo giai thừa igt[i]  
- Lũy thừa 2: pw2[i]
- Tổ hợp C(i,j)
```

### Bước 2: Tính DP `ways[m][num1][x]`
- Với mỗi trạng thái (m, num1, x)
- Đếm số cách phân bổ điểm vào các đoạn
- Sử dụng công thức tổ hợp

### Bước 3: Sinh tất cả các mặt nạ
- Duyệt đệ quy các độ dài đoạn khả thi
- Kiểm tra tính hợp lệ với chuỗi s ban đầu

### Bước 4: Kiểm tra khả thi
```cpp
// Với mỗi mặt nạ, kiểm tra:
- Có đủ 'r' và 'b' trong chuỗi s không?
- Thứ tự 'r' và 'b' có phù hợp không?
```

## Các lưu ý

1. **Union-Find (DSU)**: Dùng để theo dõi vị trí đã dùng
   ```cpp
   int frt(int i)
   ```

2. **Tính toán tổ hợp**: Công thức đếm cách phân phối là gì?

3. **Pruning (Cắt tỉa)**: 
   - Dừng sớm nếu `mnsum + num - 1 > n`
   - Kiểm tra tính khả thi của 'r' và 'b'

4. **Modular Arithmetic**: Luôn lấy mod 10^9 + 7

## Trình tự gọi ý cho Code

```cpp
// Cấu trúc chính:
1. Đọc input: n, K, s
2. Tiền xử lý: gt[], igt[], pw2[]
3. Tính DP: ways[m][num1][x]
4. Tìm vị trí r và b trong s
5. Duyệt tất cả mặt nạ khả thi
6. Với mỗi mặt nạ: kiểm tra + cộng vào ans
7. In kết quả
```

## Lưu ý:

### A. Hàm tính tổ hợp C(i, j)
```cpp
int C(int i, int j) {
    if (i > j) return 0;
    // C(i,j) = j! / ((j-i)! * i!)
    return gt[j] * 1LL * igt[j - i] % mod * igt[i] % mod;
}
```

### B. Hàm lũy thừa modulo
```cpp
int power(int x, int y) {
    int res = 1;
    while (y) {
        if (y & 1) res = res * 1LL * x % mod;
        x = x * 1LL * x % mod;
        y >>= 1;
    }
    return res;
}
```

### C. Union-Find với path compression
```cpp
int frt(int i) {
    return rt[i] == i ? i : rt[i] = frt(rt[i]);
}
```

## Kiến thức chuẩn bị

1. **Quy hoạch động (DP)**
   - Xác định trạng thái
   - Công thức chuyển trạng thái
   - Tính toán đáp án

2. **Toán tổ hợp**
   - Tổ hợp C(n,k)
   - Nghịch đảo modulo (Theo định lý Fermat's nhỏ)
   - Nguyên lý nhân và cộng

3. **Cấu trúc dữ liệu**
   - Union-Find (Disjoint Set Union - DSU)
   - Vector, Array

4. **Kỹ thuật đệ quy**
   - Backtracking
   - Pruning (cắt tỉa)

## Độ Phức Tạp

- **Tiền xử lý**: O(n³) cho việc tính mảng `ways`
- **Brute force**: Số lượng mặt nạ khả thi (rất nhỏ do pruning hiệu quả)
- **Tổng thể**: Chấp nhận được với n ≤ 70

## Các câu hỏi cần trả lời

1. **Tại sao phải loại bỏ các ký tự lặp liên tiếp?**
   - Vì các cấu hình khác nhau chỉ về số lượng ký tự lặp nhưng cùng thứ tự là tương đương

2. **Công thức tính `ways[m][num1][x]` dựa trên nguyên lý gì?**
   - Đếm số cách phân phối m điểm rảnh vào các đoạn, với ràng buộc về số đoạn độ dài 1

3. **Tại sao cần sử dụng Union-Find trong phần kiểm tra?**
   - Để theo dõi vị trí đã được sử dụng, tránh gán trùng lặp

4. **Độ phức tạp của thuật toán là bao nhiêu?**
   - O(n³) cho tiền xử lý + brute force với pruning hiệu quả
