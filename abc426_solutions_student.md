# Hướng dẫn giải chi tiết 4 bài ngày 06.10.2025
## Dành cho học sinh THPT

---

# Bài A: Phiên bản hệ điều hành

## Đề bài

Có 3 phiên bản hệ điều hành theo thứ tự từ cũ đến mới: **Ocelot** → **Serval** → **Lynx**

Cho 2 phiên bản X và Y, hỏi X có mới hơn hoặc bằng Y không?

**Ví dụ:**
- Serval và Ocelot → Serval mới hơn → In `Yes`
- Serval và Lynx → Serval cũ hơn → In `No`

## Ý tưởng

**Cách nghĩ đơn giản:**

Giống như so sánh tuổi của 3 bạn:
- Ocelot = lớp 10 (nhỏ nhất)
- Serval = lớp 11 (giữa)
- Lynx = lớp 12 (lớn nhất)

Ta chỉ cần gán số cho mỗi phiên bản rồi so sánh!

## Cách giải

### Bước 1: Gán số cho mỗi phiên bản

```
Ocelot → 1 (cũ nhất)
Serval → 2 (giữa)
Lynx   → 3 (mới nhất)
```

### Bước 2: So sánh

- Nếu số của X ≥ số của Y → In `Yes`
- Ngược lại → In `No`

## Code C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string x, y;
    cin >> x >> y;  // Đọc 2 phiên bản
    
    // Tạo "từ điển" để tra cứu số thứ tự
    map<string, int> thutu;
    thutu["Ocelot"] = 1;  // Ocelot là số 1 (cũ nhất)
    thutu["Serval"] = 2;  // Serval là số 2
    thutu["Lynx"] = 3;    // Lynx là số 3 (mới nhất)
    
    // So sánh: nếu X >= Y thì in Yes
    if (thutu[x] >= thutu[y]) {
        cout << "Yes" << endl;
    } else {
        cout << "No" << endl;
    }
    
    return 0;
}
```

## Code Python

```python
# Đọc 2 phiên bản
x, y = input().split()

# Tạo "từ điển" để tra cứu số thứ tự
thutu = {
    "Ocelot": 1,  # Ocelot là số 1 (cũ nhất)
    "Serval": 2,  # Serval là số 2
    "Lynx": 3     # Lynx là số 3 (mới nhất)
}

# So sánh: nếu X >= Y thì in Yes
if thutu[x] >= thutu[y]:
    print("Yes")
else:
    print("No")
```

### So sánh C++ và Python

| Tính năng | C++ | Python |
|-----------|-----|--------|
| **Đọc input** | `cin >> x >> y;` | `x, y = input().split()` |
| **Dictionary** | `map<string, int>` | `dict` (tự động) |
| **Khai báo** | Phải khai báo kiểu | Không cần khai báo |
| **In output** | `cout << "Yes" << endl;` | `print("Yes")` |
| **Dấu ;** | Bắt buộc | Không cần |

**Python dễ hơn vì:**
- Cú pháp ngắn gọn hơn
- Không cần `#include`, `using namespace`
- Không cần `main()`, `return 0`
- Tự động quản lý bộ nhớ

## Giải thích

**Dòng `map<string, int> thutu;`**
- `map` giống như một "cuốn từ điển"
- Cho phép tra cứu: đưa vào tên (string) → nhận về số (int)
- Ví dụ: `thutu["Ocelot"]` sẽ trả về `1`

**Dòng `thutu[x] >= thutu[y]`**
- Tra cứu số của X và Y rồi so sánh
- Ví dụ: X="Serval", Y="Ocelot"
  - `thutu["Serval"]` = 2
  - `thutu["Ocelot"]` = 1
  - 2 >= 1 → Đúng → In "Yes"

## Kiến thức cần biết

1. **Map (từ điển)**: Lưu trữ cặp key-value
2. **So sánh**: Toán tử `>=`, `<=`, `==`
3. **If-else**: Câu lệnh rẽ nhánh


# Bài B: Kẻ khác biệt

## Đề bài

Cho một xâu chỉ có 2 loại ký tự. Trong đó có **đúng 1 ký tự** xuất hiện 1 lần, các ký tự còn lại giống nhau và xuất hiện nhiều lần. Tìm ký tự xuất hiện 1 lần đó.

**Ví dụ:**
- `odd` → `o` xuất hiện 1 lần, `d` xuất hiện 2 lần → Đáp án: `o`
- `dad` → `a` xuất hiện 1 lần, `d` xuất hiện 2 lần → Đáp án: `a`

## Ý tưởng

**Mapping:**

Giống như trong lớp có 30 học sinh:
- 29 bạn mặc áo trắng
- 1 bạn mặc áo đỏ

Nhiệm vụ: Tìm bạn mặc áo đỏ (khác biệt)!

**Cách làm:** Đếm số lần xuất hiện của mỗi ký tự, ký tự nào xuất hiện 1 lần là đáp án.

## Cách giải

### Bước 1: Đếm số lần xuất hiện

Duyệt qua xâu, đếm xem mỗi ký tự xuất hiện bao nhiêu lần.

### Bước 2: Tìm ký tự xuất hiện 1 lần

Duyệt qua kết quả đếm, ký tự nào có số lần = 1 là đáp án.

## Code C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    cin >> s;  // Đọc xâu
    
    // Bước 1: Tạo "sổ đếm" cho mỗi ký tự
    map<char, int> dem;  // dem['o'] = số lần xuất hiện của 'o'
    
    // Đếm số lần xuất hiện của từng ký tự
    for (int i = 0; i < s.length(); i++) {
        dem[s[i]]++;  // Tăng số đếm lên 1
    }
    
    // Bước 2: Tìm ký tự xuất hiện 1 lần
    for (auto cap : dem) {
        char kytu = cap.first;      // Ký tự
        int soLan = cap.second;     // Số lần xuất hiện
        
        if (soLan == 1) {
            cout << kytu << endl;
            break;  // Tìm thấy rồi thì dừng
        }
    }
    
    return 0;
}
```

## Code Python(Cách 1: Dùng dictionary)

```python
s = input()  # Đọc xâu

# Bước 1: Tạo "sổ đếm" cho mỗi ký tự
dem = {}  # dem['o'] = số lần xuất hiện của 'o'

# Đếm số lần xuất hiện của từng ký tự
for kytu in s:
    if kytu in dem:
        dem[kytu] += 1  # Tăng số đếm
    else:
        dem[kytu] = 1   # Lần đầu gặp

# Bước 2: Tìm ký tự xuất hiện 1 lần
for kytu, soLan in dem.items():
    if soLan == 1:
        print(kytu)
        break  # Tìm thấy rồi thì dừng
```

## Code Python (Cách 2: Dùng Counter)

```python
from collections import Counter

s = input()

# Đếm số lần xuất hiện - Counter làm tự động!
dem = Counter(s)

# Tìm ký tự xuất hiện 1 lần
for kytu, soLan in dem.items():
    if soLan == 1:
        print(kytu)
        break
```

## Code Python cực ngắn (Cách 3: One-liner)

```python
s = input()

# Tìm và in ngay trong 1 dòng
print([c for c in set(s) if s.count(c) == 1][0])
```

### So sánh 3 cách Python

| Cách | Độ dài | Dễ hiểu | Tốc độ |
|------|--------|---------|--------|
| **Cách 1: Dict thường** | Dài nhất | Dễ nhất | Nhanh |
| **Cách 2: Counter** | Trung bình | Dễ | Nhanh nhất |
| **Cách 3: One-liner** | Ngắn nhất | Khó | Chậm hơn |

**Khuyến nghị cho học sinh:**
- Mới học: Dùng **Cách 1** (dict thường)
- Đã quen: Dùng **Cách 2** (Counter)
- Làm nhanh: Dùng **Cách 3** (nhưng khó debug)

## Giải thích

**Ví dụ với xâu `"odd"`:**

1. **Sau vòng lặp đếm:**
   ```
   dem['o'] = 1
   dem['d'] = 2
   ```

2. **Duyệt qua map:**
   - Xét `o`: số lần = 1 → Đúng! → In `o`

**Cách khác: Dùng mảng đếm (nhanh hơn)**

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    cin >> s;
    
    // Mảng đếm cho 26 chữ cái (a-z)
    int dem[26] = {0};  // Khởi tạo tất cả = 0
    
    // Đếm
    for (char c : s) {
        dem[c - 'a']++;  // 'a'→0, 'b'→1, 'c'→2, ...
    }
    
    // Tìm
    for (int i = 0; i < 26; i++) {
        if (dem[i] == 1) {
            cout << (char)('a' + i) << endl;
            break;
        }
    }
    
    return 0;
}
```

## Giải thích "c - 'a'"

Đây là cách "mã hóa" chữ cái thành số:
- `'a' - 'a'` = 0
- `'b' - 'a'` = 1
- `'c' - 'a'` = 2
- ...
- `'z' - 'a'` = 25

Vì vậy: `dem[c - 'a']` là ô nhớ tương ứng với ký tự c.

## Kiến thức

1. **Map**: Từ điển, lưu cặp key-value
2. **Mảng**: Danh sách các ô nhớ liên tiếp
3. **Vòng lặp for**: Duyệt qua từng phần tử
4. **ASCII**: Mã hóa ký tự thành số

---

# Bài C: Yêu cầu nâng cấp

## Đề bài

Có N máy tính, ban đầu máy thứ i có phiên bản i (1, 2, 3, ..., N).

Có Q thao tác, mỗi thao tác: **Nâng cấp tất cả máy có phiên bản ≤ X lên phiên bản Y**

Với mỗi thao tác, in ra số máy được nâng cấp.

**Ví dụ:**
```
8 máy: [1, 2, 3, 4, 5, 6, 7, 8]
Thao tác "2 6": Nâng ≤2 lên 6
→ Máy 1, 2 được nâng cấp (2 máy)
→ Trạng thái mới: [6, 6, 3, 4, 5, 6, 7, 8]
```

## Ý tưởng

**Nhận xét:**

Sau khi nâng cấp các máy ≤ X, sẽ **không còn máy nào** có phiên bản ≤ X nữa!

→ Phiên bản **cũ nhất** bây giờ là X+1

**Ví dụ:**
```
Ban đầu: [1, 2, 3, 4, 5]
Nâng ≤2 → [?, ?, 3, 4, 5]
→ Phiên bản cũ nhất bây giờ là 3!

Tiếp theo nâng ≤1 → Không còn máy nào ≤1
→ Không nâng cấp máy nào (0 máy)
```

## Cách giải

**Ý tưởng:**

Duy trì biến `oldest` = phiên bản cũ nhất hiện có

**Thuật toán:**

1. Ban đầu `oldest = 1`
2. Với mỗi thao tác (X, Y):
   - Nếu X < oldest → Không có máy nào cần nâng (trả về 0)
   - Nếu X ≥ oldest:
     - Nâng cấp từ `oldest` đến X
     - Cập nhật `oldest = X + 1`

## Code C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, q;
    cin >> n >> q;  // n máy, q thao tác
    
    // Mảng đếm: count[i] = số máy có phiên bản i
    vector<int> count(n + 1, 1);  // Ban đầu mỗi phiên bản có 1 máy
    count[0] = 0;  // Không có phiên bản 0
    
    int oldest = 1;  // Phiên bản cũ nhất
    
    // Xử lý từng thao tác
    for (int t = 0; t < q; t++) {
        int x, y;
        cin >> x >> y;
        
        int upgraded = 0;  // Số máy được nâng cấp
        
        // Nâng cấp từ oldest đến x
        while (oldest <= x) {
            upgraded += count[oldest];     // Cộng số máy có phiên bản oldest
            count[y] += count[oldest];     // Chuyển sang phiên bản y
            oldest++;                       // Tăng oldest
        }
        
        cout << upgraded << "\n";
    }
    
    return 0;
}
```

## Code Python

```python
n, q = map(int, input().split())  # Đọc n và q

# Mảng đếm: count[i] = số máy có phiên bản i
count = [0] * (n + 1)  # Tạo mảng n+1 phần tử, tất cả = 0
for i in range(1, n + 1):
    count[i] = 1  # Ban đầu mỗi phiên bản có 1 máy

oldest = 1  # Phiên bản cũ nhất

# Xử lý từng thao tác
for _ in range(q):
    x, y = map(int, input().split())
    
    upgraded = 0  # Số máy được nâng cấp
    
    # Nâng cấp từ oldest đến x
    while oldest <= x:
        upgraded += count[oldest]    # Cộng số máy có phiên bản oldest
        count[y] += count[oldest]    # Chuyển sang phiên bản y
        oldest += 1                   # Tăng oldest
    
    print(upgraded)
```

## Code Python ngắn gọn hơn

```python
n, q = map(int, input().split())

# Khởi tạo mảng đếm ngắn gọn
count = [1] * (n + 1)  # Tất cả = 1
count[0] = 0           # Trừ phần tử 0

oldest = 1

for _ in range(q):
    x, y = map(int, input().split())
    
    upgraded = 0
    while oldest <= x:
        upgraded += count[oldest]
        count[y] += count[oldest]
        oldest += 1
    
    print(upgraded)
```

### So sánh C++ và Python

| Tính năng | C++ | Python |
|-----------|-----|--------|
| **Mảng động** | `vector<int> count(n+1, 1)` | `count = [1] * (n+1)` |
| **Vòng lặp q lần** | `for (int t = 0; t < q; t++)` | `for _ in range(q):` |
| **Không cần biến đếm** | Phải dùng `t` | Dùng `_` (bỏ qua) |
| **In output** | `cout << upgraded << "\n"` | `print(upgraded)` |

**Điểm khác biệt quan trọng:**

1. **Khởi tạo mảng:**
   - C++: `vector<int> count(n+1, 1)` - tạo mảng với giá trị khởi tạo
   - Python: `[1] * (n+1)` - nhân list

2. **Vòng lặp không cần biến:**
   - Python: `for _ in range(q)` - dùng `_` khi không dùng biến đếm
   - C++: Phải khai báo biến `t`

3. **Cú pháp ngắn gọn:**
   - Python dễ đọc hơn, ít dòng hơn
   - C++ cần nhiều cú pháp hơn nhưng chạy nhanh hơn

## Giải thích

**Input:**
```
8 5
2 6
3 5
1 7
5 7
7 8
```

**Quá trình xử lý:**

**Ban đầu:**
```
count = [0, 1, 1, 1, 1, 1, 1, 1, 1]
         0  1  2  3  4  5  6  7  8  ← phiên bản
oldest = 1
```

**Thao tác 1: X=2, Y=6**
```
while (oldest <= 2):
  oldest=1: upgraded += count[1]=1, count[6] += 1
  oldest=2: upgraded += count[2]=1, count[6] += 1
→ upgraded = 2, oldest = 3
```

**Thao tác 2: X=3, Y=5**
```
while (oldest <= 3):
  oldest=3: upgraded += count[3]=1, count[5] += 1
→ upgraded = 1, oldest = 4
```

**Thao tác 3: X=1, Y=7**
```
X=1 < oldest=4 → Không vào vòng while
→ upgraded = 0
```

## Tại sao?

**Điểm mấu chốt:**

Mỗi phiên bản chỉ được xử lý **đúng 1 lần**!

Vì `oldest` chỉ tăng không giảm, nên:
- Phiên bản 1 chỉ được xét 1 lần
- Phiên bản 2 chỉ được xét 1 lần
- ...

→ Độ phức tạp: O(N + Q) thay vì O(N × Q)

## Kiến thức cần biết

1. **Vector**: Mảng động trong C++
2. **While loop**: Vòng lặp có điều kiện
3. **Greedy**: Tham lam - dùng biến `oldest` để tối ưu
4. **Amortized analysis**: Phân tích khấu hao

## Sai lầm thường gặp

**Sai:** Duyệt tất cả N máy mỗi thao tác
```cpp
// Code này CHẬM (O(N×Q))
for (int i = 1; i <= x; i++) {
    // Xử lý máy i
}
```

**Đúng:** Dùng biến `oldest` để bỏ qua các phiên bản đã xử lý

---

# Bài D: Xóa và chèn (Khó nhất!)

## Đề bài

Cho xâu binary (chỉ có 0 và 1). Mỗi thao tác:
- Xóa ký tự đầu hoặc cuối
- Đảo ngược nó (0→1, 1→0)
- Chèn lại vào vị trí bất kỳ

**Mục tiêu:** Biến tất cả ký tự giống nhau với số thao tác ít nhất.

**Ví dụ:**
```
"01001" → Cần 4 thao tác để thành "00000" hoặc "11111"
"000" → Đã giống nhau → 0 thao tác
```

## Ý tưởng (Khó!)

**Câu hỏi:** Làm sao biến "01001" thành "00000" hoặc "11111"?

**Quan sát:**

Có 2 lựa chọn:
1. Biến tất cả thành '0'
2. Biến tất cả thành '1'

Với mỗi lựa chọn, ta tính chi phí, rồi chọn chi phí nhỏ hơn.

## Phân tích chi tiết

**Chiến lược thông minh:**

Khi biến thành toàn '0', ta nên **giữ lại dãy '0' liên tiếp dài nhất**, rồi "gộp" các phần khác vào đó.

**Ví dụ:**
```
"01001"
Dãy 0: "0" (độ dài 1), "00" (độ dài 2)
→ Giữ dãy "00" (dài nhất)
→ Gộp các phần khác vào "00"
```

**Chi phí:**

Giả sử biến thành toàn '0':
- **Dãy '0' dài nhất**: Giữ nguyên → 0 thao tác
- **Mỗi ký tự '1'**: Cần 1 thao tác (xóa-đảo-chèn)
- **Các ký tự '0' khác**: Cần 2 thao tác (di chuyển vào dãy chính)

## Công thức

```
Đếm:
- count_0 = số ký tự '0'
- count_1 = số ký tự '1'
- max_run_0 = độ dài dãy '0' liên tiếp dài nhất
- max_run_1 = độ dài dãy '1' liên tiếp dài nhất

Tính chi phí:
- ans_to_0 = count_1 + 2 × (count_0 - max_run_0)
- ans_to_1 = count_0 + 2 × (count_1 - max_run_1)

Đáp án = min(ans_to_0, ans_to_1)
```

## Code C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;  // Số test case
    
    while (t--) {
        int n;
        string s;
        cin >> n >> s;
        
        // Bước 1: Đếm số ký tự 0 và 1
        int count0 = 0, count1 = 0;
        for (char c : s) {
            if (c == '0') count0++;
            else count1++;
        }
        
        // Bước 2: Tìm độ dài dãy liên tiếp dài nhất
        int max0 = 0, max1 = 0;  // Dãy dài nhất của 0 và 1
        int cur = 1;              // Độ dài dãy hiện tại
        char curChar = s[0];      // Ký tự hiện tại
        
        for (int i = 1; i < n; i++) {
            if (s[i] == curChar) {
                cur++;  // Còn trong dãy cũ
            } else {
                // Kết thúc dãy cũ
                if (curChar == '0') {
                    max0 = max(max0, cur);
                } else {
                    max1 = max(max1, cur);
                }
                
                // Bắt đầu dãy mới
                curChar = s[i];
                cur = 1;
            }
        }
        
        // Đừng quên dãy cuối cùng!
        if (curChar == '0') {
            max0 = max(max0, cur);
        } else {
            max1 = max(max1, cur);
        }
        
        // Bước 3: Tính chi phí và chọn min
        long long ansTo0 = count1 + 2LL * (count0 - max0);
        long long ansTo1 = count0 + 2LL * (count1 - max1);
        
        cout << min(ansTo0, ansTo1) << "\n";
    }
    
    return 0;
}
```

## Code Python

```python
t = int(input())  # Số test case

for _ in range(t):
    n = int(input())
    s = input()
    
    # Bước 1: Đếm số ký tự 0 và 1
    count0 = s.count('0')  # Python có hàm count sẵn!
    count1 = s.count('1')
    
    # Bước 2: Tìm độ dài dãy liên tiếp dài nhất
    max0, max1 = 0, 0  # Dãy dài nhất của 0 và 1
    cur = 1             # Độ dài dãy hiện tại
    cur_char = s[0]     # Ký tự hiện tại
    
    for i in range(1, n):
        if s[i] == cur_char:
            cur += 1  # Còn trong dãy cũ
        else:
            # Kết thúc dãy cũ
            if cur_char == '0':
                max0 = max(max0, cur)
            else:
                max1 = max(max1, cur)
            
            # Bắt đầu dãy mới
            cur_char = s[i]
            cur = 1
    
    # Đừng quên dãy cuối cùng!
    if cur_char == '0':
        max0 = max(max0, cur)
    else:
        max1 = max(max1, cur)
    
    # Bước 3: Tính chi phí và chọn min
    ans_to_0 = count1 + 2 * (count0 - max0)
    ans_to_1 = count0 + 2 * (count1 - max1)
    
    print(min(ans_to_0, ans_to_1))
```

## Code Python với itertools.groupby (Nâng cao)

```python
from itertools import groupby

t = int(input())

for _ in range(t):
    n = int(input())
    s = input()
    
    # Đếm số ký tự
    count0 = s.count('0')
    count1 = s.count('1')
    
    # Tìm dãy dài nhất dùng groupby - tự động nhóm!
    max0, max1 = 0, 0
    
    for char, group in groupby(s):
        length = len(list(group))
        if char == '0':
            max0 = max(max0, length)
        else:
            max1 = max(max1, length)
    
    # Tính kết quả
    ans_to_0 = count1 + 2 * (count0 - max0)
    ans_to_1 = count0 + 2 * (count1 - max1)
    
    print(min(ans_to_0, ans_to_1))
```

### So sánh C++ và Python

| Tính năng | C++ | Python |
|-----------|-----|--------|
| **Đếm ký tự** | Phải dùng vòng lặp | `s.count('0')` - có sẵn! |
| **Khai báo nhiều biến** | `int max0 = 0, max1 = 0;` | `max0, max1 = 0, 0` |
| **Long long** | `2LL * (count0 - max0)` | Tự động, không cần lo |
| **Groupby** | Phải tự code | `groupby(s)` - có sẵn! |

### Giải thích itertools.groupby

**groupby là gì?**

Hàm `groupby` tự động nhóm các phần tử liền kề giống nhau:

```python
from itertools import groupby

s = "01001"
for char, group in groupby(s):
    print(f"Nhóm '{char}': {list(group)}")

# Output:
# Nhóm '0': ['0']
# Nhóm '1': ['1']  
# Nhóm '0': ['0', '0']
# Nhóm '1': ['1']
```

**Ưu điểm:**
- Code ngắn gọn
- Dễ đọc
- Ít bug hơn (không cần xử lý dãy cuối)

**Nhược điểm:**
- Cần import thư viện
- Học sinh mới chưa quen

## Giải thích

**Ví dụ: s = "01001"**

**Bước 1: Đếm**
```
count0 = 3 (có 3 ký tự '0')
count1 = 2 (có 2 ký tự '1')
```

**Bước 2: Tìm dãy dài nhất**
```
Duyệt qua xâu:
i=0: s[0]='0', curChar='0', cur=1
i=1: s[1]='1' ≠ '0' → Kết thúc dãy '0' độ dài 1
     max0 = 1, curChar='1', cur=1
i=2: s[2]='0' ≠ '1' → Kết thúc dãy '1' độ dài 1
     max1 = 1, curChar='0', cur=1
i=3: s[3]='0' = '0' → cur=2
i=4: s[4]='1' ≠ '0' → Kết thúc dãy '0' độ dài 2
     max0 = max(1, 2) = 2, curChar='1', cur=1

Cuối cùng: Dãy '1' độ dài 1 → max1 = 1

Kết quả: max0=2, max1=1
```

**Bước 3: Tính chi phí**
```
Biến thành toàn 0:
  - Giữ dãy "00" (2 ký tự)
  - Xử lý 2 ký tự '1': 2 × 1 = 2
  - Xử lý 1 ký tự '0' còn lại: 1 × 2 = 2
  → ansTo0 = 2 + 2 = 4

Biến thành toàn 1:
  - Giữ dãy "1" (1 ký tự)
  - Xử lý 3 ký tự '0': 3 × 1 = 3
  - Xử lý 1 ký tự '1' còn lại: 1 × 2 = 2
  → ansTo1 = 3 + 2 = 5

Đáp án = min(4, 5) = 4
```

## Tại sao?

**Giải thích bằng hình ảnh:**

Tưởng tượng bạn có:
- Một **dãy '0' dài** như một "cơ sở"
- Các ký tự khác cần "bay về" cơ sở đó

**Chi phí "bay về":**
- Ký tự '1': Đổi thành '0' → 1 thao tác
- Ký tự '0' xa cơ sở: Di chuyển về → 2 thao tác

## Kiến thức cần biết

1. **Greedy**: Tham lam - giữ phần tốt nhất
2. **String processing**: Xử lý xâu
3. **Finding longest run**: Tìm dãy con liên tiếp dài nhất
4. **Cost optimization**: Tối ưu hóa chi phí

## Lưu ý quan trọng

1. **Đừng quên dãy cuối:** Sau vòng lặp phải xử lý dãy cuối cùng!
2. **Dùng long long:** Với N lớn, kết quả có thể vượt int
3. **2 trường hợp:** Phải xét cả biến thành 0 VÀ biến thành 1
---

# Tổng kết

## Độ khó tăng dần

```
Bài A ⭐        → Học về Map, So sánh
Bài B ⭐        → Học về Đếm, Map/Array
Bài C ⭐⭐      → Học về Greedy, Tối ưu hóa
Bài D ⭐⭐⭐    → Học về Greedy nâng cao, Phân tích chi phí
```

## Tips chung khi làm bài

1. **Đọc kỹ đề:** Gạch chân các từ khóa
2. **Làm ví dụ:** Thử với test mẫu trước
3. **Vẽ ra giấy:** Minh họa thuật toán
4. **Code từng bước:** Không code hết một lúc
5. **Test kỹ:** Thử nhiều test case
6. **Debug thông minh:** Dùng cout để in ra
7. **Đọc code mẫu:** Học cách code của người khác
8. **Làm lại:** Làm nhiều lần để nhớ lâu

### Thói quen tốt khi code

**Cho cả Python và C++:**

1. **Comment code:** Viết chú thích bằng tiếng Việt
2. **Đặt tên biến rõ ràng:** `dem` thay vì `d`, `oldest` thay vì `o`
3. **Test nhiều case:** Không chỉ test mẫu
4. **Format code đẹp:** Thụt lề đúng
5. **Code từng bước:** Không code hết một lúc
6. **Đọc code người khác:** Học cách code hay

### Tips đặc biệt cho Python

**1. Dùng list comprehension:**
```python
# Thay vì:
result = []
for i in range(10):
    result.append(i * 2)

# Dùng:
result = [i * 2 for i in range(10)]
```

**2. Dùng unpacking:**
```python
# Thay vì:
temp = a
a = b
b = temp

# Dùng:
a, b = b, a
```

**3. Dùng f-string (Python 3.6+):**
```python
# Thay vì:
print("Kết quả:", result)

# Dùng:
print(f"Kết quả: {result}")
```

**4. Import thông minh:**
```python
# Thường dùng:
from collections import Counter, defaultdict
from itertools import groupby, combinations
import math
```

## Lời khuyên cuối

> "Học lập trình giống học toán: Phải làm bài tập thật nhiều!"
> 
> "Không sợ sai, chỉ sợ không làm!"