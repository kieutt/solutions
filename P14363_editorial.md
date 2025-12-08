# Hướng dẫn giải: Thay thế xâu (P14363)

## 1. Phân tích bài toán

### Đề bài tóm tắt
- Cho $n$ cặp xâu $(s_{i,1}, s_{i,2})$ với $|s_{i,1}| = |s_{i,2}|$
- Mỗi truy vấn $(t_1, t_2)$: đếm số cách chọn một xâu con $y$ của $t_1$ và thay bằng $y'$ để được $t_2$
- Hai cách khác nhau nếu **vị trí khác** hoặc **cặp $i$ khác**

### Nhận xét quan trọng

**Nhận xét 1:** Vì chỉ thực hiện **đúng một** phép thay thế và $t_1 \neq t_2$, nên phép thay thế phải "phủ" toàn bộ các vị trí khác nhau giữa $t_1$ và $t_2$.

**Nhận xét 2:** Gọi:
- $L$ = vị trí **đầu tiên** mà $t_1[L] \neq t_2[L]$
- $R$ = vị trí **cuối cùng** mà $t_1[R] \neq t_2[R]$

Khi đó, một phép thay thế tại vị trí $p$ với cặp $(s_{i,1}, s_{i,2})$ có độ dài $\ell$ là hợp lệ khi và chỉ khi:

1. $p \leq L$ (bao phủ điểm khác đầu tiên)
2. $p + \ell - 1 \geq R$ (bao phủ điểm khác cuối cùng)
3. $t_1[p \ldots p+\ell-1] = s_{i,1}$
4. $t_2[p \ldots p+\ell-1] = s_{i,2}$

**Nhận xét 3:** Từ điều kiện (1) và (2), ta có:
- $\ell \geq R - L + 1$ (độ dài tối thiểu)
- $p \in [\max(0, R - \ell + 1), \min(L, |t_1| - \ell)]$

## 2. Thuật toán

### Ý tưởng chính

1. **Tiền xử lý:** Nhóm các cặp theo độ dài, lưu số lần xuất hiện của mỗi cặp $(hash(s_1), hash(s_2))$

2. **Xử lý truy vấn:**
   - Tìm $L$ và $R$
   - Với mỗi độ dài $\ell$ có trong tập cặp:
     - Nếu $\ell < R - L + 1$: bỏ qua (quá ngắn)
     - Nếu $\ell > |t_1|$: bỏ qua (quá dài)
     - Duyệt các vị trí $p$ hợp lệ, tra cứu hash và cộng kết quả

### Kỹ thuật Hash

Sử dụng **Polynomial Rolling Hash** với công thức:
$$H(s) = s[0] \cdot B^{n-1} + s[1] \cdot B^{n-2} + \ldots + s[n-1]$$

Để tính hash xâu con $s[l \ldots r]$ trong $O(1)$:
$$H(s[l \ldots r]) = H[r+1] - H[l] \cdot B^{r-l+1}$$

Với $H[i]$ là hash tiền tố của $s[0 \ldots i-1]$.

**Double Hashing:** Dùng 2 base khác nhau để giảm xác suất va chạm.

### Cấu trúc dữ liệu

```
count_by_len[ℓ][hash(s₁)][hash(s₂)] = số cặp có độ dài ℓ, s₁ có hash này, s₂ có hash này
```

Sử dụng `unordered_map` lồng nhau để tra cứu $O(1)$.

## 3. Độ phức tạp

### Thời gian
- **Tiền xử lý:** $O(L_1)$ để hash tất cả các cặp
- **Mỗi truy vấn:** $O(|t| + k \cdot m)$
  - $|t|$: tính hash tiền tố
  - $k$: số độ dài khác nhau cần xét
  - $m$: số vị trí hợp lệ trung bình

**Worst case:** Khi $L = R$ (chỉ khác 1 vị trí), số vị trí hợp lệ là $O(\min(L+1, |t|-\ell+1))$ cho mỗi $\ell$.

### Không gian
- $O(L_1)$ cho hash table
- $O(\max|t|)$ cho hash tiền tố mỗi truy vấn

## 4. Cài đặt

### Pseudocode

```
# Tiền xử lý
FOR each pair (s1, s2):
    ℓ = |s1|
    h1 = hash(s1)
    h2 = hash(s2)
    count_by_len[ℓ][h1][h2] += 1

lens = sorted list of all ℓ in count_by_len

# Xử lý truy vấn
FOR each query (t1, t2):
    IF |t1| ≠ |t2|:
        output 0
        continue
    
    L = first index where t1[i] ≠ t2[i]
    R = last index where t1[i] ≠ t2[i]
    
    Compute prefix hashes for t1 and t2
    
    ans = 0
    min_len = R - L + 1
    
    FOR each ℓ in lens where min_len ≤ ℓ ≤ |t1|:
        p_min = max(0, R - ℓ + 1)
        p_max = min(L, |t1| - ℓ)
        
        FOR p = p_min to p_max:
            h1 = hash(t1[p..p+ℓ-1])
            h2 = hash(t2[p..p+ℓ-1])
            ans += count_by_len[ℓ][h1][h2]
    
    output ans
```

## 5. Tối ưu hóa

### Tối ưu 1: Natural Overflow
Thay vì dùng modulo (chậm), để tràn số tự nhiên trong `unsigned long long`. Xác suất va chạm vẫn thấp với double hash.

### Tối ưu 2: Binary Search
Dùng binary search để tìm độ dài nhỏ nhất cần xét thay vì duyệt từ đầu.

### Tối ưu 3: Static Array
Dùng mảng tĩnh cho bảng lũy thừa thay vì `vector` để giảm overhead.

## 6. Xử lý các subtask đặc biệt

### Subtask A: $q = 1$
Không cần tối ưu đặc biệt, thuật toán chính đã đủ nhanh.

### Subtask B: Xâu đặc biệt
Mỗi xâu chỉ chứa 'a' và 'b', với đúng một chữ 'b'.

**Nhận xét:** Vị trí của 'b' trong $s_1$ và $s_2$ xác định hoàn toàn xâu (cùng với độ dài). Có thể dùng cách biểu diễn nhỏ gọn hơn, nhưng thuật toán hash vẫn hoạt động tốt.

## 7. Ví dụ minh họa

**Input:**
```
3 1
xabcx xadex
ab cd
bc de
xabcx xadex
```

**Phân tích truy vấn** $(t_1, t_2) = (\texttt{xabcx}, \texttt{xadex})$:

1. Tìm $L, R$:
   - $t_1[0] = t_2[0] = \texttt{x}$ ✓
   - $t_1[1] = t_2[1] = \texttt{a}$ ✓
   - $t_1[2] = \texttt{b} \neq \texttt{d} = t_2[2]$ → $L = 2$
   - $t_1[3] = \texttt{c} \neq \texttt{e} = t_2[3]$
   - $t_1[4] = t_2[4] = \texttt{x}$ ✓
   - → $R = 3$, cần $\ell \geq 2$

2. Xét $\ell = 2$: (cặp `ab`→`cd`, `bc`→`de`)
   - $p \in [\max(0, 3-2+1), \min(2, 5-2)] = [2, 2]$
   - $p = 2$: $t_1[2..3] = \texttt{bc}$, $t_2[2..3] = \texttt{de}$
   - Khớp với cặp 3! → +1

3. Xét $\ell = 5$: (cặp `xabcx`→`xadex`)
   - $p \in [\max(0, 3-5+1), \min(2, 5-5)] = [0, 0]$
   - $p = 0$: $t_1[0..4] = \texttt{xabcx}$, $t_2[0..4] = \texttt{xadex}$
   - Khớp với cặp 1! → +1

**Kết quả:** 2
