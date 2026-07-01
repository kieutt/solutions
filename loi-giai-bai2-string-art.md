# Lời giải Bài 2 — "Stринг-арт" (Nghệ thuật giăng chỉ)

> Đối tượng: học sinh lớp 10 đã quen mảng, vòng lặp, và **mới bắt đầu học Quy hoạch động (DP)**.
> Đây là một bài DP "kinh điển" rất đáng học, vì nó dạy ta cách **chọn trạng thái** — kỹ năng khó nhất khi mới học DP.

---

## 0. Tóm tắt đề bài

Có $n$ chiếc đinh đánh số $1..n$ theo thứ tự từ trái sang phải, đinh thứ $i$ ở toạ độ $(x_i, y_i)$, với $x_1 \le x_2 \le \dots \le x_n$.

Người hoạ sĩ dùng **2 sợi chỉ**, ban đầu **cả hai đều buộc vào đinh 1**. Yêu cầu:

- **Mỗi sợi chỉ chỉ được buộc tiến về phía sau:** nếu lần cuối một sợi buộc ở đinh $i$, lần tiếp theo nó chỉ được buộc vào đinh có số $> i$. Nói cách khác, dãy đinh trên mỗi sợi có **số tăng nghiêm ngặt**.
- **Mọi đinh phải được dùng ít nhất một lần** (có nút của sợi 1, hoặc sợi 2, hoặc cả hai trên đinh đó).
- **Tổng độ dài chỉ** (tổng khoảng cách giữa các nút liên tiếp trên cả 2 sợi) **nhỏ nhất**.

Khoảng cách là khoảng cách Euclid: $\mathrm{dist}(i,j) = \sqrt{(x_i-x_j)^2 + (y_i-y_j)^2}$.

In ra tổng độ dài nhỏ nhất (sai số $10^{-6}$ chấp nhận được). Giới hạn: $2 \le n \le 2000$.

---

## 1. Dịch đề sang ngôn ngữ dễ hình dung

Hãy quên "chỉ" và "đinh" đi, thay bằng hình ảnh quen hơn:

> Có **2 người đi bộ** cùng xuất phát ở **thành phố 1**. Cả hai chỉ được đi tới thành phố có **số lớn hơn** chỗ đang đứng. Hai người **chia nhau** sao cho **mọi thành phố đều có ít nhất một người ghé qua**. Tổng quãng đường hai người đi là **nhỏ nhất**.

Vì mỗi người chỉ đi tiến (số tăng dần), **lộ trình mỗi người là một dãy thành phố tăng dần**. Hai dãy này gộp lại phải phủ hết $1..n$. Đây là bài **"hai lữ khách cùng phủ mọi thành phố"**.

Một câu hỏi tự nhiên: sao không tham lam? Ví dụ "mỗi đinh giao cho người gần nó hơn". **Tham lam sai**, vì quyết định giao đinh $k$ cho người nào còn ảnh hưởng tới chi phí của các đinh **sau** $k$ — ta chưa biết tương lai. Khi một lựa chọn hiện tại ảnh hưởng tương lai và ta muốn tối ưu toàn cục → nghĩ tới **Quy hoạch động**.

---

## 2. Xây dựng mô hình — chọn trạng thái cho DP

DP khó nhất ở chỗ: **trạng thái cần ghi lại đúng những gì ảnh hưởng tương lai, và không ghi dư.**

Quan sát: để đi tiếp, chi phí các bước sau **chỉ phụ thuộc vào vị trí hiện tại của hai người**. Lịch sử họ đã đi qua đâu thì **không còn quan trọng** — vì các thành phố tương lai đều có số lớn hơn, và bước kế tiếp chỉ tính khoảng cách từ chỗ người đó đang đứng.

Vậy trạng thái chỉ cần **hai vị trí hiện tại** của hai người. Gọi đó là cặp $(i, j)$.

### 2.1. Nhận xét then chốt: luôn có một người đứng ở "biên"

Ta xây lời giải bằng cách **thêm đinh theo đúng thứ tự** $1, 2, 3, \dots$ Giả sử đã phủ xong các đinh $1..k$ và hai người đang ở $i, j$. Khi đó **bắt buộc**:

$$
\max(i, j) = k.
$$

Vì sao?

- Nếu **cả hai** vị trí đều $< k$ thì đinh $k$ **chưa ai ghé** → mâu thuẫn với "đã phủ tới $k$".
- Nếu **một** vị trí $> k$ thì người đó đã ghé một đinh số lớn hơn $k$, mà muốn ghé đinh đó thì trước đó đã phải phủ qua $k$ rồi (vì chỉ đi tiến) — nhưng ta đang nói "vừa phủ tới $k$" → vẫn mâu thuẫn.

Kết luận: **một trong hai người luôn đang đứng đúng ở đinh $k$ (đinh vừa phủ), người kia đứng ở một đinh nào đó $\le k$.**

### 2.2. Đinh tiếp theo cần thêm là duy nhất

Vì chỉ được đi tiến và ta phủ theo thứ tự, sau khi phủ xong $1..k$, **đinh chưa phủ nhỏ nhất chắc chắn là $k+1$** (không thể "nhảy cóc" qua $k+1$ để phủ đinh xa hơn trước). Điều này biến bài từ "mũ" (thử mọi cách giao đinh) thành DP **bậc hai** — đây là vẻ đẹp của bài.

### 2.3. Định nghĩa DP

$$
dp[i][j] = \text{tổng độ dài nhỏ nhất khi đã phủ xong các đinh } 1..\max(i,j), \text{ hai đầu sợi đang ở } i \text{ và } j.
$$

Vì hai sợi là **như nhau** (đổi vai không ảnh hưởng), ta coi $dp[i][j] = dp[j][i]$ và quy ước lưu với $\max$ là một chỉ số.

**Khởi tạo:** $dp[1][1] = 0$ — cả hai cùng ở đinh 1, chưa đi đâu, phủ đúng đinh 1.

---

## 3. Công thức chuyển trạng thái (transition)

Đặt $k = \max(i,j) + 1$ là đinh tiếp theo cần thêm. Nếu $k \le n$, có **đúng 2 cách** thêm đinh $k$ (một trong hai người vươn tới nó):

- **Người đang ở $i$ đi tới $k$:** trạng thái mới $(k, j)$, chi phí cộng thêm $\mathrm{dist}(i, k)$:
  $$
  dp[k][j] \;\leftarrow\; \min\big(dp[k][j],\; dp[i][j] + \mathrm{dist}(i, k)\big).
  $$
- **Người đang ở $j$ đi tới $k$:** trạng thái mới $(i, k)$, chi phí cộng thêm $\mathrm{dist}(j, k)$:
  $$
  dp[i][k] \;\leftarrow\; \min\big(dp[i][k],\; dp[i][j] + \mathrm{dist}(j, k)\big).
  $$

Vì mỗi bước $\max$ tăng **đúng 1 đơn vị**, mọi phụ thuộc đều "hướng về phía trước" (từ $\max = m$ sang $\max = m+1$), **không có vòng lặp quẩn** → ta xử lý trạng thái theo thứ tự $\max(i,j)$ tăng dần là an toàn.

### 3.1. Vì sao bấy nhiêu là đủ?

Câu hỏi học sinh hay băn khoăn: "Sao chỉ nhớ 2 vị trí mà không nhớ lịch sử?" Lý do (chính là phần *invariant* của DP):

- Mọi thành phố tương lai đều có số $> k$, nên chi phí các bước sau **không liên quan** tới việc trước đó ai đã đi qua đinh nào.
- Chi phí bước kế tiếp **chỉ** phụ thuộc vị trí hiện tại của người thực hiện bước đó.

Do đó $dp[i][j]$ chứa **đủ** thông tin để đi tiếp tối ưu. Đây là tính chất **"bài toán con tối ưu" + "không hậu hiệu" (no aftereffect)** — nền tảng của mọi DP.

---

## 4. Đáp số

Khi đã phủ hết, $\max(i,j) = n$, tức **một trong hai người đang đứng ở đinh $n$** (đinh cuối, số lớn nhất, chắc chắn phải có người ghé). Đáp số là:

$$
\text{answer} = \min_{1 \le j \le n} dp[n][j].
$$

(Hai sợi không bắt buộc kết thúc ở cùng một đinh; người kia đứng ở $j$ bất kỳ.)

---

## 5. Độ phức tạp

- **Số trạng thái:** cặp $(\max, \text{người kia})$ — $\max$ chạy $1..n$, người kia $1..\max$ → khoảng $\dfrac{n^2}{2}$ trạng thái → $O(n^2)$.
- **Mỗi trạng thái** xử lý $O(1)$ (2 phép chuyển) → **thời gian $O(n^2)$.** Với $n = 2000$ là $\approx 4 \cdot 10^6$ phép — chạy trong vài chục mili-giây.
- **Bộ nhớ:** bảng $dp$ kích thước $n \times n$ kiểu `double` $\approx 32$ MB (vừa giới hạn 256 MB). Nếu muốn tiết kiệm: vì chỉ chuyển từ $\max = m$ sang $m+1$, ta chỉ cần **2 hàng** → bộ nhớ $O(n)$ (xem mục 8.2).

---

## 6. Trường hợp biên

- **$n = 2$:** chỉ có đinh 1 và 2. Một sợi đi $1 \to 2$ (dài $\mathrm{dist}(1,2)$), sợi kia ở yên đinh 1 (dài 0). Đáp số $= \mathrm{dist}(1,2)$. Công thức tự cho ra điều này.
- **Mọi đinh thẳng hàng ($y_i = 0$):** $\mathrm{dist} = |x_i - x_j|$. Cấu trúc lời giải **không đổi**, chỉ là khoảng cách đơn giản hơn (đây là subtask 4 — dùng để test nhanh).
- **Toạ độ trùng nhau / nhiều đinh cùng vị trí:** khoảng cách $= 0$, vẫn xử lý đúng.

---

## 7. Lỗi học sinh hay mắc

- **Tràn số khi tính khoảng cách.** $x_i$ tới $10^9$, hiệu tới $2\cdot10^9$, **bình phương** tới $4\cdot10^{18}$. Nếu để `int` thì tràn ngay. Phải tính hiệu bằng `long long`, bình phương bằng `long long` (tổng tới $8\cdot10^{18}$ vẫn lọt `long long` $\approx 9.2\cdot10^{18}$), **rồi mới** ép `double` để lấy `sqrt`.
- **Quên cả hai sợi cùng bắt đầu ở đinh 1** → quên $dp[1][1]=0$ hoặc khởi tạo sai.
- **Nhầm thành bài người bán hàng (TSP).** Đây **không** phải TSP: vì chỉ đi tiến nên bài có lời giải đa thức $O(n^2)$. Đừng quay lui mũ.
- **Lẫn lộn 0-index / 1-index.** Bài cho đinh đánh số từ 1; nên dùng 1-index để khỏi nhầm.
- **Dùng `endl`** trong vòng lặp lớn ở các bài khác → flush liên tục → TLE. Tập thói quen dùng `"\n"`.
- **In thiếu chữ số thập phân.** Dùng `setprecision(10)` (hoặc hơn) với `fixed`. Sai số cho phép $10^{-6}$ nên 10 chữ số là dư an toàn.

---

## 8. Code mẫu (C++20)

### 8.1. Bản dễ đọc (bảng 2 chiều)

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
long long X[2005], Y[2005];

double dist(int a, int b) {                 // a, b là chỉ số đinh (1-based)
    long long dx = X[a] - X[b], dy = Y[a] - Y[b];
    return sqrt((double)(dx * dx + dy * dy)); // tính bình phương bằng long long rồi mới đổi double
}

static double dp[2005][2005];               // dp[i][j]: đã phủ 1..max(i,j), hai đầu sợi ở i, j

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    cin >> n;
    for (int i = 1; i <= n; i++) cin >> X[i] >> Y[i];

    for (int i = 0; i <= n; i++)
        for (int j = 0; j <= n; j++) dp[i][j] = 1e18;
    dp[1][1] = 0;                            // cả hai sợi xuất phát ở đinh 1

    for (int frontier = 1; frontier < n; frontier++) {
        int k = frontier + 1;                // đinh tiếp theo cần thêm là duy nhất
        for (int j = 1; j <= frontier; j++) {
            double cur = dp[frontier][j];
            if (cur >= 1e18) continue;       // trạng thái chưa với tới
            // sợi đang ở frontier đi tới k:
            dp[k][j] = min(dp[k][j], cur + dist(frontier, k));
            // sợi đang ở j đi tới k (đầu kia trở thành frontier cũ):
            dp[k][frontier] = min(dp[k][frontier], cur + dist(j, k));
        }
    }

    double ans = 1e18;
    for (int j = 1; j <= n; j++) ans = min(ans, dp[n][j]); // một sợi phải ở đinh n
    cout << fixed << setprecision(10) << ans << "\n";
    return 0;
}
```

> Lưu ý: khai báo `dp` là mảng **toàn cục/`static`** (không phải mảng cục bộ trong `main`) để tránh tràn ngăn xếp (stack) với kích thước $\approx 32$ MB.

### 8.2. Bản tiết kiệm bộ nhớ $O(n)$ (cuốn mảng)

Vì chỉ chuyển từ `frontier = m` sang `m+1`, ta giữ hai hàng: `cur` $= dp[m][\cdot]$ và `nxt` $= dp[m+1][\cdot]$.

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
vector<long long> X, Y;

inline double dist(int a, int b) {
    long long dx = X[a] - X[b], dy = Y[a] - Y[b];
    return sqrt((double)(dx * dx + dy * dy));
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    cin >> n;
    X.resize(n + 1); Y.resize(n + 1);
    for (int i = 1; i <= n; i++) cin >> X[i] >> Y[i];

    const double INF = 1e18;
    vector<double> cur(n + 1, INF), nxt(n + 1, INF);
    cur[1] = 0.0;                            // dp[1][1] = 0

    for (int m = 1; m < n; m++) {
        int k = m + 1;
        for (int i = 0; i <= n; i++) nxt[i] = INF;
        double dmk = dist(m, k);
        for (int i = 1; i <= m; i++) {
            double cv = cur[i];
            if (cv >= INF) continue;
            if (cv + dmk < nxt[i]) nxt[i] = cv + dmk;        // sợi ở frontier m -> k
            double t = cv + dist(i, k);                       // sợi kia i -> k
            if (t < nxt[m]) nxt[m] = t;                       // đầu kia thành m (frontier cũ)
        }
        swap(cur, nxt);                                       // cur = dp[k][*]
    }

    double ans = INF;
    for (int i = 1; i <= n; i++) ans = min(ans, cur[i]);
    cout << fixed << setprecision(10) << ans << "\n";
    return 0;
}
```

Hai bản cho **cùng kết quả**. Bản 8.1 dễ hiểu hơn để học; bản 8.2 gọn bộ nhớ, nên dùng khi giới hạn chặt.

> Nếu lo về chữ số cuối do làm tròn `double` (bài này cộng dồn tới 2000 căn bậc hai), có thể đổi toàn bộ `double` → `long double` và `sqrt` → `sqrtl`. Với $n \le 2000$ thời gian không đáng kể. (Sai số `double` ở bài này vẫn xa dưới ngưỡng $10^{-6}$.)

---

## 9. Chạy thử ví dụ

Lấy ví dụ 2 của đề: 3 đinh $(0,0), (1,10), (2,0)$.

- $\mathrm{dist}(1,2) = \sqrt{1^2 + 10^2} = \sqrt{101} \approx 10{,}0499$
- $\mathrm{dist}(2,3) = \sqrt{1^2 + 10^2} = \sqrt{101} \approx 10{,}0499$
- $\mathrm{dist}(1,3) = \sqrt{2^2 + 0^2} = 2$

Diễn tiến DP:

1. $dp[1][1] = 0$.
2. Thêm đinh 2 ($\max: 1 \to 2$): từ $dp[1][1]$, một người đi $1 \to 2$ → $dp[2][1] = \sqrt{101}$.
3. Thêm đinh 3 ($\max: 2 \to 3$), từ $dp[2][1]$ (hai người ở đinh 2 và 1):
   - người ở đinh 2 đi tới 3: $dp[3][1] = \sqrt{101} + \sqrt{101} = 2\sqrt{101} \approx 20{,}0998$;
   - người ở đinh 1 đi tới 3: $dp[3][2] = \sqrt{101} + 2 \approx 12{,}0499$.
4. Đáp số $= \min(dp[3][1], dp[3][2]) = 12{,}0499$.

Khớp đáp án mẫu **12.0498756211**. Ý nghĩa: một sợi đi $1\to2$, sợi kia đi $1\to3$ — chia việc thông minh hơn là để một sợi gánh hết $1\to2\to3$.

---

## 10. Tóm tắt giải thuật

**Mô hình:** 2 lữ khách xuất phát ở thành phố 1, chỉ đi tiến (số tăng), cùng phủ mọi thành phố, tổng quãng đường nhỏ nhất.

**Trạng thái:** $dp[i][j]$ = chi phí nhỏ nhất khi đã phủ $1..\max(i,j)$, hai người ở $i, j$.

**Bất biến (mấu chốt):** sau khi phủ $1..k$ thì luôn $\max(i,j) = k$, và **đinh kế tiếp cần thêm là duy nhất** $= k+1$. Nhờ đó DP chỉ $O(n^2)$ thay vì mũ.

**Chuyển trạng thái** với $k = \max(i,j)+1$:
- $dp[k][j] \leftarrow \min(dp[k][j],\, dp[i][j] + \mathrm{dist}(i,k))$
- $dp[i][k] \leftarrow \min(dp[i][k],\, dp[i][j] + \mathrm{dist}(j,k))$

**Khởi tạo:** $dp[1][1]=0$. **Đáp số:** $\min_j dp[n][j]$.

**Mã giả:**

```
dp[*][*] = +vô cùng;  dp[1][1] = 0
for frontier = 1 .. n-1:
    k = frontier + 1
    for j = 1 .. frontier:           # trạng thái (frontier, j), với frontier = max
        if dp[frontier][j] vô cùng: bỏ qua
        dp[k][j]        = min(dp[k][j],        dp[frontier][j] + dist(frontier, k))
        dp[k][frontier] = min(dp[k][frontier], dp[frontier][j] + dist(j, k))
answer = min over j của dp[n][j]
```

**Độ phức tạp:** thời gian $O(n^2)$, bộ nhớ $O(n^2)$ (hoặc $O(n)$ nếu cuốn mảng).

**Bẫy cần nhớ:** bình phương khoảng cách phải dùng `long long` rồi mới đổi `double`; nhớ $dp[1][1]=0$; đây không phải TSP; in đủ chữ số thập phân (`fixed << setprecision(10)`).

---

## 11. Mở rộng để học sinh suy nghĩ

- Nếu có **$3$ sợi chỉ** thì sao? Trạng thái thành $dp[i][j][\ell]$ với $\max = $ một trong ba → $O(n^3)$. Tổng quát $t$ sợi → $O(n^t)$.
- Đây là họ hàng của bài **"Bitonic tour"** (hành trình đi sang phải rồi quay về trái) — cùng ý tưởng "hai nhánh đi tiến". Tìm hiểu thêm để thấy một khuôn mẫu DP rất hay gặp.
