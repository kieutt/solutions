# Lời giải Bài 3 — "Комбини" (Cửa hàng tiện lợi)

> Đối tượng: học sinh lớp 10 đã biết **chặt nhị phân (binary search)** và đang học **kỹ thuật hình học cơ bản**.
> Bài này dạy hai "chiêu" cực kỳ hữu dụng: **chặt nhị phân trên đáp số** và **phép xoay 45° đổi khoảng cách Manhattan thành Chebyshev**.

---

## 0. Tóm tắt đề bài

Có $n$ cửa hàng trên lưới phố, cửa hàng thứ $i$ ở $(x_i, y_i)$. Khoảng cách giữa hai cửa hàng là **Manhattan**:

$$
\rho(i,j) = |x_i - x_j| + |y_i - y_j|.
$$

Cần **chia $n$ cửa hàng thành 2 nhóm** (hai chuỗi cửa hàng) sao cho **khoảng cách lớn nhất giữa hai cửa hàng cùng nhóm là nhỏ nhất có thể**. In ra giá trị nhỏ nhất đó.

Giới hạn: $1 \le n \le 3\cdot10^5$; $1 \le x_i, y_i \le 5\cdot10^8$. Toạ độ có thể trùng.

Nói gọn: tô màu mỗi điểm bằng 1 trong 2 màu; "đường kính" của một màu = khoảng cách lớn nhất giữa 2 điểm cùng màu; ta muốn **max của hai đường kính là nhỏ nhất**.

---

## 1. Chiêu 1 — Chặt nhị phân trên đáp số

Đề hỏi "giá trị nhỏ nhất của một đại lượng max". Đây là dấu hiệu kinh điển của **chặt nhị phân trên đáp số**: thay vì tìm trực tiếp đáp số $D$, ta đặt câu hỏi **dễ hơn**:

> **"Với một giá trị $D$ cho trước, có thể chia 2 nhóm sao cho mọi cặp cùng nhóm đều có khoảng cách $\le D$ không?"**

Gọi hàm trả lời câu hỏi đó là `feasible(D)`. Nó có tính **đơn điệu**: nếu $D$ làm được thì mọi $D' > D$ cũng làm được (nới lỏng ràng buộc thì càng dễ). Nhờ đơn điệu, ta **chặt nhị phân** tìm $D$ nhỏ nhất khiến `feasible(D) = true`. Đáp số là số nguyên (khoảng cách Manhattan luôn nguyên), nằm trong $[0,\ \text{khoảng cách lớn nhất}]$.

Giờ trọng tâm chuyển sang: **viết `feasible(D)` cho đúng và nhanh.**

"Mọi cặp cùng nhóm $\le D$" nghĩa là **mỗi nhóm có đường kính $\le D$**. Vậy `feasible(D)` = "có chia được 2 nhóm, mỗi nhóm đường kính (theo Manhattan) $\le D$ không?".

---

## 2. Chiêu 2 — Xoay 45°: Manhattan → Chebyshev

Khoảng cách Manhattan có dấu giá trị tuyệt đối **cộng lại**, rất khó xử lý trực tiếp ("đường kính Manhattan của một tập" không phải là công thức gọn). Có một phép biến đổi thần kỳ. Đặt:

$$
u = x + y, \qquad v = x - y.
$$

Khi đó với hai điểm bất kỳ:

$$
\rho(i,j) = |x_i - x_j| + |y_i - y_j| \;=\; \max\big(|u_i - u_j|,\ |v_i - v_j|\big).
$$

Tức là **khoảng cách Manhattan trong toạ độ $(x,y)$ = khoảng cách Chebyshev trong toạ độ $(u,v)$** (Chebyshev = lấy $\max$ của chênh lệch từng trục).

> **Vì sao đúng?** Ta có $|a| + |b| = \max(|a+b|, |a-b|)$ với mọi số thực $a,b$ (thử 4 trường hợp dấu của $a,b$ sẽ thấy). Áp dụng $a = x_i - x_j,\ b = y_i - y_j$: $a+b = u_i - u_j$, $a - b = v_i - v_j$. Suy ra $|a|+|b| = \max(|u_i-u_j|, |v_i-v_j|)$. ∎

**Tại sao đổi sang Chebyshev lại lợi?** Vì với Chebyshev, "đường kính của một tập điểm $\le D$" có nghĩa **cực kỳ đơn giản**:

$$
\text{đường kính Chebyshev} \le D
\iff (\max u - \min u \le D)\ \text{và}\ (\max v - \min v \le D),
$$

tức **toàn bộ tập nằm gọn trong một hình vuông cạnh $D$** (cạnh song song trục $u, v$). Hai trục $u, v$ giờ **tách rời nhau**, dễ xử lý hơn nhiều so với tổng giá trị tuyệt đối.

**`feasible(D)` được phát biểu lại (rất gọn):**

> Trong toạ độ $(u, v)$, có thể **phủ tất cả điểm bằng 2 hình vuông cạnh $D$** (cạnh song song trục) hay không?

(Phủ = mỗi điểm nằm trong ít nhất một hình vuông; rồi gán điểm đó cho nhóm của hình vuông chứa nó. "Chia 2 nhóm, mỗi nhóm lọt 1 hình vuông" tương đương "2 hình vuông phủ hết".)

---

## 3. Kiểm tra phủ bằng 2 hình vuông: chỉ cần thử 4 góc

Đây là phần tinh tế nhất. Gọi **hộp bao** của toàn bộ điểm (trong toạ độ $u,v$) là hình chữ nhật

$$
[U_{\min}, U_{\max}] \times [V_{\min}, V_{\max}].
$$

**Khẳng định:** nếu phủ được bằng 2 hình vuông cạnh $D$, thì luôn tồn tại cách phủ trong đó **một hình vuông được "dán" vào một trong 4 góc của hộp bao**. Bốn lựa chọn dán góc (mỗi hình vuông cạnh $D$):

- **Góc dưới-trái (BL):** $[U_{\min}, U_{\min}+D] \times [V_{\min}, V_{\min}+D]$
- **Góc dưới-phải (BR):** $[U_{\max}-D, U_{\max}] \times [V_{\min}, V_{\min}+D]$
- **Góc trên-trái (TL):** $[U_{\min}, U_{\min}+D] \times [V_{\max}-D, V_{\max}]$
- **Góc trên-phải (TR):** $[U_{\max}-D, U_{\max}] \times [V_{\max}-D, V_{\max}]$

Nhờ khẳng định này, thuật toán kiểm tra cực gọn: **với mỗi góc trong 4 góc**, lấy hình vuông dán góc đó, tìm các điểm **không** bị nó phủ, rồi kiểm tra **các điểm còn lại có lọt vào một hình vuông cạnh $D$ không** (chỉ cần biên độ $u$ và biên độ $v$ của chúng đều $\le D$). Nếu **một góc nào đó** thoả → `feasible(D) = true`.

### 3.1. Vì sao chỉ cần dán góc? (ý tưởng chứng minh)

**Bổ đề trượt:** giả sử có cách phủ hợp lệ bằng 2 hình vuông $S_1, S_2$. Xét hình vuông $S_1$ và giả sử nó chứa điểm **trái nhất** (nhỏ $u$ nhất) và điểm **dưới nhất** (nhỏ $v$ nhất). Vì mọi điểm đều có $u \ge U_{\min}$ và $v \ge V_{\min}$, ta có thể **trượt $S_1$ về sát góc dưới-trái** thành hình BL mà **không làm mất điểm nào nó đang phủ** (trượt sang trái/xuống dưới chỉ "đẩy" mép phải/mép trên ra xa hơn, phủ rộng hơn về phía các điểm). Khi đó phần điểm còn lại $\subseteq$ phần $S_2$ phủ → vẫn lọt 1 hình vuông. Vậy cách phủ "dán BL" cũng hợp lệ.

**Vì sao luôn rơi vào một trong 4 góc?** Xét 4 điểm cực biên: $A$ (nhỏ $u$ nhất), $B$ (lớn $u$ nhất), $C$ (nhỏ $v$ nhất), $E$ (lớn $v$ nhất). Mỗi điểm thuộc $S_1$ hoặc $S_2$. Mỗi góc ứng với **một cặp** điểm cực biên cùng định nghĩa nó (BL ↔ $\{A, C\}$, TR ↔ $\{B, E\}$, BR ↔ $\{B, C\}$, TL ↔ $\{A, E\}$). Có thể chứng minh: **luôn có một cặp mà hai điểm rơi vào cùng một hình vuông** — trừ đúng một trường hợp đặc biệt là $S_1$ chứa cả $A, B$ còn $S_2$ chứa cả $C, E$; nhưng khi đó biên độ $u \le D$ **và** biên độ $v \le D$, nghĩa là **toàn bộ điểm lọt một hình vuông duy nhất**, và dán góc nào cũng phủ sạch. Tóm lại, **4 góc luôn đủ** để bắt mọi tình huống.

> **Bẫy quan trọng:** đừng dùng cách "cắt bằng một đường thẳng đứng hoặc nằm ngang rồi mỗi nửa là một nhóm". Cách đó **SAI**, vì hai hình vuông có thể **đè lên nhau theo cả hai trục** (ví dụ một hình "góc dưới-trái", một hình "góc trên-phải", phần giữa chồng nhau) mà không có đường thẳng nào tách được. Kiểm tra theo 4 góc mới đúng.

---

## 4. Tổng hợp thuật toán

1. Đổi toạ độ: với mỗi điểm tính $u_i = x_i + y_i$, $v_i = x_i - y_i$. Tính $U_{\min}, U_{\max}, V_{\min}, V_{\max}$.
2. Chặt nhị phân $D$ trong $[0,\ \max(U_{\max}-U_{\min},\ V_{\max}-V_{\min})]$:
   - `feasible(D)`: thử 4 góc. Với mỗi góc, duyệt các điểm, gom các điểm **không bị góc phủ** lại, lấy hộp bao của chúng; nếu hộp bao đó có biên độ $u$ và $v$ đều $\le D$ (hoặc không còn điểm nào) → trả `true`.
3. In ra $D$ nhỏ nhất khiến `feasible(D)` đúng.

**Cận trên của chặt nhị phân.** Lấy $D = \max(U_{\max}-U_{\min},\ V_{\max}-V_{\min})$: khi đó một hình vuông dán BL đã phủ **tất cả** (cả biên độ $u$ lẫn $v$ đều $\le D$), nên `feasible` chắc chắn đúng → đây là cận trên hợp lệ. Đáp số luôn $\le$ giá trị này (dồn hết vào 1 nhóm cũng chỉ tốn bằng đó).

---

## 5. Độ phức tạp

- `feasible(D)`: duyệt $n$ điểm cho mỗi góc, 4 góc → $O(4n) = O(n)$.
- Chặt nhị phân: $O(\log C)$ vòng, với $C \approx 10^9$ → khoảng 30 vòng.
- **Tổng:** $O(n \log C) \approx 30 \cdot 3\cdot10^5 \approx 10^7$ — chạy dưới 0,3 giây.
- Bộ nhớ: $O(n)$ để lưu $u, v$.

---

## 6. Trường hợp biên

- **$n = 1$ hoặc $n = 2$:** đặt mỗi cửa hàng vào một nhóm riêng → mỗi nhóm có $\le 1$ điểm → đường kính $0$. Đáp số $0$. Thuật toán tự ra (góc TL/BL phủ một điểm, điểm còn lại lọt hình vuông cạnh 0).
- **Mọi điểm trùng nhau:** đáp số $0$.
- **Điểm thẳng hàng / một trục không đổi:** vẫn đúng, vì $u, v$ vẫn tính bình thường.

---

## 7. Lỗi học sinh hay mắc

- **Tràn số.** $x, y$ tới $5\cdot10^8$ nên $u = x+y$ tới $10^9$. Hiệu $U_{\max}-U_{\min}$ tới $\approx 2\cdot10^9$ — **vượt `int`**! Dùng `long long` cho toàn bộ $u, v$ và các biên.
- **Hiểu sai phép xoay.** Nhớ $u = x+y$, $v = x-y$ (đừng nhầm dấu). Sau khi xoay, "đường kính Manhattan" thành "lọt hình vuông trục toạ độ theo $u,v$".
- **Dùng cách cắt đường thẳng** (prefix/suffix) → sai ở trường hợp 2 hình vuông đè nhau. Bắt buộc kiểm tra **4 góc**.
- **Quên trường hợp không còn điểm nào** sau khi một góc phủ hết → vẫn phải tính là `true`.
- **Chặt nhị phân lệch biên** (off-by-one): dùng mẫu `while (lo < hi) { mid=(lo+hi)/2; if feasible(mid) hi=mid; else lo=mid+1; }` rồi đáp số là `lo`.

---

## 8. Code mẫu (C++20)

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
vector<long long> U, V;
long long Umin, Umax, Vmin, Vmax;

// Có phủ hết các điểm bằng 2 hình vuông cạnh D (theo toạ độ u,v) không?
bool feasible(long long D) {
    const long long INF = (long long)4e18;
    for (int c = 0; c < 4; c++) {                 // thử 4 góc của hộp bao
        long long umn = INF, umx = -INF, vmn = INF, vmx = -INF;
        bool any = false;
        for (int i = 0; i < n; i++) {
            long long u = U[i], v = V[i];
            bool covered;
            if      (c == 0) covered = (u <= Umin + D && v <= Vmin + D); // góc dưới-trái
            else if (c == 1) covered = (u >= Umax - D && v <= Vmin + D); // dưới-phải
            else if (c == 2) covered = (u <= Umin + D && v >= Vmax - D); // trên-trái
            else             covered = (u >= Umax - D && v >= Vmax - D); // trên-phải
            if (!covered) {                        // điểm chưa được góc này phủ
                any = true;
                umn = min(umn, u); umx = max(umx, u);
                vmn = min(vmn, v); vmx = max(vmx, v);
            }
        }
        if (!any) return true;                      // góc phủ hết -> xong
        if (umx - umn <= D && vmx - vmn <= D) return true; // phần còn lại lọt 1 hình vuông
    }
    return false;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    cin >> n;
    vector<long long> x(n), y(n);
    for (int i = 0; i < n; i++) cin >> x[i];
    for (int i = 0; i < n; i++) cin >> y[i];

    U.resize(n); V.resize(n);
    Umin = Vmin = (long long)4e18; Umax = Vmax = -(long long)4e18;
    for (int i = 0; i < n; i++) {
        U[i] = x[i] + y[i];                         // phép xoay 45 độ: Manhattan -> Chebyshev
        V[i] = x[i] - y[i];
        Umin = min(Umin, U[i]); Umax = max(Umax, U[i]);
        Vmin = min(Vmin, V[i]); Vmax = max(Vmax, V[i]);
    }

    long long lo = 0, hi = max(Umax - Umin, Vmax - Vmin);
    while (lo < hi) {                               // chặt nhị phân trên đáp số
        long long mid = (lo + hi) / 2;
        if (feasible(mid)) hi = mid;
        else               lo = mid + 1;
    }
    cout << lo << "\n";
    return 0;
}
```

> Chú ý đọc nhập: đề cho **dòng riêng cho tất cả $x_i$, rồi dòng riêng cho tất cả $y_i$** (không phải từng cặp $x\ y$ một dòng). Code trên đọc đúng định dạng đó.

---

## 9. Chạy thử ví dụ

Ví dụ đề: $n = 7$, các điểm $(2,4),(10,6),(7,1),(13,5),(5,7),(13,3),(15,5)$. Đáp án **8**.

Đổi sang $(u,v) = (x+y,\ x-y)$:

| Cửa hàng | $(x,y)$ | $u=x+y$ | $v=x-y$ |
|---|---|---|---|
| 1 | (2,4) | 6 | -2 |
| 2 | (10,6) | 16 | 4 |
| 3 | (7,1) | 8 | 6 |
| 4 | (13,5) | 18 | 8 |
| 5 | (5,7) | 12 | -2 |
| 6 | (13,3) | 16 | 10 |
| 7 | (15,5) | 20 | 10 |

Hộp bao: $U \in [6, 20]$, $V \in [-2, 10]$ (biên độ $u = 14$, $v = 12$, đều $> 8$).

Thử $D = 8$, góc **trên-phải (TR)**: phủ các điểm có $u \ge 20-8 = 12$ **và** $v \ge 10-8 = 2$ → đó là cửa hàng 2, 4, 6, 7. Các điểm **còn lại** là 1, 3, 5: $u \in [6, 12]$ (biên độ 6 ≤ 8 ✓), $v \in [-2, 6]$ (biên độ 8 ≤ 8 ✓) → lọt một hình vuông cạnh 8. Vậy `feasible(8) = true`.

Có thể kiểm tra `feasible(7) = false` (chặt nhị phân sẽ tự loại) → đáp số nhỏ nhất là **8**. Đúng đáp án. Cách chia: nhóm $\{1,3,5\}$ và nhóm $\{2,4,6,7\}$ — trùng với gợi ý của đề.

---

## 10. Tóm tắt giải thuật

**Chiêu 1 — chặt nhị phân trên đáp số:** hỏi `feasible(D)` = "chia 2 nhóm, mỗi nhóm đường kính $\le D$ được không?", đơn điệu theo $D$ → chặt nhị phân tìm $D$ nhỏ nhất.

**Chiêu 2 — xoay 45°:** $u = x+y$, $v = x-y$ biến Manhattan thành Chebyshev. "Đường kính $\le D$" $\iff$ "lọt hình vuông cạnh $D$ song song trục $(u,v)$". `feasible(D)` = "phủ hết điểm bằng 2 hình vuông cạnh $D$?".

**Kiểm tra phủ 2 hình vuông:** chỉ cần thử **4 góc** của hộp bao. Mỗi góc: dán 1 hình vuông vào góc, lấy hộp bao các điểm còn lại, kiểm tra biên độ $u, v$ đều $\le D$. (Cắt-bằng-đường-thẳng là **SAI**.)

**Mã giả:**

```
u_i = x_i + y_i;  v_i = x_i - y_i;  tính Umin,Umax,Vmin,Vmax
hàm feasible(D):
    for góc in {BL, BR, TL, TR}:
        gom các điểm KHÔNG bị hình-vuông-dán-góc phủ -> tính hộp bao (umn..umx, vmn..vmx)
        nếu không còn điểm nào: return true
        nếu (umx-umn <= D) và (vmx-vmn <= D): return true
    return false
chặt nhị phân D nhỏ nhất với feasible(D) = true
```

**Độ phức tạp:** $O(n \log C)$ thời gian, $O(n)$ bộ nhớ.

**Bẫy cần nhớ:** $u = x+y$ tới $10^9$, hiệu tới $2\cdot10^9$ → dùng `long long`; đừng dùng cách cắt đường thẳng; nhớ trường hợp một góc phủ hết.

---

## 11. Mở rộng để học sinh suy nghĩ

- Nếu chia thành **$k$ nhóm** ($k > 2$) thì sao? Bài toán phủ bằng $k$ hình vuông trở nên khó hơn nhiều (nói chung là NP-khó khi $k$ tuỳ ý).
- Phép $|a|+|b| = \max(|a+b|,|a-b|)$ và phép xoay $(x+y, x-y)$ xuất hiện ở **rất nhiều** bài Manhattan: tìm cặp xa nhất, cây khung Manhattan, k-điểm gần nhất… Nắm chắc nó là một "vũ khí" mạnh.
