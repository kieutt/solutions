# Lời giải Bài 1 — Trò chơi cổ xưa

> Đối tượng: học sinh lớp 10 vừa học xong phần cơ bản, đang bước vào thuật toán nâng cao.
> Mục tiêu: nghe xong là hiểu vì sao đúng và **tự code lại được ngay**.

---

## 0. Tóm tắt đề bài

Cho một bảng $n \times m$. Ô ở hàng $i$, cột $j$ đang có $a_{i,j}$ quân cờ ($0 \le a_{i,j} \le 10^9$).

**Luật cần thoả:** với **mọi cặp ô kề cạnh** (chung một cạnh, tức cách nhau đúng 1 ô theo hàng hoặc cột), **tổng số quân của hai ô đó phải là số lẻ**.

Người chơi thứ nhất được phép **thêm** quân cờ (mỗi lần đặt thêm đúng 1 quân vào một ô bất kỳ, làm bao nhiêu lần tuỳ ý — chỉ thêm, **không được bớt**). Sau khi chỉnh, ta cần một bảng:

1. **Tổng số quân nhỏ nhất có thể**;
2. Nếu nhiều bảng cùng tổng nhỏ nhất → chọn bảng **nhỏ nhất theo thứ tự từ điển** (đọc bảng thành một dãy số: hàng 1 trước, rồi hàng 2, …; so sánh như so sánh chuỗi).

In ra bảng cuối cùng.

Giới hạn: $1 \le n, m \le 100$, nên bảng có tối đa $10^4$ ô — rất nhỏ. Điều đó là một gợi ý: **bài này không nằm ở việc tối ưu tốc độ, mà nằm ở việc nhìn ra bản chất toán học.**

---

## 1. Xây dựng mô hình toán học

### 1.1. "Tổng lẻ" nghĩa là gì?

Đây là bước quan trọng nhất. Hãy nhớ một sự thật rất nhỏ về chẵn/lẻ:

$$
x + y \text{ lẻ} \iff x, y \text{ khác tính chẵn lẻ (một chẵn, một lẻ)}.
$$

Vì sao? Vì chẵn + chẵn = chẵn, lẻ + lẻ = chẵn, chỉ có chẵn + lẻ = lẻ. Vậy điều kiện "tổng hai ô kề là lẻ" **không quan tâm giá trị cụ thể**, nó chỉ nói: **hai ô kề nhau phải khác tính chẵn lẻ.**

> Bài học rút ra: gặp điều kiện về "tổng lẻ / tổng chẵn", hãy lập tức dịch sang ngôn ngữ **parity** (tính chẵn lẻ), tức là chỉ giữ lại bit cuối cùng $a_{i,j} \bmod 2$. Mọi thứ còn lại của con số là nhiễu.

### 1.2. Tô bảng như bàn cờ vua

Điều kiện "hai ô kề khác parity" giống hệt cách tô màu bàn cờ vua: ô đen cạnh ô trắng, ô trắng cạnh ô đen, không bao giờ hai ô cùng màu nằm cạnh nhau.

Hãy thử "lan truyền" parity từ ô $(0,0)$:

- Giả sử $(0,0)$ là **chẵn**. Thì 2 ô kề nó — là $(0,1)$ và $(1,0)$ — phải **lẻ**.
- Tới lượt $(0,2)$ kề $(0,1)$ (đang lẻ) nên phải **chẵn**; $(1,1)$ kề $(0,1)$ và $(1,0)$ (đều lẻ) nên phải **chẵn**; …

Cứ thế lan ra, ta thấy parity của một ô bị **ép cứng** theo công thức:

$$
\text{parity của } (i,j) \;=\; (i + j) \bmod 2 \quad(\text{nếu } (0,0) \text{ chẵn}).
$$

Nghĩa là: hai ô có $(i+j)$ cùng tính chẵn lẻ thì cùng màu; khác thì khác màu. Đây chính là "bàn cờ".

### 1.3. Chỉ có đúng **HAI** bảng hợp lệ về parity

Toàn bộ parity của bảng bị quyết định **chỉ bởi parity của một ô duy nhất** — chẳng hạn ô $(0,0)$. Mà ô $(0,0)$ chỉ có 2 lựa chọn (chẵn hoặc lẻ), nên **chỉ tồn tại đúng 2 "mẫu" parity hợp lệ**.

**Mẫu A** — ô $(i,j)$ có parity $(i+j)\bmod 2$ (ô $(0,0)$ chẵn):

$$
\begin{matrix}
C & L & C & L \\
L & C & L & C \\
C & L & C & L
\end{matrix}
\qquad (C=\text{chẵn},\ L=\text{lẻ})
$$

**Mẫu B** — ngược lại hoàn toàn, parity $(i+j+1)\bmod 2$ (ô $(0,0)$ lẻ):

$$
\begin{matrix}
L & C & L & C \\
C & L & C & L \\
L & C & L & C
\end{matrix}
$$
> **Một câu hỏi tinh tế:** liệu có khi nào *không tô được* không (như đồ thị có chu trình lẻ thì không tô 2 màu được)? **Không.** Lưới ô vuông luôn "2 màu hoá" được theo $(i+j)\bmod 2$ — nói theo ngôn ngữ đồ thị, lưới là **đồ thị hai phía (bipartite)**, không có chu trình lẻ. Vì vậy cả hai mẫu A, B **luôn khả thi**. Đây là lý do bài không cần xét trường hợp "vô nghiệm".

**Kết luận của mô hình:** bảng cuối cùng bắt buộc là một trong hai mẫu bàn cờ A hoặc B. Bài toán "vô số cách thêm cờ" đã thu lại còn **đúng 2 ứng viên**. Phần còn lại chỉ là *chọn cái nào* và *điền số bao nhiêu*.

---

## 2. Khai thác ràng buộc "chỉ được THÊM"

Ta đã chốt được parity mong muốn của từng ô (theo mẫu A hoặc B). Bây giờ với một ô, giá trị cuối cùng phải:

- có **đúng parity mục tiêu**, và
- **không nhỏ hơn** $a_{i,j}$ (vì chỉ được thêm).

Giá trị nhỏ nhất thoả cả hai điều kiện đó là:

- Nếu $a_{i,j}$ **đã đúng** parity mục tiêu → giữ nguyên $a_{i,j}$ (thêm 0 quân).
- Nếu $a_{i,j}$ **sai** parity → thêm **đúng 1** quân, thành $a_{i,j}+1$ (thêm 1 lần là đổi được chẵn↔lẻ, không cần thêm 2; thêm 2 chỉ làm tổng to ra vô ích).

> Ý quan trọng: với một mẫu đã chọn, **bảng kết quả tối thiểu là duy nhất** — mỗi ô chỉ có một giá trị nhỏ nhất hợp lệ. Không có chuyện "thêm nhiều cho đẹp". Thêm dư luôn làm tổng lớn hơn → loại.

Vậy **chi phí (số quân phải thêm)** cho một ô là:

$$
\text{cost}(i,j) =
\begin{cases}
0 & \text{nếu } a_{i,j}\bmod 2 = \text{parity mục tiêu},\\[2pt]
1 & \text{nếu sai parity}.
\end{cases}
$$

Tổng số quân thêm vào của cả bảng (với một mẫu) = **số ô bị sai parity** so với mẫu đó.

---

## 3. Chọn mẫu A hay mẫu B?

### 3.1. Tiêu chí 1 — tổng nhỏ nhất

Tổng cuối = (tổng quân ban đầu, cố định) + (số quân thêm vào). Tổng ban đầu không đổi, nên **giảm tổng cuối ⟺ giảm số ô bị sai parity.**

Đặt:

- $\text{lechA}$ = số ô có parity **khác** mẫu A;
- $\text{lechB}$ = số ô có parity **khác** mẫu B.

Có một quan sát rất gọn: vì mẫu A và mẫu B là **đối nhau hoàn toàn** ở mọi ô, nên mỗi ô **sai đúng một trong hai mẫu** (không thể sai cả hai, cũng không thể đúng cả hai). Do đó:

$$
\text{lechA} + \text{lechB} = n \cdot m.
$$

Tức là tính $\text{lechA}$ xong thì $\text{lechB} = n\cdot m - \text{lechA}$, **không cần quét bảng lần hai**.

- Nếu $\text{lechA} < \text{lechB}$ → mẫu A thêm ít cờ hơn → **chọn A**.
- Nếu $\text{lechB} < \text{lechA}$ → **chọn B**.
- Nếu $\text{lechA} = \text{lechB}$ (chỉ xảy ra khi $n\cdot m$ chẵn và bằng nhau đúng một nửa) → **hoà về tổng**, chuyển sang tiêu chí 2.

### 3.2. Tiêu chí 2 — nhỏ nhất theo từ điển (chỉ khi hoà)

Khi hoà, cả hai bảng $M_A$ (theo mẫu A) và $M_B$ (theo mẫu B) cùng tổng. Ta so từ điển: đọc bảng thành dãy số theo hàng, so sánh từ trái sang phải, **vị trí đầu tiên khác nhau** quyết định.

Vị trí đầu tiên chính là ô $(0,0)$. Ở ô này:

- Mẫu A muốn $(0,0)$ **chẵn**, mẫu B muốn $(0,0)$ **lẻ** — hai mục tiêu ngược nhau.
- $a_{0,0}$ có một parity cố định, nên nó **khớp đúng một mẫu**: mẫu khớp giữ nguyên $a_{0,0}$, mẫu kia phải $+1$ thành $a_{0,0}+1$.

Suy ra $M_A[0][0]$ và $M_B[0][0]$ **luôn khác nhau** (chênh đúng 1). Mà đây đã là ô đầu tiên, nên thứ tự từ điển **được quyết ngay tại $(0,0)$**: bảng nào có $(0,0)$ nhỏ hơn thì thắng — tức là bảng **giữ nguyên** $a_{0,0}$ (không $+1$).

> Nói gọn: khi hoà, chọn mẫu sao cho **parity mục tiêu tại $(0,0)$ = parity của $a_{0,0}$**. Học sinh hay sa đà đi so cả bảng — không cần, chỉ ô $(0,0)$ là đủ.

### 3.3. Gộp cả hai mẫu vào một tham số

Để code gọn, ta tham số hoá: parity mục tiêu của ô $(i,j)$ là

$$
\text{target}(i,j) = (i + j + \text{offset}) \bmod 2,
$$

với `offset = 0` là mẫu A, `offset = 1` là mẫu B. Khi đó:

- $\text{lechA} < \text{lechB}$ → `offset = 0`;
- $\text{lechB} < \text{lechA}$ → `offset = 1`;
- hoà → `offset = a[0][0] % 2` (vì $\text{target}(0,0) = \text{offset}$, đặt bằng parity của $a_{0,0}$ là giữ nguyên ô đầu).

---

## 4. Tóm tắt thuật toán (các bước để code)

1. Đọc $n, m$ và bảng $a$. Trong lúc đọc, đếm luôn $\text{lechA}$ = số ô có $a_{i,j}\bmod 2 \ne (i+j)\bmod 2$.
2. $\text{lechB} = n\cdot m - \text{lechA}$.
3. Quyết định `offset`:
   - `lechA < lechB` → 0;
   - `lechB < lechA` → 1;
   - bằng nhau → `a[0][0] % 2`.
4. In bảng: với mỗi ô, đặt `target = (i + j + offset) % 2`; nếu `a[i][j] % 2 != target` thì in `a[i][j] + 1`, ngược lại in `a[i][j]`.

Thế là xong. Không DP, không đồ thị, không tìm kiếm — chỉ là **nhìn ra mô hình bàn cờ + một phép đếm**.

---

## 5. Độ phức tạp

- **Thời gian:** $O(n \cdot m)$ — quét bảng đúng 2 lần (1 lần đọc/đếm, 1 lần in). Tối đa $\approx 10^4$ phép tính.
- **Bộ nhớ:** $O(n \cdot m)$ để lưu bảng (thực ra có thể $O(m)$ nếu xử lý từng hàng, nhưng không cần thiết với giới hạn này).

So với giới hạn $1$ giây / $256$ MB thì dư sức rất nhiều.

---

## 6. Trường hợp biên cần để ý

- **$n = 1$ (một hàng) hoặc $m = 1$ (một cột):** vẫn là bàn cờ 1 chiều (C L C L …). Công thức $(i+j+\text{offset})\bmod 2$ tự đúng, không cần code riêng. Đây đúng là subtask 1, 2 ($n=1$).
- **$n = m = 1$ (một ô duy nhất):** không có cặp ô kề nào → không có ràng buộc. Khi đó $\text{lechA}+\text{lechB}=1$, không bao giờ hoà, thuật toán tự chọn mẫu "không phải thêm" → in nguyên $a_{0,0}$. Đúng (tổng nhỏ nhất là không thêm gì).
- **Ô đã đúng hết theo một mẫu:** $\text{lech}=0$ cho mẫu đó → giữ nguyên toàn bộ.

---

## 7. Lỗi học sinh hay mắc

- **Tràn số (overflow).** $a_{i,j}$ tới $10^9$, cộng thêm 1 thành $10^9+1$. Số này *vẫn* lọt `int` (giới hạn $\approx 2.1\cdot10^9$), nhưng **đừng tin may rủi** — hãy dùng `long long` cho giá trị ô, và đặc biệt khi tính $n\cdot m$ hãy ép `(long long)n*m` cho chắc tay (ở đây $n\cdot m \le 10^4$ nên không tràn, nhưng tạo thói quen tốt).
- **Hiểu sai "tổng lẻ".** Một số bạn đi cố làm cho từng ô lẻ, hoặc nhầm thành "tổng chẵn". Nhớ: điều kiện là **hai ô kề khác parity** (bàn cờ), không phải mỗi ô phải lẻ.
- **Quên rằng chỉ được THÊM.** Nếu cho phép cả bớt thì lời giải khác (luôn về parity gần nhất). Ở đây sai parity → bắt buộc $+1$ (đi lên), không được $-1$.
- **Tie-break đi so cả bảng.** Khi hoà, chỉ cần xét ô $(0,0)$. So cả bảng vừa thừa vừa dễ sai.
- **In sai định dạng.** Mỗi hàng $m$ số cách nhau bởi dấu cách, hết hàng xuống dòng. Dùng `"\n"` thay vì `endl` để khỏi flush nhiều (thói quen tránh TLE ở các bài I/O lớn về sau).
- **Dùng `%` với số có thể âm.** Bài này $a_{i,j}\ge 0$ nên không lo, nhưng nhắc trước: trong C++ `(-3) % 2 == -1`, không phải `1`. Khi xử lý parity số âm phải cẩn thận.

---

## 8. Code mẫu (C++20)

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;

    vector<vector<long long>> a(n, vector<long long>(m));

    long long lechA = 0;                 // số ô SAI parity so với mẫu A: target = (i+j)%2
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++) {
            cin >> a[i][j];
            int targetA = (i + j) & 1;   // mẫu A muốn ô (i,j) có parity (i+j)%2
            if ((a[i][j] & 1) != targetA) lechA++;
        }

    long long tong = (long long)n * m;
    long long lechB = tong - lechA;      // mỗi ô sai đúng 1 trong 2 mẫu

    int offset;                          // target(i,j) = (i + j + offset) % 2
    if (lechA < lechB)      offset = 0;  // mẫu A thêm ít cờ hơn  -> tổng nhỏ hơn
    else if (lechB < lechA) offset = 1;  // mẫu B
    else                    offset = (int)(a[0][0] & 1); // HOÀ: giữ nguyên ô (0,0) -> lex nhỏ nhất

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            int target = (i + j + offset) & 1;
            long long v = a[i][j];
            if ((v & 1) != target) v++;  // chỉ được THÊM: sai parity thì +1 là đủ
            cout << v << (j + 1 < m ? ' ' : '\n');
        }
    }
    return 0;
}
```

Ghi chú nhỏ về code:

- `x & 1` là cách nhanh và an toàn (với số không âm) để lấy parity, tương đương `x % 2`.
- Vòng lặp in dùng `(j + 1 < m ? ' ' : '\n')`: trong hàng thì cách nhau dấu cách, cuối hàng thì xuống dòng.

---

## 9. Chạy thử với ví dụ đề

Đầu vào:

```
2 3
1 1 2
2 2 3
```

Parity của bảng (L = lẻ, C = chẵn):

$$
\begin{matrix}
1(L) & 1(L) & 2(C) \\
2(C) & 2(C) & 3(L)
\end{matrix}
$$

**Mẫu A** muốn (theo $(i+j)\bmod 2$):

$$
\begin{matrix}
C & L & C \\
L & C & L
\end{matrix}
$$

So từng ô với parity thực tế: ô $(0,0)$ thực **L** ≠ mong muốn **C** → sai (+1); ô $(1,0)$ thực **C** ≠ **L** → sai (+1); các ô còn lại khớp. Vậy $\text{lechA} = 2$.

$\text{lechB} = n\cdot m - \text{lechA} = 6 - 2 = 4$.

Vì $\text{lechA}=2 < \text{lechB}=4$ → chọn **mẫu A** (`offset = 0`). Điền số:

- $(0,0)$: $1$ sai parity → $1+1 = \mathbf{2}$
- $(0,1)$: $1$ đúng → $\mathbf{1}$
- $(0,2)$: $2$ đúng → $\mathbf{2}$
- $(1,0)$: $2$ sai → $2+1 = \mathbf{3}$
- $(1,1)$: $2$ đúng → $\mathbf{2}$
- $(1,2)$: $3$ đúng → $\mathbf{3}$

Bảng ra:

```
2 1 2
3 2 3
```

Đúng bằng đáp án mẫu của đề. Kiểm tra nhanh vài cặp kề: $2+1=3$ (lẻ ✓), $1+2=3$ (✓), $2+3=5$ (✓), $3+2=5$ (✓)… tất cả đều lẻ.

---

## 10. Nhận xét

Bài này không khó về kỹ thuật, mà khó ở **bước mô hình hoá**:

1. "Tổng kề lẻ" → dịch sang **parity** (chỉ giữ bit cuối).
2. "Hai ô kề khác parity" → **tô bàn cờ** → chỉ còn **2 mẫu** hợp lệ.
3. "Chỉ được thêm" → mỗi ô tốn **0 hoặc 1** quân; tổng cần thêm = **số ô sai parity**.
4. Hai mẫu đối nhau ⇒ $\text{lechA}+\text{lechB}=nm$ ⇒ chọn mẫu lệch ít hơn; hoà thì so ô $(0,0)$.

Khi một đề "phức tạp" (luật game rối rắm) mà giới hạn lại **rất nhỏ** ($n,m\le100$), hãy nghi ngờ rằng phần lớn đề là *gây nhiễu*, và lời giải thật sự nằm ở **một quan sát toán học gọn**. Rèn phản xạ "dịch điều kiện sang parity / sang tô màu" sẽ giúp ích cho rất nhiều bài về sau.

---

## 11. Tóm tắt giải thuật

**Nhận xét cốt lõi.** Điều kiện "tổng hai ô kề là lẻ" $\equiv$ "hai ô kề khác parity" $\equiv$ tô bàn cờ. Toàn bảng bị ép theo **đúng 2 mẫu** parity (A: $(0,0)$ chẵn; B: $(0,0)$ lẻ), và mẫu nào cũng **luôn khả thi** (lưới là đồ thị hai phía).

**Chi phí.** Chỉ được thêm cờ ⇒ mỗi ô tốn **0** (đã đúng parity) hoặc **1** (sai parity, $+1$ là đủ). Tổng thêm của một mẫu = **số ô sai parity** của mẫu đó.

**Chọn mẫu.** Hai mẫu đối nhau ⇒ mỗi ô sai đúng một mẫu ⇒ $\text{lechA} + \text{lechB} = n\cdot m$.
- $\text{lechA} < \text{lechB}$ → mẫu A; $\text{lechB} < \text{lechA}$ → mẫu B (tổng nhỏ nhất).
- Hoà ($\text{lechA}=\text{lechB}$) → so từ điển, quyết **ngay tại ô $(0,0)$**: chọn mẫu **giữ nguyên** $a_{0,0}$.

**Mã giả.**

```
Đọc n, m, bảng a
lechA = đếm số ô có (a[i][j] % 2) != ((i + j) % 2)
lechB = n * m - lechA

if   lechA < lechB:  offset = 0          # mẫu A
elif lechB < lechA:  offset = 1          # mẫu B
else:                offset = a[0][0] % 2 # hoà -> giữ nguyên ô (0,0)

for mỗi ô (i, j):
    target = (i + j + offset) % 2
    out = a[i][j] + (0 nếu a[i][j] % 2 == target, ngược lại 1)
    in ra out
```

**Độ phức tạp:** thời gian $O(n\cdot m)$, bộ nhớ $O(n\cdot m)$ (có thể giảm còn $O(m)$ nếu in từng hàng).

**Bẫy cần nhớ:** dùng `long long` cho giá trị ô ($a_{i,j}+1$ tới $10^9+1$); chỉ được **thêm** nên sai parity phải $+1$ chứ không $-1$; tie-break **chỉ xét ô $(0,0)$**, đừng so cả bảng.
