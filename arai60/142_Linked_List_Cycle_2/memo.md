# 問題

142. Linked List Cycle 2

https://leetcode.com/problems/linked-list-cycle-ii/

## Step.1

### 方針

前回 141_Linked List Cycle でレビューコメントをいただいて、unordered_setを持ちた方法を
step.4として実装した。
今回は、その応用として、閉ループを検出したときに、bool型で返すのではなく、
検出したListNodeのアドレスを返すように実装した。

処理内容も同じであるため、
時間計算量は　最悪の場合 O(N^2)であるため、制限時間内に実行可能である。
空間計算量では、O(N)であり、Nの最大値は10^4であるため、制限のメモリ容量内に収まる。


```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode* head) {
        std::unordered_set<ListNode*> visited;

        while (head != nullptr) {
            if (visited.contains(head)) {
                return head;
            }
            
            visited.insert(head);
            head = head->next;
        }

        return nullptr;
    }
};
```


## Step.2.1　（他の人のコードを読み、コードを整える）

Step.1について、googleコーディングスタンダードを見ていると、
引数を関数内で再代入して破壊されることは、好ましくないのでローカル変数を使用する方法に変更する。

この場合は、引数のheadにconst 属性を付与して、明示的に再代入することを防ぐべきでもあると考えたが、
leetCodeのテンプレートもあるので、constを付与せず、ローカル変数を使用する。

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* current = head;
        std::unordered_set<ListNode*> visited;

        while (current != nullptr) {
            if (visited.contains(current)) {
                return current;
            }

            visited.insert(current);
            current = current->next;
        }

        return nullptr;
    }
};
```

## Step.2.2 (別解法について)

Floyedの循環検出法を用いた方法でも解けるようなので、こちらの解法についても
回答の引き出しを増やすために、検討する。

https://github.com/chryschron/codings/pull/2

### 解法

Floyedの循環検出で用いた変数、slowとfastはそれぞれ、到達するまでに以下の距離進む。

slow: a + b
fast: a + b + k * L

ここで、a, b, k, L はそれぞれ

* a:　先頭ノードから循環開始ノードまでの距離
* b: 循環開始ノードから、２つの変数が出会った地点までの距離
* L: 循環ノードの長さ
* k: 循環ノードの周回数

加えて以下も定義する
* c: 出会った地点から循環開始ノードまでの距離

fastをslowの２倍の速さで動かしているため、以下の等式がなりたつ。

2(a + b) = a + b + k * L

以下のように等式を変形していく。

a + b = k * L
a = k * L-b
a = (k - 1) * L + (b + c) - b
a = (k - 1) * L + c

それぞれ以下を意味する。
* 左辺の a は先頭ノードから循環の開始ノードまでの距離
* 右辺の (k - 1) * L + c は、出会ったノードからループを k - 1 周したあと、さらに cノード進むと循環の開始ノードに到達する距離

なので、出会ったノードと、先頭ノードから同じステップ進めることで、循環開始ノードにたどり着くことができる。


### 計算量

時間計算量については、循環検出に必要な時間計算量はO(N)である。
その後、開始ノードを検出するまでについては、上記のa (a < N) であるため、O(N)となる。
よって全体の計算量は、O(N) + O(N) = O(N) である。

空間計算量は、使用した変数は３つであるため、O(3) (=O(1)) である。

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;

        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast) {
                ListNode* current = head;

                while (slow != current) {
                    slow = slow->next;
                    current = current->next;
                }

                return slow;
            }
        }

        return nullptr;
    }
};
```

### 感想

コードの綺麗さ、読みやすさの観点では、Step.1から改善できているが、時間計算量の観点は同じであるため、
別の解法などの検討の余地がある。
他の方のレビュー依頼等を参考にすると、hashmapを利用した方法もあったが、
hashmapの解法も訪問済みのNodeをhashmapに登録して、訪問済みのNodeに戻ることがあるかを確認する方法であると理解しているため、
時間計算量の観点では、今回の循環検出方と同じ ( O(N) )であり、
空間計算量の観点では、Nodeの数のhaspmapが必要であるため、 O(N)　である。


## Step.3 (反復練習)

Step.2 のコードを10分以内にエラーなく実装できるように反復練習。


```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* current = head;
        std::unordered_set<ListNode*> visited;

        while (current != nullptr) {
            if (visited.contains(current)) {
                return current;
            }

            visited.insert(current);
            current = current->next;
        }

        return nullptr;
    }
};
```


### 参考

https://github.com/Tarec39/LeetCode_arai60/pull/2

https://github.com/chryschron/codings/pull/2