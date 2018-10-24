#23. merge k sorted lists

**<font color=red>�Ѷ�:Hard</font>**

## ˢ������

> ԭ������

*https://leetcode.com/problems/merge-k-sorted-lists/
* 
> ��������

```
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

Example:

Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

## ���ⷽ��

> ˼·1
******- ʱ�临�Ӷ�: O(Nlg(K))******- �ռ临�Ӷ�: O(K)******

����������С�ѣ���ȥÿ������ĵ�һ��Ԫ�ع�����С�ѣ�����������������ģ���ˣ�ÿ�ζѵĶ���������С��Ԫ�أ����������ȶ���ʵ����С�ѡ�


```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
	struct cmp
	{
		bool operator()(ListNode* a, ListNode* b) const
		{
			return a ->val > b ->val;
		}
	};
    ListNode* mergeKLists(vector<ListNode*>& lists) {  
		priority_queue<ListNode*,vector<ListNode*>,cmp> pq;
		ListNode* ret = nullptr;
		ListNode* current = nullptr;
		for(int i = 0;i < lists.size();++i)
			if(lists[i])
				pq.push(lists[i]);
		while(pq.size())
		{
			ListNode* temp = pq.top();
			pq.pop();
			if(!ret)
				ret = temp;
			else
				current ->next = temp;
			current = temp;
			if(temp ->next)
				pq.push(temp ->next);
		}
		return ret;
    }
};
```
> ˼·2
******- ʱ�临�Ӷ�: O(Nlg(K))******- �ռ临�Ӷ�: O(1)******

���˼·�÷���˼�룬���ǿ���ͨ���鲢������������ǰ���Ѿ���������������������������ǿ��԰�������Ԫ�أ�ֻҪ������ Lists���й鲢���򼴿ɡ�

```cpp
class Solution {
public:
    ListNode* merge(ListNode* list1, ListNode* list2) {
        ListNode head(0);
        ListNode* tail = &head;
        auto cur1 = list1;
        auto cur2 = list2;
        while (cur1 != nullptr && cur2 != nullptr) {
            if (cur1->val < cur2->val) {
                tail->next = cur1;
                tail = tail->next;
                cur1 = cur1->next;
            } else {
                tail->next = cur2;
                tail = tail->next;
                cur2 = cur2->next;
            }
        }
        auto cur = cur1 == nullptr ? cur2 : cur1;
        while (cur != nullptr) {
            tail->next = cur;
            tail = tail->next;
            cur = cur->next;
        }
        return head.next;
    }
    ListNode* mergeSort(vector<ListNode*>& lists, int start, int end) {
        if (start > end) {
            return nullptr;
        }
        if (start == end) {
            return lists[start];
        }
        int mid = start + (end - start) / 2;
        auto list1 = mergeSort(lists, start, mid);
        auto list2 = mergeSort(lists, mid + 1, end);
        return merge(list1, list2);
    }
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int n = lists.size();
        return mergeSort(lists, 0, n - 1);
    }
};
```