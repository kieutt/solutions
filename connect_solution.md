# Bài giải CONNECT - Tối ưu hóa Binary Search

## 1. Phân tích vấn đề TLE

**Vấn đề của solution cũ**:
- Binary search trên [1, 10^9] → 30 lần kiểm tra
- Nhiều giá trị không xuất hiện trong input → lãng phí thời gian

**Solution tối ưu**:
- Binary search chỉ trên **các giá trị w xuất hiện trong input**
- Sắp xếp và loại bỏ trùng lặp → chỉ tối đa M giá trị khác nhau
- Giảm số lần kiểm tra từ 30 xuống log(M)

## 2. Thuật toán tối ưu

1. **Thu thập và sắp xếp** tất cả giá trị w
2. **Loại bỏ trùng lặp**
3. **Binary search** trên mảng giá trị đã sắp xếp
4. **Kiểm tra khả thi** với thuật toán Tarjan

## 3. Code tham khảo tối ưu

### C++ (Optimized)
```cpp
#include <bits/stdc++.h>
using namespace std;

struct Edge {
    int u, v, w;
};

int n, m;
vector<Edge> edges;
vector<vector<pair<int, int>>> adj;
vector<int> tin, low, visited;
int timer, cnt;
bool hasBridge;

void dfs(int u, int p = -1) {
    visited[u] = 1;
    tin[u] = low[u] = ++timer;
    cnt++;
    
    for (auto &edge : adj[u]) {
        int v = edge.first;
        int edgeId = edge.second;
        
        if (edgeId == p) continue; // Không quay lại cạnh cha
        
        if (visited[v]) {
            low[u] = min(low[u], tin[v]);
        } else {
            dfs(v, edgeId);
            low[u] = min(low[u], low[v]);
            if (low[v] > tin[u]) {
                hasBridge = true;
            }
        }
    }
}

bool check(int maxWeight) {
    // Xây dựng đồ thị với các cạnh w <= maxWeight
    adj.assign(n, vector<pair<int, int>>());
    int edgeId = 0;
    
    for (auto &e : edges) {
        if (e.w <= maxWeight) {
            adj[e.u].push_back({e.v, edgeId});
            adj[e.v].push_back({e.u, edgeId});
            edgeId++;
        }
    }
    
    // Tìm đỉnh bắt đầu (đỉnh có cạnh)
    int start = -1;
    for (int i = 0; i < n; i++) {
        if (!adj[i].empty()) {
            start = i;
            break;
        }
    }
    
    if (start == -1) return false; // Không có cạnh nào
    
    // Khởi tạo cho thuật toán Tarjan
    tin.assign(n, 0);
    low.assign(n, 0);
    visited.assign(n, 0);
    timer = 0;
    cnt = 0;
    hasBridge = false;
    
    // Chạy DFS từ đỉnh bắt đầu
    dfs(start);
    
    // Kiểm tra: liên thông và không có cầu
    return cnt == n && !hasBridge;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    cin >> n >> m;
    edges.resize(m);
    
    for (int i = 0; i < m; i++) {
        cin >> edges[i].u >> edges[i].v >> edges[i].w;
        edges[i].u--; // Convert to 0-indexed
        edges[i].v--;
    }
    
    if (n == 1) {
        cout << 0 << endl;
        return 0;
    }
    
    // Thu thập tất cả giá trị w và sắp xếp
    vector<int> weights;
    for (auto &e : edges) {
        weights.push_back(e.w);
    }
    
    sort(weights.begin(), weights.end());
    weights.erase(unique(weights.begin(), weights.end()), weights.end());
    
    // Binary search trên các giá trị w duy nhất
    int left = 0, right = weights.size() - 1, ans = -1;
    
    while (left <= right) {
        int mid = (left + right) / 2;
        if (check(weights[mid])) {
            ans = mid;
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    
    cout << (ans == -1 ? -1 : weights[ans]) << endl;
    return 0;
}
```
## 4. Tối ưu hóa chính

### A. Binary Search trên giá trị duy nhất
```cpp
// Thay vì binary search trên [1, 10^9]
int left = 1, right = 1e9;

// Binary search trên các giá trị w xuất hiện
vector<int> weights;
for (auto &e : edges) weights.push_back(e.w);
sort(weights.begin(), weights.end());
weights.erase(unique(weights.begin(), weights.end()), weights.end());

int left = 0, right = weights.size() - 1;
```

### B. Tối ưu thuật toán Tarjan
- Sử dụng **edge ID** thay vì parent vertex để tránh đếm sai cạnh song song
- Khởi tạo lại vector một cách hiệu quả
- Sử dụng 0-indexed để tránh nhầm lẫn

### C. Xử lý edge cases
- N = 1: Đáp án = 0
- Không có cạnh nào thỏa mãn: Đáp án = -1

## 5. Độ phức tạp tối ưu

**Trước khi tối ưu**: O(30 × (N + M)) = O(N + M) nhưng hằng số = 30
**Sau khi tối ưu**: O(log M × (N + M)) với M ≤ 2×10^5

**Cải thiện**: Từ 30 lần kiểm tra xuống tối đa log(200000) ≈ 18 lần

## 6. Lưu ý implementation

1. **0-indexed vs 1-indexed**: Chuyển về 0-indexed cho dễ xử lý
2. **Edge ID**: Dùng ID cạnh thay vì đỉnh cha trong Tarjan
3. **Unique weights**: Loại bỏ trùng lặp để giảm số lần kiểm tra
4. **Memory management**: Sử dụng `assign()` thay vì `clear() + resize()`

Với những tối ưu này, solution sẽ chạy trong thời gian cho phép cho tất cả test cases.
