# Greedy
### Joe Lin

## *455. Assign Cookies*
Input: g = [1,2,3], s = [1,1]
Output: 1
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
```
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        //int res = 0;
        int i = 0;
        int j = 0;
        while(i < g.length && j < s.length){
            if(g[i] <= s[j]){
                //res++;
                i++;
                j++;
            } else {
                j++;
            }
        }
        return i;
    }
}
```

## *376. Wiggle Subsequence*
Input: nums = [1,17,5,10,13,15,10,5,16,8]
Output: 7
Explanation: There are several subsequences that achieve this length.
One is [1, 17, 10, 13, 10, 16, 8] with differences (16, -7, 3, -3, 6, -8).
```
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return nums.length;
        }
        /*当前差值
        int curDiff = 0;
        //上一个差值
        int preDiff = 0;
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            //得到当前差值
            curDiff = nums[i] - nums[i - 1];
            //如果当前差值和上一个差值为一正一负
            //等于0的情况表示初始时的preDiff
            if ((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)) {
                count++;
                preDiff = curDiff;
            }
        }
        return count;
        */
        int count = 1;
        int[] diff = new int[nums.length - 1];
        for(int i = 0; i < diff.length; i++){
            diff[i] = nums[i+1] - nums[i];
        }
        int pre = 0;
        for(int i = 0; i < diff.length; i++){
            if ((diff[i] > 0 && pre <= 0) || (diff[i] < 0 && pre >= 0)) {
                count++;
                pre = diff[i];
            }
        }
        return count;
    }
}
```

## *53. Maximum Subarray*
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

method 1: greedy
```
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 1){
            return nums[0];
        }
        
        int res = Integer.MIN_VALUE;
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
            if(sum > res){
                res = sum;
            }
            if(sum <= 0){
                sum = 0;
            }
            
        }
        return res;
    }
}
```

method 2: dp
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        vector<int> dp(nums.size(), 0); 
        dp[0] = nums[0];
        int result = dp[0];
        for (int i = 1; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1] + nums[i], nums[i]); // 状态转移公式
            if (dp[i] > result) result = dp[i]; // result 保存dp[i]的最大值
        }
        return result;
    }
};

```

## *55. Jump Game*
```
class Solution {
    public boolean canJump(int[] nums) {
        if(nums.length == 1){
            return true;
        }
        int reach = 0;
        for(int i = 0; i <= reach; i++){
          
            reach = Math.max(i + nums[i],reach);
       
            
            if(reach >= nums.length - 1) return true;
        }
        return false;
        
    }
}
```

## *45. Jump Game II*
```
class Solution {
    public int jump(int[] nums) {
        if (nums == null || nums.length == 0 || nums.length == 1) {
            return 0;
        }
       
        int count=0;
        int curDistance = 0;
        int maxDistance = 0;
        for (int i = 0; i < nums.length; i++) {
            //update
            maxDistance = Math.max(maxDistance,i+nums[i]);
            if (maxDistance>=nums.length-1){
                count++;
                break;
            }
            if (i==curDistance){
                curDistance = maxDistance;
                count++;
            }
        }
        return count;
    }
}

```

## *1005. Maximize Sum Of Array After K Negations*
Input: nums = [4,2,3], k = 1
Output: 5
Explanation: Choose indices (1,) and nums becomes [4,-2,3].
```
class Solution {
    public int largestSumAfterKNegations(int[] A, int K) {
        if (A.length == 1) return A[0];
        Arrays.sort(A);
        int sum = 0;
        int idx = 0;
        for (int i = 0; i < K; i++) {
            if (i < A.length - 1 && A[idx] < 0) {
                A[idx] = -A[idx];
                if (A[idx] >= Math.abs(A[idx + 1])) idx++;
                continue;
            }
            A[idx] = -A[idx];
        }

        for (int i = 0; i < A.length; i++) {
            sum += A[i];
        }
        return sum;
    }
}

```

## *134. Gas Station*
```
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        //int curr = 0;
        for(int i = 0; i < gas.length; i++){
            if(gas[i] >= cost[i]){
                if(yes(gas, cost,i)) return i;
            }
        }
        return -1;
        
    }
    
    private boolean yes(int[] gas, int[] cost, int curr){
        int tank = 0;
        for(int i = curr; i < curr+gas.length; i++){
            int index = i >= gas.length ? i - gas.length : i;
            
            tank += gas[index];
            if(tank < cost[index]){
                return false;
            }
            tank -= cost[index];
        }
        return true;
    }
}
```


```
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for (int i = 0; i < gas.length; i++) {
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if (curSum < 0) {   // 
                start = i + 1;  // update start posi
                curSum = 0;    
            }
        }
        if (totalSum < 0) return -1; //impossible to run
        return start;
    }
}

```

## *135. Candy*
```
class Solution {
    public int candy(int[] ratings) {
        int[] candy = new int[ratings.length];
        for(int i = 0; i < candy.length; i++){
            candy[i] = 1;
        }
        //first greedy
        for(int i = 0; i < ratings.length - 1; i++){
            if(ratings[i] < ratings[i + 1]){
                candy[i+1] = candy[i] + 1;
            }
        }
        
        //second greedy
        for(int i = ratings.length - 1; i > 0; i--){
            if(ratings[i] < ratings[i - 1]){
                candy[i - 1] = Math.max(candy[i] + 1,candy[i - 1]);
            }
        }
        int res = 0;
        for(int i = 0; i < candy.length;i++){
            res+= candy[i];
        }
        return res;
    }
}
```


## *135. Candy*
```

```


## *135. Candy*
```

```


## *135. Candy*
```

```


## *135. Candy*
```

```




