---
title: "Leetcode算法"
date: "2024-1-8 17:26:36"
description: "算法刷题"
categories: 算法例题
---

# 双指针
## 相向双指针
leetcode 167. 两数之和 II - 输入有序数组
```c++
vector<int> twoSum(vector<int>& numbers, int target) {
    int n = numbers.size();
    int left = 0, right = n - 1;
    while(left < right){
        if(numbers[left] + numbers[right] == target){
            return {left + 1, right + 1};
        }
        else if(numbers[left] + numbers[right] > target){
            right--;
        }
        else if(numbers[left] + numbers[right] < target){
            left++;
        }
    }
    return {};
}
```
## 同向双指针
leetcode 209. 长度最小的子数组
```c++
int minSubArrayLen(int target, vector<int>& nums) {
    int n = nums.size();
    int left = 0;
    int ans = n + 1;
    int s = 0;
    for(int right = 0; right < n; right++){
        s += nums[right];
        while(s - nums[left] >= target){
            s -= nums[left];
            left++;
        }
        if(s >= target){
            ans = min(ans, right - left + 1);
        }
    }
    return ans <= n? ans : 0;
}
```
# 二分查找
leetcode 34. 在排序数组中查找元素的第一个和最后一个位置
```c++
vector<int> searchRange(vector<int>& nums, int target) {
    int n = nums.size();
    int l1 = -1,l2 = -1;
    // 第一次二分
    int left = 0;
    int right = n - 1;
    // 二分查找到最开始的下标
    while(left <= right){
        int mid = left + (right - left)/2;
        if(nums[mid] < target){
            left = mid + 1;
        }
        else{
            right = mid - 1;
        }
    }
    // 如果target存在，就进行赋值
    if(left < n && nums[left] == target)
        l1 = left;
    else
        return {-1, -1};
    // 第二次二分
    left = 0;
    right = n - 1;
    // 二分查找到最后的下标
    while(left <= right){
        int mid = left + (right - left)/2;
        if(nums[mid] <= target){
            left = mid + 1;
        }
        else{
            right = mid - 1;
        }
    }
    // 如果target存在，就进行赋值
    if(right >= 0 && nums[right] == target)
        l2 = right;
    return {l1, l2};
}
```
# 反转链表
leetcode 206. 反转链表
```c++
ListNode* reverseList(ListNode* head) {
    ListNode* pre = nullptr;
    ListNode* cur = head;
    while(cur){
        ListNode* next = cur->next;
        cur->next = pre;
        pre = cur;
        cur = next;
    }
    return pre;
}
```
# 快慢指针
leetcode 141. 环形链表
```c++
bool hasCycle(ListNode *head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while(fast && fast->next){
        slow = slow->next;
        fast = fast->next->next;
        if(slow == fast)
            return true;
    }
    return false;
}
```
# 前后指针
leetcode 19. 删除链表的倒数第 N 个结点
```c++
ListNode *removeNthFromEnd(ListNode *head, int n) {
    ListNode *dummy = new ListNode(0, head);
    ListNode *left = dummy, *right = dummy;
    while (n--)
        right = right->next;
    while (right->next) {
        left = left->next;
        right = right->next;
    }
    left->next = left->next->next;
    return dummy->next;
}
```
# 单调栈
leetcode 739. 每日温度
```c++
vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> ans(n,0);
    stack<int> st;
    for(int i = n - 1; i >= 0; i--){
        int t = temperatures[i];
        while(!st.empty() && t >= temperatures[st.top()]){
            st.pop();
        }
        if(!st.empty())
            ans[i] = st.top() - i;
        st.push(i);
    }
    return ans;
}
```
# 单调队列/双端队列
leetcode 239. 滑动窗口最大值
```c++
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> ans;
    deque<int> q;
    for(int i = 0; i < nums.size(); i++){
        while(!q.empty() && nums[q.back()] <= nums[i]){
            q.pop_back();
        }
        q.push_back(i);
        if(i - q.front() + 1 > k){
            q.pop_front();
        }
        if(i >= k - 1){
            ans.push_back(nums[q.front()]);
        }
    }
    return ans;
}
```
# 0-1背包
有n个物品，第i个物品的重量为w[i]，价值为v[i]。每个物品至多选一个，求体积和不超过capacity时的最大价值和。
```c
int knapsack(int n, int capacity, vector<int>& w, vector<int>& v){
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));
    for(int i = 1; i <= n; i++){
        for(int j = 0; j <= capacity; j++){
            if(j >= w[i - 1]){
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i - 1]] + v[i - 1]);
            }
            else{
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    return dp[n][capacity];
}
```
# 完全背包
有n种物品，第i种物品的重量为w[i]，价值为v[i]。每个物品无限次重复选，求体积和不超过capacity时的最大价值和。
```c
int knapsack(int n, int capacity, vector<int>& w, vector<int>& v){
    vector<int> dp(capacity + 1, 0);
    for(int i = 1; i <= n; i++){
        for(int j = w[i - 1]; j <= capacity; j++){
            dp[j] = max(dp[j], dp[j - w[i - 1]] + v[i - 1]);
        }
    }
    return dp[capacity];
}
```
# 最长公共子序列
leetcode 1143. 最长公共子序列
```c++
int longestCommonSubsequence(string text1, string text2) {
    int m = text1.size();
    int n = text2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for(int i = 1; i <= m; i++){
        for(int j = 1; j <= n; j++){
            if(text1[i - 1] == text2[j - 1]){
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }
            else{
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[m][n];
}
```
# 相交链表
leetcode 160. 相交链表
```c
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    if (headA == NULL || headB == NULL) {
        return NULL;
    }
    struct ListNode *pA = headA, *pB = headB;
    while (pA != pB) {
        pA = pA == NULL ? headB : pA->next;
        pB = pB == NULL ? headA : pB->next;
    }
    return pA;
}
```
# 两个非重叠子数组的最大和
leetcode 1031. 两个非重叠子数组的最大和
```c
int maxSumTwoNoOverlap(int* nums, int numsSize, int firstLen, int secondLen) {
    // 前缀和
    int pre[numsSize+1];
    pre[0] = 0;
    for(int i=0;i<numsSize;i++){
        pre[i+1] = pre[i] + nums[i];
    }
    int ans = 0;
    
    int maxfirst1 = -1;
    int maxsecond1 = -1;
    int maxfirst2 = -1;
    int maxsecond2 = -1;
    for(int i=firstLen+secondLen;i<=numsSize;i++){
        // 左firstLen 右secondLen
        maxsecond1 = pre[i] - pre[i-secondLen];
        if(pre[i-secondLen]-pre[i-secondLen-firstLen]>maxfirst1){
            maxfirst1 = pre[i-secondLen]-pre[i-secondLen-firstLen];
        }
        ans = fmax(ans,maxfirst1+maxsecond1);
        // 左secondLen 右firstLen
        maxfirst2 = pre[i]-pre[i-firstLen];
        if(pre[i-firstLen]-pre[i-firstLen-secondLen]>maxsecond2){
            maxsecond2 = pre[i-firstLen]-pre[i-firstLen-secondLen];
        }
        ans = fmax(ans,maxfirst2+maxsecond2);
    }
    return ans;
}
```
# 笨阶乘
leetcode 1006. 笨阶乘
```c
int clumsy(int n) {
    int stack[n], top=0;
    stack[top++] = n;
    n = n-1;
    int index = 0;
    while(n){
        // *
        if(index==0){
            stack[top-1] *= n;
        }
        else if(index==1){
            stack[top-1] /= n;
        }
        else if(index==2){
            stack[top++] = n;
        }
        else if(index==3){
            stack[top++] = -n;
        }
        index = (index+1)%4;
        n--;
    }

    int sum = 0;
    for(int i=0;i<top;i++){
        sum += stack[i];
    }

    return sum;
}
```
# 阶乘后的零的个数
leetcode 172. 阶乘后的零
```c
int trailingZeroes(int n) {
    int ans = 0;
    while(n){
        n /= 5;
        ans+=n;
    }
    return ans; 
}
```
# Pow(x, n)
leetcode 50. Pow(x, n)
```c
double quickMul(double x, long long N){
    if(N==0){
        return 1.0;
    }
    double y = quickMul(x, N/2);
    return N%2 == 0?y*y:y*y*x;
}
double myPow(double x, int n) {
    long long N = n;

    return N>=0?quickMul(x,N):1.0/quickMul(x,-N);
}
```
# 只出现一次的数字 II
leetcode 137. 只出现一次的数字 II
```c
int singleNumber(int* nums, int numsSize) {
    int ans = 0;
    for(int i=0;i<32;i++){
        int cnt = 0;
        for(int j=0;j<numsSize;j++){
            cnt += (nums[j]>>i) & 1;
        }
        if(cnt%3!=0)
            ans |= (1u<<i);
    }

    return ans;
}
```
# 腐烂的橘子
leetcode 994. 腐烂的橘子
```c
int orangesRotting(int** grid, int gridSize, int* gridColSize) {
    int count = 0;
    while(1){
        int end = 1;
        for(int i=0;i<gridSize;i++){
            for(int j=0;j<gridColSize[i];j++){
                if(grid[i][j]==3){
                    grid[i][j] = 2;
                }
                else if(grid[i][j]==1){
                    if((i<gridSize-1&&grid[i+1][j]==2)||(j<gridColSize[i]-1&&grid[i][j+1]==2)){
                        grid[i][j] = 2;
                        end = 0;
                    }
                }
                else if(grid[i][j]==2){
                    if(i<gridSize-1&&grid[i+1][j]==1){
                        grid[i+1][j] = 3;
                        end = 0;
                    }
                    if(j<gridColSize[i]-1&&grid[i][j+1]==1){
                        grid[i][j+1] = 3;
                        end = 0;
                    }
                }
            }
        }
        if(end){
            break;
        }
        else{
            count++;
        }
    }
    for (int i = 0; i < gridSize; i++)
		for (int j = 0; j < gridColSize[i]; j++)
			if (grid[i][j] == 1)
				return -1;
	return count;
}
```
# 航班预订统计(差分数组)
leetcode 1109. 航班预订统计
```c
int* corpFlightBookings(int** bookings, int bookingsSize, int* bookingsColSize, int n, int* returnSize) {
    int* ans = (int*)malloc(sizeof(int)*n);
    memset(ans,0,sizeof(int)*n);
    *returnSize = n;
    for(int i=0;i<bookingsSize;i++){
        int left = bookings[i][0];
        int right = bookings[i][1];
        // increase
        ans[left-1] += bookings[i][2];
        if(right<n)
            ans[right] -= bookings[i][2];
    }
    for(int i=1;i<n;i++){
        ans[i] += ans[i-1];
    }
    return ans;
}
```
# 单调递增的数字
leetcode 738. 单调递增的数字
```c
int monotoneIncreasingDigits(int n) {
    if(n<10){
        return n;
    }
    int arr[15];
    int k = 0;
    while(n){
        arr[k++] = n%10;
        n = n/10;
    }
    int flag = -1;
    for(int i=0;i<k-1;i++){
        if(arr[i]<arr[i+1]){
            flag = i;
            arr[i+1]--;
        }
    }
    for(int i=flag;i>=0;i--){
        arr[i] = 9;
    }
    int ans = 0;
    for(int i=k-1;i>=0;i--){
        if(ans==0&&arr[i]==0){
            continue;
        }
        else{
            ans *= 10;
            ans += arr[i];
        }
    }

    return ans;
}
```
# 快乐数
leetcode 202. 快乐数
```c
int bitSquareSum(int n){
    int sum=0;
    while(n>0){
        int bit = n%10;
        sum+=bit*bit;
        n=n/10;
    }
    return sum;
}

bool isHappy(int n) {
    int slow = n, fast = n;
    do{
        slow = bitSquareSum(slow);
        fast = bitSquareSum(fast);
        fast = bitSquareSum(fast);
    }while(slow!=fast);

    return slow==1;
}
```
# 最大间距
leetcode 164. 最大间距
```c
// 基数排序
int maximumGap(int* nums, int numsSize) {
    if(numsSize==1){
        return 0;
    }
    // 基数排序
    // 找到最大值
    int maxval = INT_MIN;
    for(int i=0;i<numsSize;i++){
        maxval = fmax(maxval,nums[i]);
    }
    int buffer[numsSize];
    int exp = 1;
    while(maxval>=exp){
        int cnt[10] = {0};
        // 按照该位置进行排序
        for(int i=0;i<numsSize;i++){
            int digit = nums[i]/exp;
            cnt[digit%10]++;
        }
        for(int i=1;i<10;i++){
            cnt[i] += cnt[i-1];
        }

        // 放入桶内
        for(int i=numsSize-1;i>=0;i--){
            int digit = nums[i]/exp;
            digit = digit%10;
            buffer[cnt[digit]-1] = nums[i];
            cnt[digit]--;
        }

        // 得到第i位的排列
        for(int i=0;i<numsSize;i++){
            nums[i] = buffer[i];
        }

        exp*=10;
    }

    maxval = 0;
    for(int i=1;i<numsSize;i++){
        maxval = fmax(maxval,nums[i]-nums[i-1]);
    }
    return maxval;
}
```
# 格雷编码
leetcode 89. 格雷编码
```c
int* grayCode(int n, int* returnSize) {
    int* ans = malloc(sizeof(int)*(int)pow(2,n));
    ans[0] = 0;
    int k=0;
    int j=1;
    while(j<pow(2,n)){
        for(int i=k;i>=0;i--){
            ans[i] = ans[i] << 1;
            ans[j++] = ans[i]+1;
        }
        k = j-1;
    }

    *returnSize = j;
    return ans;
}
```
对于qsort函数，cmp()会有三种返回值:
1、返回一个正数：a排列在b之后；
2、返回0：a、b相等；
3、返回一个负数：a排在b之前；
# 位运算 两整数之和
leetcode 371. 两整数之和
```c
int getSum(int a, int b) {
    while(b!=0){
        unsigned int carry = (unsigned int)(a&b) << 1;
        a = a^b;
        b = carry;
    }
    return a;
}
```
# 消除游戏
leetcode 390. 消除游戏
```c
int lastRemaining(int n) {
    // n=1的情况
    if(n==1){
        return 1;
    }
    // 记录 cnt为总的个数
    int cnt = n;
    // 步长
    int step = 1;
    // 删除方向
    int k = 0;
    // 首项和尾项
    int a1 = 1;
    int an = n;
    // 知道删除后总数变为1
    while(cnt>1){
        if(k%2==0){
            if(cnt%2==0){
                a1 += step;
            }
            else{
                a1 += step;
                an -= step;
            }
        }
        else{
            if(cnt%2==0){
                an -= step;
            }
            else{
                a1 += step;
                an -= step;
            }
        }
        step *= 2;
        k++;
        cnt = cnt/2;
    }

    return an;
}
```

