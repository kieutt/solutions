# Lời giải Bài 4 — Ẩm thực Nhật Bản

> Đối tượng: học sinh lớp 10 đã quen **quy tắc nhân, quy tắc cộng** trong đếm và **DP cơ bản**, đang làm quen **số học mô-đun**.
> Đây là bài khó nhất trong bộ: kết hợp **mô hình hoá bài đếm**, **đếm phần bù**, **một quan sát "đa số"** rất đẹp, và **DP đếm theo độ lệch**. Hãy đọc chậm.

---

## 0. Tóm tắt đề bài

Có $n$ cách nấu (đánh số $1..n$) và $m$ nguyên liệu (đánh số $1..m$). Đầu bếp làm đúng $a_{i,j}$ món dùng **cách nấu $i$** và **nguyên liệu chính $j$**. Tổng số món là $A = \sum_{i,j} a_{i,j}$.

Cần đếm số cách chọn một **tập món** (kích thước tuỳ ý) thoả mãn cả ba luật:

1. **Không rỗng**: ít nhất 1 món, $k \ge 1$.
2. **Mọi món trong tập dùng cách nấu khác nhau đôi một** (mỗi cách nấu xuất hiện nhiều nhất 1 lần).
3. **Không nguyên liệu nào chiếm quá nửa**: với mọi nguyên liệu, số món dùng nó làm chính $\le \big\lfloor \tfrac{k}{2} \big\rfloor$ (với $k$ là tổng số món trong tập).

Hai tập khác nhau nếu có ít nhất một món thuộc tập này mà không thuộc tập kia. Vì số cách có thể rất lớn, in **số dư khi chia cho $P = 998\,244\,353$**.

Giới hạn: $1 \le n \le 100$; $1 \le m \le 2000$; $0 \le a_{i,j} \le P$.

---

## 1. Xây dựng mô hình đếm

### 1.1. Mỗi "cách nấu" (hàng) đóng góp nhiều nhất một món

Luật 2 nói các món phải dùng cách nấu khác nhau. Coi $a_{i,j}$ như một **lưới** $n$ hàng (cách nấu) $\times$ $m$ cột (nguyên liệu); ô $(i,j)$ chứa $a_{i,j}$ món **phân biệt được** (chúng được tính là khác nhau). Luật 2 ⟹ **từ mỗi hàng chọn nhiều nhất 1 món.**

Vậy một tập món hợp lệ tương đương với việc:

- chọn một tập hàng $R$ (các cách nấu được dùng), $k = |R|$;
- với **mỗi hàng** $i \in R$, chọn **một cột** $c_i$ và **một món cụ thể** trong số $a_{i, c_i}$ món của ô $(i, c_i)$.

Số cách "đưa hàng $i$ vào tập với một cột nào đó" = tổng cả hàng:

$$
r_i = \sum_{j=1}^{m} a_{i,j}.
$$

(Còn cách "không dùng hàng $i$" có đúng 1 lựa chọn.)

### 1.2. Đếm khi bỏ qua luật 3

Tạm bỏ luật 3, chỉ giữ luật 1 + 2. Mỗi hàng độc lập: "không dùng" (1 cách) hoặc "dùng" ($r_i$ cách). Theo quy tắc nhân, số cách chọn (kể cả tập rỗng) là $\prod_{i=1}^{n} (1 + r_i)$. Trừ đi 1 cho tập rỗng:

$$
\text{Total} = \prod_{i=1}^{n} (1 + r_i) \;-\; 1.
$$

Đây là số tập thoả **luật 1 + 2** (chưa quan tâm luật 3). Phần khó là **loại đi những tập vi phạm luật 3**.

---

## 2. Quan sát "đa số" — chìa khoá của bài

### 2.1. Vi phạm luật 3 nghĩa là một cột chiếm "quá bán"

Trong một tập có $k$ món, gọi $p$ = số món dùng cột $j$ làm chính, và $q = k - p$ = số món dùng cột **khác**. Luật 3 bị vi phạm tại cột $j$ khi $p > \big\lfloor \tfrac{k}{2}\big\rfloor$. Ta chứng minh điều này tương đương:

$$
p > \left\lfloor \frac{k}{2} \right\rfloor \iff p > q.
$$

> *Kiểm tra nhanh:* nếu $p > q$ thì $p \ge q+1$, suy ra $2p \ge p+q+1 = k+1 > k$, nên $p > k/2 \ge \lfloor k/2 \rfloor$. Ngược lại nếu $p > \lfloor k/2 \rfloor$ thì $p \ge \lfloor k/2\rfloor + 1 > k/2$, nên $p > k - p = q$. ∎

Nói cách: cột $j$ vi phạm $\iff$ nó chiếm **đa số tuyệt đối** (nhiều hơn một nửa số món).

### 2.2. Nhiều nhất MỘT cột vi phạm

Vì tổng số món là $k$, **không thể có hai cột cùng chiếm quá nửa** (hai cái cùng $> k/2$ thì tổng $> k$, vô lý). Vậy mỗi tập "xấu" (vi phạm luật 3) có **đúng một** cột tội đồ.

> Đây là quan sát đẹp nhất của bài. Nó cho phép ta **đếm phần bù mà không sợ trùng**: cộng riêng số tập xấu theo từng cột tội đồ, các nhóm này **rời nhau** (vì cột tội đồ là duy nhất), nên chỉ việc cộng lại.

### 2.3. Công thức phần bù

$$
\boxed{\text{Đáp số} = \text{Total} - \sum_{j=1}^{m} \text{Invalid}_j}
$$

với $\text{Invalid}_j$ = số tập (đã thoả luật 1+2) mà **cột $j$ chiếm quá nửa**, tức $p > q$.

---

## 3. Đếm $\text{Invalid}_j$ bằng DP theo "độ lệch"

Cố định một cột $j$. Với mỗi hàng $i$, có **ba** lựa chọn, kèm "trọng số" (số cách) và một thay đổi về độ lệch $p - q$:

| Lựa chọn cho hàng $i$ | Trọng số (số cách) | Ảnh hưởng $p - q$ |
|---|---|---|
| Không dùng hàng $i$ | $1$ | $0$ |
| Dùng, **với cột $j$** (loại J) | $a_{i,j}$ | $+1$ (tăng $p$) |
| Dùng, với **cột khác $j$** (loại O) | $r_i - a_{i,j}$ | $-1$ (tăng $q$) |

(Trọng số loại O là tổng các món của hàng $i$ ở mọi cột $\ne j$, tức $r_i - a_{i,j}$.)

Ta cần **tổng trọng số (tích các lựa chọn) trên mọi cấu hình có $p > q$**, tức **độ lệch $p - q > 0$**.

### 3.1. Định nghĩa DP

Độ lệch $d = p - q$ chạy trong $[-n,\ n]$. Đặt:

$$
dp[d] = \text{tổng (theo mọi cấu hình đạt độ lệch } d) \text{ của tích trọng số các hàng}.
$$

Xử lý từng hàng. Khởi tạo (chưa hàng nào): $dp[0] = 1$, còn lại $0$. Với mỗi hàng $i$ (trọng số loại J là $x = a_{i,j}$, loại O là $y = r_i - a_{i,j}$), cập nhật:

$$
dp_{\text{mới}}[d] = \underbrace{dp[d]\cdot 1}_{\text{bỏ qua}} \;+\; \underbrace{dp[d-1]\cdot x}_{\text{loại J, lệch }+1} \;+\; \underbrace{dp[d+1]\cdot y}_{\text{loại O, lệch }-1}.
$$

Sau khi xử lý hết $n$ hàng:

$$
\text{Invalid}_j = \sum_{d > 0} dp[d].
$$

(Lưu ý $d > 0$ buộc $p \ge 1$, nên tập rỗng không bao giờ bị tính — đúng ý.)

### 3.2. Vì sao DP này đếm đúng?

DP "phân phối nhân" theo từng hàng chính là khai triển tích

$$
\prod_{i=1}^{n} \big(\underbrace{y_i \cdot z^{-1}}_{\text{loại O}} + \underbrace{1}_{\text{bỏ qua}} + \underbrace{x_i \cdot z^{+1}}_{\text{loại J}}\big),
$$

trong đó $z$ là biến "đánh dấu" độ lệch (luỹ thừa của $z$ = $p - q$). Hệ số của $z^d$ chính là tổng trọng số các cấu hình có độ lệch đúng bằng $d$ — đó là $dp[d]$. Ta chỉ lấy phần luỹ thừa **dương** ($d>0$) là ra $\text{Invalid}_j$. (Học sinh chưa quen "đa thức sinh" cứ hiểu mộc mạc: DP cộng dồn mọi cách chọn và nhóm theo độ lệch.)

---

## 4. Số học mô-đun (mod $P$)

Mọi phép cộng và nhân ở trên đều thực hiện **modulo $P = 998\,244\,353$** (một số nguyên tố). Lý do dùng được: số dư có tính chất

$$
(a+b) \bmod P,\quad (a\cdot b)\bmod P
$$

tính được từ $a \bmod P$ và $b \bmod P$ — nên cứ lấy dư sau mỗi phép là kết quả cuối vẫn đúng. Vài lưu ý:

- Đọc $a_{i,j}$ xong **lấy dư ngay** ($a_{i,j}$ có thể bằng $P$, khi đó $\equiv 0$).
- Phép nhân hai số $< P$ (cỡ $10^9$) cho tích cỡ $10^{18}$ → phải dùng `long long`.
- Phép trừ trong mô-đun: $a - b$ có thể âm, cộng thêm $P$ rồi mới lấy dư: `(a - b + P) % P`.

> Bài này **không cần** chia mô-đun (không cần nghịch đảo), chỉ cần cộng/trừ/nhân — nhẹ nhàng cho học sinh mới học mod.

---

## 5. Tổng hợp thuật toán

1. Đọc lưới, lấy dư mọi $a_{i,j}$. Tính $r_i = \sum_j a_{i,j} \bmod P$ cho từng hàng.
2. $\text{Total} = \big(\prod_i (1 + r_i) - 1\big) \bmod P$.
3. Với **mỗi cột** $j = 1..m$: chạy DP độ lệch (mục 3) để tính $\text{Invalid}_j = \sum_{d>0} dp[d]$.
4. Đáp số $= \big(\text{Total} - \sum_j \text{Invalid}_j\big) \bmod P$ (nhớ cộng $P$ trước khi lấy dư để tránh âm).

---

## 6. Độ phức tạp

- Mỗi cột: DP qua $n$ hàng, mảng độ lệch rộng $2n+1$ → $O(n^2)$ cho một cột.
- $m$ cột → **thời gian $O(n^2 m)$**. Với $n=100$, $m=2000$: $\approx 100^2 \cdot 2000 = 2\cdot10^7$ (nhân thêm hằng số nhỏ) → chạy dưới 0,2 giây.
- Bộ nhớ: $O(n m)$ để lưu lưới + $O(n)$ cho mảng DP.

> *Lưu ý ước lượng:* nhiều bạn nhầm thành $O(n^2 m)$ là $2\cdot10^9$ rồi tưởng TLE. Tính kỹ: mảng độ lệch chỉ rộng $\approx 2n = 200$, mỗi cột $\approx n \cdot 2n = 2\cdot10^4$ phép, nhân $m = 2000$ ra $\approx 4\cdot10^7$ — hoàn toàn ổn.

---

## 7. Trường hợp biên

- **$n = 1$ (một cách nấu):** mọi tập 1 món có $k=1$, $\lfloor 1/2\rfloor = 0$, mà nguyên liệu của món đó được dùng 1 lần $> 0$ → **luôn vi phạm**. Đáp số $0$. Công thức tự cho: $\text{Total} = r_1$, và $\sum_j \text{Invalid}_j = \sum_j a_{1,j} = r_1$ → hiệu bằng $0$. ✓
- **$a_{i,j} = P$:** lấy dư thành $0$ → ô đó đóng góp $0$ cách (đúng tinh thần đếm mô-đun).
- **Tất cả $a_{i,j} = 0$:** $\text{Total}=0$ → đáp số $0$.

---

## 8. Lỗi học sinh hay mắc

- **Quên lấy dư khi đọc $a_{i,j}$** (vì $a_{i,j}$ có thể $= P$).
- **Tràn số khi nhân.** Hai số cỡ $10^9$ nhân nhau cỡ $10^{18}$ → bắt buộc `long long`; nếu lỡ để `int` là sai ngầm.
- **Trừ ra số âm trong mô-đun.** `(Total - sum) % P` có thể âm; phải `(Total - sum % P + P) % P`.
- **Đếm trùng tập xấu.** Nếu quên quan sát "mỗi tập xấu có đúng 1 cột tội đồ" mà đi bao hàm–loại trừ lằng nhằng → vừa khó vừa dễ sai. Quan sát ở mục 2.2 làm mọi thứ rời nhau, chỉ cần **cộng**.
- **Nhầm $\lfloor k/2\rfloor$ thành $k/2$.** Với $k$ lẻ, ngưỡng là phần nguyên dưới; nhưng đã quy về "$p > q$" nên không phải lo dấu sàn nữa — đây là lý do nên đổi sang điều kiện $p>q$ cho gọn.
- **Ước lượng độ phức tạp sai** rồi bỏ cuộc tưởng TLE (xem mục 6).

---

## 9. Code mẫu (C++20)

```cpp
#include <bits/stdc++.h>
using namespace std;

const long long MOD = 998244353;

int main() {
    int n, m;
    scanf("%d %d", &n, &m);

    vector<vector<long long>> a(n, vector<long long>(m));
    vector<long long> r(n, 0);                       // r[i] = tổng hàng i (mod P)
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++) {
            long long x; scanf("%lld", &x);
            x %= MOD;                                // a[i][j] có thể = P -> lấy dư ngay
            a[i][j] = x;
            r[i] = (r[i] + x) % MOD;
        }

    // Total = tích(1 + r_i) - 1  : số tập thoả luật 1 + 2 (chưa xét luật 3)
    long long total = 1;
    for (int i = 0; i < n; i++) total = total * ((1 + r[i]) % MOD) % MOD;
    total = (total - 1 + MOD) % MOD;

    // Với mỗi cột, đếm số tập mà cột đó CHIẾM QUÁ NỬA (p > q) bằng DP độ lệch
    int sz = 2 * n + 1;                              // độ lệch p-q, offset +n: chỉ số 0..2n
    vector<long long> dp(sz), ndp(sz);
    long long invalidSum = 0;
    for (int j = 0; j < m; j++) {
        for (int b = 0; b < sz; b++) dp[b] = 0;
        dp[n] = 1;                                   // độ lệch = 0 (chưa hàng nào)
        for (int i = 0; i < n; i++) {
            long long x = a[i][j];                   // loại J: dùng cột j  -> +1
            long long y = (r[i] - a[i][j] + MOD) % MOD; // loại O: dùng cột khác -> -1
            for (int b = 0; b < sz; b++) ndp[b] = 0;
            for (int b = 0; b < sz; b++) {
                long long v = dp[b];
                if (!v) continue;
                ndp[b] = (ndp[b] + v) % MOD;                         // bỏ qua hàng
                if (b + 1 < sz) ndp[b + 1] = (ndp[b + 1] + v * x) % MOD; // dùng cột j
                if (b - 1 >= 0) ndp[b - 1] = (ndp[b - 1] + v * y) % MOD; // dùng cột khác
            }
            swap(dp, ndp);
        }
        long long inv = 0;
        for (int b = n + 1; b < sz; b++) inv = (inv + dp[b]) % MOD;  // độ lệch > 0
        invalidSum = (invalidSum + inv) % MOD;
    }

    long long ans = (total - invalidSum % MOD + MOD) % MOD;
    printf("%lld\n", ans);
    return 0;
}
```

---

## 10. Chạy thử ví dụ

Ví dụ 1 của đề:

```
2 3
1 0 1
0 1 1
```

- $r_1 = 1+0+1 = 2$, $r_2 = 0+1+1 = 2$.
- $\text{Total} = (1+2)(1+2) - 1 = 9 - 1 = 8$.

Đếm $\text{Invalid}_j$ (số tập mà cột $j$ chiếm quá nửa). Với $n=2$, cấu hình "xấu" là $(p=1,q=0)$ hoặc $(p=2,q=0)$:

- **Cột 1** ($a_{1,1}=1, a_{2,1}=0$): $(p{=}1,q{=}0)$ cho $1\cdot$(hàng 2 không dùng) $+ 0 = 1$; $(p{=}2,q{=}0)$ cho $1\cdot 0 = 0$. $\Rightarrow \text{Invalid}_1 = 1$.
- **Cột 2** ($a_{1,2}=0, a_{2,2}=1$): đối xứng $\Rightarrow \text{Invalid}_2 = 1$.
- **Cột 3** ($a_{1,3}=1, a_{2,3}=1$): $(p{=}1)$ cho $1+1 = 2$; $(p{=}2)$ cho $1\cdot 1 = 1$. $\Rightarrow \text{Invalid}_3 = 3$.

$\sum_j \text{Invalid}_j = 1 + 1 + 3 = 5$.

$$
\text{Đáp số} = \text{Total} - 5 = 8 - 5 = 3.
$$

Đúng đáp án mẫu **3**. (Hai ví dụ còn lại của đề cho $190$ và $742$ — kiểm chứng bằng code.)

---

## 11. Tóm tắt giải thuật

**Mô hình:** mỗi cách nấu (hàng) đóng góp $\le 1$ món; "đưa hàng $i$ vào" có $r_i = \sum_j a_{i,j}$ cách. Số tập thoả luật 1+2 là $\text{Total} = \prod_i (1+r_i) - 1$.

**Phần bù:** một tập vi phạm luật 3 $\iff$ có một cột chiếm **quá nửa**; mà **nhiều nhất một** cột làm được điều đó ⟹ các tập xấu rời nhau theo cột tội đồ. Vậy
$$\text{Đáp số} = \text{Total} - \sum_j \text{Invalid}_j.$$

**Đếm $\text{Invalid}_j$:** với cột $j$, mỗi hàng có 3 lựa chọn gắn độ lệch ($+1$ nếu dùng cột $j$, $-1$ nếu dùng cột khác, $0$ nếu bỏ qua). DP theo độ lệch $p-q$; lấy tổng phần độ lệch $> 0$.

**Mã giả:**

```
đọc lưới (lấy dư);  r[i] = tổng hàng i
Total = tích(1 + r[i]) - 1
for cột j:
    dp[độ lệch 0] = 1
    for hàng i:
        x = a[i][j] (loại J, +1);  y = r[i]-a[i][j] (loại O, -1)
        dp'[d] = dp[d]*1 + dp[d-1]*x + dp[d+1]*y
        dp = dp'
    Invalid_j = tổng dp[d] với d > 0
Đáp số = (Total - tổng Invalid_j) mod P
```

**Độ phức tạp:** thời gian $O(n^2 m) \approx 4\cdot10^7$, bộ nhớ $O(nm)$.

**Bẫy cần nhớ:** lấy dư khi đọc ($a_{i,j}$ có thể $=P$); nhân dùng `long long`; trừ mô-đun cộng thêm $P$; đừng đếm trùng (nhờ quan sát "một cột tội đồ").

---

## 12. Mở rộng để học sinh suy nghĩ

- Quan sát "nhiều nhất một phần tử chiếm quá nửa" gắn liền với thuật toán **Boyer–Moore Majority Vote** (tìm phần tử đa số) — cùng một sự thật toán học, ứng dụng khác nhau.
- Kỹ thuật **DP theo hiệu hai đại lượng** ($p - q$) và **đếm phần bù** xuất hiện thường xuyên trong các bài đếm có ràng buộc "không quá một nửa", "ngoặc hợp lệ", "đường đi không vượt đường chéo"… Nắm chắc cặp công cụ này.
