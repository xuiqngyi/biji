### 1.栈与队列

#### 用两个栈实现队列

![image-20220102104158540](C:\Users\xuqingyi\AppData\Roaming\Typora\typora-user-images\image-20220102104158540.png)

```c++
class CQueue {
public:
    stack<int>sta1;
    stack<int>sta2;
    CQueue() {

    }
    
    void appendTail(int value) {
        if(sta1.empty()){
            sta1.push(value);
            return;
        }
        while(!sta1.empty()){
            sta2.push(sta1.top());
            sta1.pop();
        }
        sta1.push(value);
        while(!sta2.empty()){
            sta1.push(sta2.top());
            sta2.pop();
        }
    }
    
    int deleteHead() {
        if(sta1.empty()){ //重点
            return -1;
        }
        int result=sta1.top();
        sta1.pop();
        return result;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

#### [包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

![image-20220102112719879](C:\Users\xuqingyi\AppData\Roaming\Typora\typora-user-images\image-20220102112719879.png)

```C++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int>datasta;
    stack<int>minsta;
    MinStack() {
        while(!datasta.empty()){
            datasta.pop();
        }
        while(!minsta.empty()){
            minsta.pop();
        }

    }
    
    void push(int x) {
        if(datasta.empty()){
            datasta.push(x);
            minsta.push(x);
            return;
        }
        datasta.push(x);
        if(x<minsta.top()){
            minsta.push(x);
        }
        else{
            minsta.push(minsta.top());
        }
    }
    
    void pop() {
        if(datasta.empty() && minsta.empty()){
            return;
        }
        datasta.pop();
        minsta.pop();
    }
    
    int top() {
        if(datasta.empty()){
            return -1;
        }
        return datasta.top();
    }
    
    int min() {
        if(minsta.empty()){
            return -1;
        }
        return minsta.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```

### 链表

#### 从尾到头打印链表

![image-20220102113030195](C:\Users\xuqingyi\AppData\Roaming\Typora\typora-user-images\image-20220102113030195.png)

辅助栈方法：

```C++
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
    vector<int> reversePrint(ListNode* head) {
        stack<int>sta;
        for(auto temp=head;temp;temp=temp->next){
            sta.push(temp->val);
        }
        int n=sta.size();
        vector<int>res(n);
        for(int i=0;i<n;i++){
            res[i]=sta.top();
            sta.pop();
        }
        return res;


    }
};
```

#### 反转链表

![image-20220102114553348](C:\Users\xuqingyi\AppData\Roaming\Typora\typora-user-images\image-20220102114553348.png)

```
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
    ListNode* reverseList(ListNode* head) {
        if(head==NULL ||head->next==NULL){
            return head;
        }
        ListNode* temp=reverseList( head->next);
        head->next->next=head;
        head->next=NULL;

        return temp;
    }
};
```

#### [ 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

![image-20220102115733001](C:\Users\xuqingyi\AppData\Roaming\Typora\typora-user-images\image-20220102115733001.png)

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(head==NULL){
            return head;
        }
        unordered_map<Node*,Node*>map;
        //利用哈希表实现原节点跟新节点的映射
        for(auto temp=head;temp;temp=temp->next){
            map[temp]=new Node(temp->val);
        }
        //实现新节点的指针指向
        for(auto temp=head;temp;temp=temp->next){
            map[temp]->next=map[temp->next];
            map[temp]->random=map[temp->random];
        }
        return map[head];
    }
};
```

### 字符串

#### 替换空格

![image-20220102151245568](E:\typora\剑指offer\image-20220102151245568.png)

```c++
class Solution {
public:
    string replaceSpace(string s) {
        int n=s.size();
        int count=0;
        for(int i=0;i<n;i++){
            if(s[i]==' '){
                count++;
            }
        }
        int newsize=n+count*2;
        s.resize(newsize);
        for(int i=n-1,j=newsize-1;i>=0;i--,j--){
            if(s[i]!=' '){
                s[j]=s[i];
            }
            else{
                s[j]='0';
                s[j-1]='2';
                s[j-2]='%';
                j=j-2;
            }
        }
        return s;
    }
};
```

#### 左旋字符串

![image-20220102151326406](E:\typora\剑指offer\image-20220102151326406.png)

具体步骤为：

1. 反转区间为前n的子串
2. 反转区间为n到末尾的子串
3. 反转整个字符串

```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        reverse(s.begin(),s.begin()+n);
        reverse(s.begin()+n,s.end());
        reverse(s.begin(),s.end());
        return s;

    }
};
```

### 查找算法

#### [数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

![image-20220102153326614](E:\typora\剑指offer\image-20220102153326614.png)

```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        unordered_map<int,int>map;
        for(int x:nums){
            map[x]++;
        }
        for(int x:nums){
            if(map[x]>1)
                return x;
        }
        return -1;
    }
};
```

#### [在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

![image-20220102153812739](E:\typora\剑指offer\image-20220102153812739.png)

方法一：哈希表

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        unordered_map<int,int>map;
        for(int x:nums){
            map[x]++;
        }
        for(int x:nums){
            if(x==target)
                return map[x];
        }
        return 0;
    }
};
```

方法二：二分查找

```c++
  if(nums.size()==0)
        return 0;
    int n = nums.size();
    int left =0;
    int right = n-1;
    while(left<right){
        int mid = left+(right-left)/2;
        if(nums[mid]>=target){
            right=mid;
        }
        else{
            left=mid+1;
        }
    }
    if(nums[left]!=target)
        return 0;
    int l=left;
    left =l;
    right = n-1;
    while(left<right){
        int mid = left+(right-left+1)/2;
        if(nums[mid]<=target){
            left=mid;
        }
        else{
            right=mid-1;
        }
    }
    int r=left;
    return r-l+1;
```

#### [0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

![image-20220102173023978](E:\typora\剑指offer\image-20220102173023978.png)

```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n=nums.size();
        int left=0;
        int right=n;
        while(left<right){
            int mid = left+(right-left)/2;
            if(nums[mid]==mid){
                left=mid+1;
            }
            else{
                right=mid;
            }
        }
        return left;
    }
};
```

#### 二分法总结：

1. 第一步判断结果范围，即确定left跟right的值。
2. while(left<right)
3. 不断逼近结果（使用right=mid-1的时候mid要加1）

#### [ 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

![image-20220102182931057](E:\typora\剑指offer\image-20220102182931057.png)

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.empty()){
            return false;
        }
        int row=0;
        int column=matrix[0].size()-1;
        while(row<matrix.size() && column>=0){
            if(matrix[row][column]<target){
                row++;
            }
            else if(matrix[row][column]>target){
                column--;
            }
            else{
                return true;
            }
        }
        return false;
    }
};
```

#### [旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

![image-20220102201637966](E:\typora\剑指offer\image-20220102201637966.png)

```
class Solution {
public:
    int minArray(vector<int>& numbers) {
        if(numbers.empty()){
            return 0;
        }
        int n=numbers.size();
        int left=0;
        int right=n-1;
        if(numbers[left]<numbers[right]){
            return numbers[left];
        }
        while(left<right){
            int mid=left+(right-left)/2;
            if(numbers[mid]>numbers[right]){
                left =mid +1;
            }
            else if(numbers[mid]<numbers[right]){
                right=mid;
            }
            else{
                right--;
            }
        }
        return numbers[left];
    }
};
```

#### 第一次出现的字符

![image-20220103102644311](E:\typora\剑指offer\image-20220103102644311.png)

```
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char,int>map;
        for(int i=0;i<s.size();i++){
            map[s[i]]++;
        }
        for(int i=0;i<s.size();i++){
            if(map[s[i]]==1){
                return s[i];
            }
        }
        return ' ';
    }
};
```

### 探索与回溯

#### [从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/) I

![image-20220103112437053](E:\typora\剑指offer\image-20220103112437053.png)

层序遍历

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        vector<int>result;
        if(root==NULL)
            return result;
        queue<TreeNode*>que;
        que.push(root);
        while(!que.empty()){
            int size = que.size();
            for(int i=0;i<size;i++){
                TreeNode* node=que.front();
                que.pop();
                result.push_back(node->val);
                if(node->left)
                    que.push(node->left);
                if(node->right)
                    que.push(node->right);
            }
        }
        return result;
    }
};
```

#### [从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

![image-20220103112617098](E:\typora\剑指offer\image-20220103112617098.png)

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>>result;
        if(root==NULL)
            return result;
        queue<TreeNode*>que;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            vector<int>vec;
            for(int i=0;i<size;i++){
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if(node->left){
                    que.push(node->left);
                }
                if(node->right){
                    que.push(node->right);
                }
            }
            result.push_back(vec);
        }
        return result;

    }
};
```

#### [从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

![image-20220103113152005](E:\typora\剑指offer\image-20220103113152005.png)

利用双栈存储奇偶行节点

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>>result;
        if(root==NULL){
            return result;
        }
        stack<TreeNode*>sta1;//偶数层
        stack<TreeNode*>sta2;//奇数层
        sta1.push(root);
        int count=0;
        while(!sta1.empty() || !sta2.empty()){
            if(count%2==0){
                int size=sta1.size();
                vector<int>vec;
                for(int i=0;i<size;i++){
                    TreeNode* node = sta1.top();
                    sta1.pop();
                    vec.push_back(node->val);
                    if(node->left){
                        sta2.push(node->left);
                    }
                    if(node->right){
                        sta2.push(node->right);
                    }
                }
                result.push_back(vec);
            }
            else{
                int size=sta2.size();
                vector<int>vec;
                for(int i=0; i<size; i++){
                    TreeNode* node=sta2.top();
                    sta2.pop();
                    vec.push_back(node->val);
                    if(node->right){
                        sta1.push(node->right);
                    }
                    if(node->left){
                        sta1.push(node->left);
                    }
                }
                result.push_back(vec);
            }
            count++;

        }
        return result;
    }
};
```

#### [树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

![image-20220103121346680](E:\typora\剑指offer\image-20220103121346680.png)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool panduan(TreeNode* A, TreeNode* B){
        if(B==NULL){
            return true;
        }
        if(A==NULL){
            return false;
        }
        if(A->val==B->val){
            return panduan(A->left,B->left) && panduan(A->right,B->right);
        }
        else{
            return false;
        }
    }
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if(A==NULL || B==NULL){
            return false;
        }
        if(A->val==B->val && panduan(A->left,B->left) && panduan(A->right,B->right)){
            return true;
        }
        return isSubStructure(A->left,B) ||isSubStructure(A->right,B);
    }
};
```

二叉树递归总结：

1. 先判断空节点的时候的结果
2. 计算递归到下一层的结果。
3. 在最后的函数里面不能轻易return true，要分析后续；

#### [二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

![image-20220103164017292](E:\typora\剑指offer\image-20220103164017292.png)

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isMirror(TreeNode* root1,TreeNode* root2){
        if(root1==NULL && root2==NULL){
            return true;
        }
        if(root1==NULL || root2==NULL){
            return false;
        }
        if(root1->val!=root2->val){
            return false;
        }
        return isMirror(root1->left,root2->right) && isMirror(root1->right,root2->left);
    }
    bool isSymmetric(TreeNode* root) {
        return isMirror(root,root);
    }
};
```

#### [对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

![image-20220103170320029](E:\typora\剑指offer\image-20220103170320029.png)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isMirror(TreeNode* root1,TreeNode* root2){
        if(root1==NULL && root2==NULL){
            return true;
        }
        if(root1==NULL || root2==NULL){
            return false;
        }
        if(root1->val!=root2->val){
            return false;
        }
        return isMirror(root1->left,root2->right) && isMirror(root1->right,root2->left);
    }
    bool isSymmetric(TreeNode* root) {
        return isMirror(root,root);
    }
};
```

### 动态规划

#### [斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

![image-20220103192043415](E:\typora\剑指offer\image-20220103192043415.png)

```
class Solution {
public:
    int fib(int n) {
        vector<int>dp(n+1);
        if(n==0 || n==1)
            return n;
        dp[0]=0;
        dp[1]=1;
        for(int i=2;i<=n;i++){
            dp[i]=dp[i-1]+dp[i-2];
            dp[i] %= 1000000007;
        }
        return dp[n];
    }
};
```

#### [青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

![image-20220103192126490](E:\typora\剑指offer\image-20220103192126490.png)

```
class Solution {
public:
    int numWays(int n) {
        vector<int>dp(n+1);
        if(n<=1)
            return 1;
        dp[0]=1;
        dp[1]=1;
        for(int i=2;i<=n;i++){
            dp[i]=dp[i-1]+dp[i-2];
            dp[i] = dp[i]%1000000007;
        }
        return dp[n];
    }
};
```

#### 动态规划总结：

1. 状态转移：`max(dp[i],max((i-j)*j,dp[i-j]*j));`和`max((i-j)*j,dp[i-j]*j);`两种
2. 初始化：不用从0开始。

#### [股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

![image-20220103193300268](E:\typora\剑指offer\image-20220103193300268.png)

```

```

#### [最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

![image-20220107121927362](E:\typora\剑指offer\image-20220107121927362.png)

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size();
        if(n==0)    return 0;
        unordered_map<char,int>map;
        vector<int>dp(n);
        dp[0]=1;
        map[s[0]]=0;
        int res=1;
        for(int j=1;j<n;j++){
            int i;
            if(map.count(s[j])!=0){
                i = map[s[j]];
            }
            else{
                i=-1;
            }
            map[s[j]]=j;
            if(dp[j-1]<j-i){
                dp[j]=dp[j-1]+1;
            }
            else{
                dp[j]=j-i;
            }
            res=max(res,dp[j]);

        }
        return res;

    }
};
```

进行最大值比较的时候，第一个数必须是循环之前最大的数。

### 双指针

#### [删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

![image-20220107135523841](E:\typora\剑指offer\image-20220107135523841.png)

写cur->next或者cur->val一定要确保cur不为空。

```
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
    ListNode* deleteNode(ListNode* head, int val) {
        if(head==NULL){
            return head;
        }
        if(head->val==val){
            return head->next;
        }
        for(ListNode* cur=head;cur && cur->next;cur=cur->next){

            if(cur->next!=NULL && cur->next->val==val){
                cur->next = cur->next->next;
            }
        }
        return head;

    }
};
```

#### [链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

![image-20220107140241830](E:\typora\剑指offer\image-20220107140241830.png)

```
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
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* cur=head;
        for(int i=0;i<k;i++){
            if(cur !=NULL)
                cur=cur->next;
        }
        ListNode* pre =head;
        for(pre,cur;cur;cur=cur->next,pre=pre->next){

        }
        return pre;


    }
};
```

#### [合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

![image-20220107142111031](E:\typora\剑指offer\image-20220107142111031.png)

```
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == NULL)  return l2;
        if(l2==NULL)    return l1;
        if(l1->val>l2->val){
            l2->next = mergeTwoLists(l1,l2->next);
            return l2;
        }
        else{
            l1->next = mergeTwoLists(l1->next,l2);
            return l1;
        }
    }
};
```

#### [ 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

![image-20220107143355159](E:\typora\剑指offer\image-20220107143355159.png)

```
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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lenA=0;
        int lenB=0;
        for(ListNode* cur=headA;cur;cur=cur->next){
            lenA++;
        }
        for(ListNode* cur=headB;cur;cur=cur->next){
            lenB++;
        }
        ListNode* curA=headA;
        ListNode* curB=headB;
        if(lenB>lenA){
            int temp = lenB;
            lenB=lenA;
            lenA=temp;

            ListNode* curtemp=curA;
            curA=curB;
            curB=curtemp;
        }
        int gap = lenA-lenB;
        for(int i=0;i<gap;i++){
            curA = curA->next;
        }
        for(curA,curB;curA;curA=curA->next,curB=curB->next){
            if(curA==curB){
                return curA;
            }
        }
        return NULL;
    }
};
```

#### [调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

![image-20220107160113451](E:\typora\剑指offer\image-20220107160113451.png)

```
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int n = nums.size();
        int left=0;
        int right=n-1;
        while(left<right){
            while(left<right && nums[left]%2==1)
                left++;
            while(left<right && nums[right]%2==0)
                right--;
            swap(nums[left],nums[right]);
        }
        return nums;
    }
};
```

#### [和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

![image-20220107162633317](E:\typora\剑指offer\image-20220107162633317.png)

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0;
        int right = n-1;
        while(left<right){
            if((nums[left]+nums[right])==target){
                return vector<int>{nums[left],nums[right]};
            }
            else if((nums[left]+nums[right])>target){
                right--;
            }
            else{
                left++;
            }
        }
        return vector<int>{-1,-1};

    }
};
```

#### [翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

![image-20220107164201204](E:\typora\剑指offer\image-20220107164201204.png)

```c++
class Solution {
public:
    string reverseWords(string s) {
        s+=' ';
        string temp="";
        vector<string>res;
        for(char ch:s){
            if(ch ==' '){
                if(!temp.empty()){
                    res.push_back(temp);
                    temp.clear();
                }
            }
            else{
                temp +=ch;
            }
        }
        string data="";
        int n = res.size();
        if(n == 0)
            return "";
        for(int i=n-1;i>=1;i--){
            data += res[i];
            data += ' ';
        }
        data +=res[0];
        return data;
    }
};
```

### 回溯

#### [ 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

![image-20220107172333825](E:\typora\剑指offer\image-20220107172333825.png)

```c++
class Solution {
public:
    bool backtrack(vector<vector<char>>& board, string word, vector<vector<bool>>& visited,int i, int j, int index){
        if(i<0 || j<0|| j>=board[0].size() || i>=board.size() || visited[i][j] || board[i][j]!=word[index])
            return false;
        if(index == word.size()-1){
            return true;
        }
        visited[i][j]=true;
        //不用返回路径，直接用||运算
        bool res = backtrack(board,word,visited,i+1,j,index+1) ||
                   backtrack(board,word,visited,i-1,j,index+1) ||
                   backtrack(board,word,visited,i,j+1,index+1) ||
                   backtrack(board,word,visited,i,j-1,index+1);
        visited[i][j] = false;
        return res;
    }
    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size();
        int n = board[0].size();
        vector<vector<bool>>visited(m,vector<bool>(n,false));
        //不知道起点，用双循环
        for(int i=0;i<m;i++){
            for(int j = 0; j<n; j++){
                if(backtrack(board,word,visited,i,j,0))
                    return true;
            }
        }
        return false;

    }
};
```

#### [机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)（不用回溯）

![image-20220117135305752](E:\typora\剑指offer\image-20220117135305752.png)

```
class Solution {
public:
    int getsum(int data){
        int res=0;
        while(data/10!=0){
            res = res+data%10;
            data = data/10;
        }
        res +=data;
        return res;
    }
    void backtrack(int m, int n, int k, int i, int j, int &res,vector<vector<bool>>&visited){
        if(i<0 || j<0 || i>=m || j>=n || visited[i][j])
            return ;
        if(getsum(i)+getsum(j)>k)
            return;
        visited[i][j]=true;
        res++;
        backtrack(m,n,k,i+1,j,res,visited);
        backtrack(m,n,k,i-1,j,res,visited);
        backtrack(m,n,k,i,j+1,res,visited);
        backtrack(m,n,k,i,j-1,res,visited);
    }
    int movingCount(int m, int n, int k) {
        if(m == 0 || n==0)
            return -1;
        int res = 0;
        vector<vector<bool>>visited(m,vector<bool>(n,false));
        backtrack(m,n,k,0,0,res,visited);
        return res;
    }
};
```

#### [二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

![image-20220117135349218](E:\typora\剑指offer\image-20220117135349218.png)

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>>res;
    vector<int>path;
    int sum =0;
    void backtrack(TreeNode* root, int target){
        if(root==nullptr)
            return;
        path.push_back(root->val);
        sum += root->val;
        if(target==sum && root->left ==nullptr && root->right == nullptr){
            res.push_back(path);
        }
        backtrack(root->left,target);
        backtrack(root->right,target);
        sum -= root->val;
        path.pop_back();   
    }
    vector<vector<int>> pathSum(TreeNode* root, int target) {
        if(root==nullptr)
            return vector<vector<int>>{};
        backtrack(root,target);
        return res;

    }
};
```

#### [二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

![image-20220117145714136](E:\typora\剑指offer\image-20220117145714136.png)

遍历一个链表，利用一个前驱，跟头结点

```
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* head;
    Node* pre;
    void backtrack(Node* root){
        if(root==NULL)
            return;
        backtrack(root->left);
        if(pre!=NULL){
            pre->right = root;
        }
        else{
            head = root;
        }
        root->left = pre;
        pre = root;
        backtrack(root->right);

    }
    Node* treeToDoublyList(Node* root) {
        if(root == nullptr) return nullptr;
        backtrack(root);
        head->left = pre;
        pre->right = head;
        return head;       
    }
};
```

#### [二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

![image-20220117163959010](E:\typora\剑指offer\image-20220117163959010.png)

中序遍历的倒序。

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int res=0;
    void dfs(TreeNode* root, int &k){
        if(root==NULL)
            return;
        dfs(root->right,k);
        k--;
        if(k==0){
            res = root->val;
        }
        dfs(root->left,k);
    }
    int kthLargest(TreeNode* root, int k) {
        dfs(root,k);
        return res;

    }
};
```

#### [二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

![image-20220118130402611](E:\typora\剑指offer\image-20220118130402611.png)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(p->val > q->val){
            TreeNode* temp;
            temp = p;
            p = q;
            q = temp;
        }
        while(root!=NULL){
            if(root->val <p->val)
                root = root->right;
            else if(root->val >q->val)
                root= root->left;
            else{
                break;
            }
        }
        return root;
    }
};
```

#### [二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

![image-20220118130935756](E:\typora\剑指offer\image-20220118130935756.png)

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL)
            return root;
        if(root == p || root == q)
            return root;
        TreeNode* left = lowestCommonAncestor(root->left,p,q);
        TreeNode* right = lowestCommonAncestor(root->right,p,q);
        if(left == NULL)
            return right;
        if(right == NULL)
            return left;
        return root;
        
    }
};
```



### 排序

#### [把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

![image-20220117165900038](E:\typora\剑指offer\image-20220117165900038.png)

```c++
class Solution {
public:
    struct cmp{
        bool operator()(string &x, string &y){
            return x+y<y+x;
        }
    };
    string minNumber(vector<int>& nums) {
        vector<string>str;
        string res;
        for(int x:nums){
            str.push_back(to_string(x));
        }
        sort(str.begin(),str.end(),cmp());
        for(string x:str){
            res += x;
        }
        return res;
    }
};
```

#### [扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

![image-20220118113908622](E:\typora\剑指offer\image-20220118113908622.png)

```
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int temp=0;
        for(int i=0;i<4;i++){
            if(nums[i]==0)  continue;
            if(nums[i]==nums[i+1])
                return false;
            temp += nums[i+1]-nums[i];
        }
        return temp<5;
    }
};
```

#### [最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

![image-20220118120729270](E:\typora\剑指offer\image-20220118120729270.png)

```
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        sort(arr.begin(),arr.end());
        vector<int>res;
        for(int i=0;i<k;i++){
            res.push_back(arr[i]);
        }
        return res;

    }
};
```

### 分治算法

#### [重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

![image-20220119173130811](E:\typora\剑指offer\image-20220119173130811.png)

`TreeNode* rebiuld(int pre_root_index, int in_left_index, int  in_right_index)`表示以pre_root_index索引的根节点（前序遍历），树的范围为[in_left_index,in_right_index] (中序遍历)。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int>preorder;
    unordered_map<int,int>map;
    TreeNode* rebiuld(int pre_root_index, int in_left_index, int  in_right_index){
        if(in_left_index > in_right_index)
            return NULL;
        TreeNode* root = new TreeNode(preorder[pre_root_index]);
        int i = map[preorder[pre_root_index]];
        root->left = rebiuld(pre_root_index+1,in_left_index,i-1);
        root->right = rebiuld(pre_root_index+i-in_left_index+1,i+1,in_right_index);
        return root;


    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        this->preorder = preorder;
        for(int i=0;i<inorder.size();i++){
            map[inorder[i]]=i;
        }
        return rebiuld(0,0,inorder.size()-1);

    }
};
```

#### [数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

![image-20220119211620366](E:\typora\剑指offer\image-20220119211620366.png)

快速幂： 

偶数：pow(x,n) = pow(x*x,n>>1)；

奇数：pow(x,n) = pow(x*x,n>>1) *x;

```c++
class Solution {
public:
    double myPow(double x, int n) {
        if(n == 0){
            return 1;
        }
        if(n== -1){
            return 1.0/x;
        }
        if(n%2){
            return myPow(x*x,n>>1)*x;
        }
        else{
            return myPow(x*x,n>>1);
        }
        return 0;
    }
};
```

#### [二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

![image-20220119211833394](E:\typora\剑指offer\image-20220119211833394.png)

```c++
class Solution {
public:
    bool recur(vector<int>& postorder, int i ,int j){
        if(i>j)
            return true;
        int p=i;
        while(postorder[p]<postorder[j]){
            p++;
        }
        int m = p;
        while(postorder[p]>postorder[j]){
            p++;
        }
        return p == j && recur(postorder,i,m-1) && recur(postorder,m,j-1);

    }
    bool verifyPostorder(vector<int>& postorder) {
        int  n  = postorder.size();
        return recur(postorder,0,n-1);
    }
};
```

### 位运算

#### [二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

![image-20220119214622977](E:\typora\剑指offer\image-20220119214622977.png)

一个一个数字判断，每次从末尾判断

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while(n){
            res += n&1;
            n = n>>1;
        }
        return res;
        
    }
};
```

#### [不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

![image-20220121141442127](E:\typora\剑指offer\image-20220121141442127.png)

`unsigned int` 正数保持不变，负数输出补码

```
class Solution {
public:
    int add(int a, int b) {
        while(b!=0){
            int temp = a^b;
            int carray = (unsigned int)(a&b)<<1;
            a = temp;
            b = carray;

        }
        return a;
    }
};
```

#### [数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

![image-20220121144946031](E:\typora\剑指offer\image-20220121144946031.png)

```c++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int z=0;
        int m = 1;
        int x=0;
        int y=0;
        for(int n:nums){
            z ^= n;
        }
        while((z&m)==0){
            m <<= 1;
        }
        for(int n:nums){
            if(n&m){
                x^=n;
            }
            else{
                y^=n;
            }
        }
        return vector<int>{x,y};

    }
};
```

#### [数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

![image-20220122140337964](E:\typora\剑指offer\image-20220122140337964.png)

### 数学

#### [数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

![image-20220122141348178](E:\typora\剑指offer\image-20220122141348178.png)

```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int,int>map;
        for(int x:nums){
            map[x]++;
        }
        for(int x:nums){
            if(map[x]>nums.size()/2){
                return x;
            }
        }
        return 0;
    }
};
```

#### [构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

![image-20220122144225520](E:\typora\剑指offer\image-20220122144225520.png)

```
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        vector<int>res;
        vector<int>left(a.size(),1);
        vector<int>right(a.size(),1);
        for(int i=1;i<a.size();i++){
            left[i]= left[i-1]*a[i-1];
        }
        for(int i=a.size()-2;i>=0;i--){
            right[i] = right[i+1]*a[i+1];
        }
        for(int i=0;i<a.size();i++){
            res.push_back(left[i]*right[i]);
        }
        return res;

    }
};
```

#### [ 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)（动态规划）

![image-20220122150135845](E:\typora\剑指offer\image-20220122150135845.png)

```c++
class Solution {
public:
    int cuttingRope(int n) {
        vector<int>dp(n+1,1);
        dp[2]=1;
        for(int i=3;i<=n;i++){
            for(int j=1;j<i;j++){//每次切去多少
                dp[i] = max(dp[i],max(j*(i-j),dp[i-j]*j));
            }
        }
        return dp[n];

    }
};
```

#### [和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

![image-20220122154936995](E:\typora\剑指offer\image-20220122154936995.png)

```c++
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        int left=1;
        int right = 1;
        vector<vector<int>>res;
        vector<int>window;
        int temp=1;
        while(right<=(target+1)/2){
            if(temp<target){
                right++;
                temp += right;
            }
            else if(temp>target){
                temp -= left;
                left++;
            }
            else{
                for(int i= left;i<=right;i++){
                    window.push_back(i);
                }
                res.push_back(window);
                window.clear();
                right++;
                temp += right;
            }
        }
        return res;

    }
};
```

#### [ 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

![image-20220123154256051](E:\typora\剑指offer\image-20220123154256051.png)

dp[i] 表示i个数字最后剩下的结果

```
class Solution {
public:
    int lastRemaining(int n, int m) {
        vector<int>dp(n+1);
        dp[1]=0;
        for(int i=2;i<=n;i++){
            dp[i] = (dp[i-1]+m)%i;
        }
        return dp[n];
    }
};
```

### 模拟

#### [顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

![image-20220123154411004](E:\typora\剑指offer\image-20220123154411004.png)

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if(matrix.size()==0){
            return vector<int>{};
        }
        int m = matrix.size();
        int n = matrix[0].size();
        vector<int>res;
        int up=0;
        int down = m-1;
        int left=0;
        int right=n-1;
        while(up<=down && left<=right){
            for(int j=left;j<=right;j++){
                res.push_back(matrix[up][j]);
            }
            up++;
            for(int i=up;i<=down;i++){
                res.push_back(matrix[i][right]);
            }
            right--;
            for(int j=right;j>=left && up<=down;j--){
                res.push_back(matrix[down][j]);
            }
            down--;
            for(int i=down;i>=up && left<=right;i--){
                res.push_back(matrix[i][left]);
            }
            left++;
        }
        return res;
    }
};
```

#### [栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

![image-20220123164048951](E:\typora\剑指offer\image-20220123164048951.png)

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int>sta;
        int i=0;
        for(int x:pushed){
            sta.push(x);
            while(!sta.empty() && sta.top()==popped[i]){
                sta.pop();
                i++;
            }
        }
        return sta.empty();
    }
};
```

#### [滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)（单调队列）

![image-20220124141036373](E:\typora\剑指offer\image-20220124141036373.png)

```
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        if(n==0){
            return vector<int>{};
        }
        deque<int>que;
        vector<int>res;
        for(int j=0,i=1-k;j<n;i++,j++){
            if(i>0 &&   nums[i-1]==que.front()){
                que.pop_front();
            }
            while(!que.empty() && nums[j]>que[que.size()-1]){
                que.pop_back();
            }
            que.push_back(nums[j]);
            if(i>=0){
                res.push_back(que[0]);
            }
        }
        return res;

    }
};
```

#### [队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

![image-20220124143811143](E:\typora\剑指offer\image-20220124143811143.png)

```
class MaxQueue {
public:
    queue<int>que;
    deque<int>maxque;
    MaxQueue() {

    }
    
    int max_value() {
        if(que.empty()){
            return -1;
        }
        return maxque.front();

    }
    
    void push_back(int value) {
        que.push(value);
        while(!maxque.empty() && maxque[maxque.size()-1]<value){
            maxque.pop_back();
        }
        maxque.push_back(value);

    }
    
    int pop_front() {
        if(que.empty()){
            return -1;
        }
        int temp = que.front();
        que.pop();
        if(temp == maxque.front()){
            maxque.pop_front();
        }
        return temp;
    }
};
```

#### [字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

![image-20220124154420274](E:\typora\剑指offer\image-20220124154420274.png)
