# 問題

141. Linked List Cycle

https://leetcode.com/problems/linked-list-cycle/description/

## Step.1

### 方針

Floydの循環検出アルゴリズムを用いて、愚直にコードを実装。
whileの条件式と、if の条件式が冗長のような気がしているが、とりあえず Step.1としては、以下で実装。
改善方法については、Step.2 で検討することとした。

また、LeetCodeの結果から、実行速度の観点で改善の余地があることがわかったので、ここもStep.2での課題としていく。


```cpp
class Solution {
public:
    bool hasCycle(ListNode* head) {
        ListNode* step1 = head;
        ListNode* step2 = head;

        while (step1 != NULL and step2 != NULL) {
            if (step1->next == NULL) {
                return false;
            }
            step1 = step1->next;

            if (step2->next == NULL || step2->next->next == NULL) {
                return false;
            }
            step2 = step2->next->next;

            if (step1 == step2) {
                return true;
            }
        }
        return false;
    }
};

```


## Step.2　（他の人のコードを読み、コードを整える）

### 参考

https://github.com/tkytky5/leetcode/pull/1

### 改善箇所

Step.1では、変数step1 と step2 のnext メンバの NULLポインタチェックを行っているが、
変数step1 は step2 が1度通ったNodeを指しているので、NULLポインタチェックを行う必要はない。

### 計算量について

Step.1 では、計算量について考えることができていなかったため、計算量を考える。

Step2. の方法では時間計算量は、O(N) となる。
LinkedListの要素をすべて確認し、循環の確認を行う必要があるため。

空間計算量では、回答で新規で使用するメモリは、変数slow, fast の2つのみであるため、
O(1)となる。

空間計算量は

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *slow = head;
        ListNode *fast = head;
        
        while(fast != NULL && fast->next != NULL)
        {
            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast)
            {
                return true;
            }
        }
        return false;
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
    bool hasCycle(ListNode *head) {
        ListNode *slow = head;
        ListNode *fast = head;

        while(fast != NULL && fast->next != NULL)
        {
            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast)
            {
                return true;
            }
        }
        return false;
    }
};
```


### 参考