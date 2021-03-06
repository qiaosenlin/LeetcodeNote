# Backtracking
### Joe Lin

## *78. Subsets*
input: nums = [1,2,3]
output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> sets = new ArrayList<>();
        Arrays.sort(nums);
        helpfn(nums, sets, new ArrayList<Integer>(), 0);
        return sets;
    }
    
    private void helpfn(int[] nums,List<List<Integer>> sets,List<Integer> rec, int posi){
        sets.add(new ArrayList<>(rec));
    
        for(int i = posi; i < nums.length; i++){
            rec.add(nums[i]);
            helpfn(nums,sets,rec,i + 1);
            rec.remove(rec.size() - 1);
        }
        return;
    }
}
```

## *90. Subsets II*
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> sets = new ArrayList<>();
        Arrays.sort(nums);
        
        //deal with duplications
        boolean[] used = new boolean[nums.length];
        
        helpfn(nums, sets, new ArrayList<Integer>(), 0, used);
        return sets;
    }
    
    private void helpfn(int[] nums,List<List<Integer>> sets,List<Integer> rec, int posi,  boolean[] used){
        sets.add(new ArrayList<>(rec));
        
        for(int i = posi; i < nums.length; i++){
            if(i>0 && nums[i] == nums[i-1] && !used[i-1]){
                continue;
            }
            rec.add(nums[i]);
            used[i] = true;
            helpfn(nums,sets,rec,i + 1,used);
            rec.remove(rec.size() - 1);
            used[i] = false;
        }
        return;
    }
}
```

## *491. Increasing Subsequences*
Input: nums = [4,6,7,7]
Output: [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]

```
class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        //boolean[] used = new boolean[nums.length];
        helpfn(nums, res, 0, new ArrayList<Integer>());
        return res;
    }
    
    private void helpfn(int[] nums,List<List<Integer>> res, int posi, List<Integer> rec){
        //add
        if(rec.size() >= 2){
            res.add(new ArrayList<>(rec));
        }
        Set<Integer> set = new HashSet<Integer>();
        //backtracking
        for(int i = posi; i < nums.length; i++){
            if(posi > 0 && nums[i] < nums[posi - 1]){
                continue;
            }
            if(set.contains(nums[i])){
                continue;
            }
            rec.add(nums[i]);
            set.add(nums[i]);
            //used[i] = true;
            helpfn(nums,res,i + 1,rec);
            rec.remove(rec.size() - 1);
            //set.remove(nums[i]); 本层不需要remove，下一层新建set

        }
    }
}
```

## *47. Permutations II*
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
 
 Method1: order
 
 ```
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        int[] used = new int[nums.length];
        helpfn(nums,res,new ArrayList<Integer>(),used);
        return res;
    }
    
    private void helpfn(int[] nums, List<List<Integer>> res, List<Integer> rec,int[] used){
        if(rec.size() == nums.length){
            res.add(new ArrayList<>(rec));
            return;
        }
        for(int i = 0; i < nums.length; i++){
            if(used[i] == 1){
                continue;
            }
            if(i > 0 && nums[i] == nums[i - 1] && used[i-1] == 0){
                continue;
            }
            rec.add(nums[i]);
            used[i] = 1;
            helpfn(nums,res,rec,used);
            used[i] = 0;
            rec.remove(rec.size() -1 );
        }
        
    }
}
```

Method2: without sort
```
 class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        //Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        int[] used = new int[nums.length];
        helpfn(nums,res,new ArrayList<Integer>(),used);
        return res;
    }
    
    private void helpfn(int[] nums, List<List<Integer>> res, List<Integer> rec,int[] used){
        if(rec.size() == nums.length){
            res.add(new ArrayList<>(rec));
            return;
        }
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < nums.length; i++){
            if(used[i] == 1){
                continue;
            }
            /*if(i > 0 && nums[i] == nums[i - 1] && used[i-1] == 0){
                continue;
            }
            */
            if(set.contains(nums[i])){
                continue;
            }
            rec.add(nums[i]);
            set.add(nums[i]);
            used[i] = 1;
            helpfn(nums,res,rec,used);
            used[i] = 0;
            rec.remove(rec.size() -1 );
        }
        
    }
}
```
 
 
 ## *332. Reconstruct Itinerary*
 图论dfs的回溯写法
 ```
 class Solution {
    private Deque<String> res;
    private Map<String, Map<String, Integer>> map;

    private boolean backTracking(int ticketNum){
        if(res.size() == ticketNum + 1){
            return true;
        }
        String last = res.getLast();
        if(map.containsKey(last)){//防止出现null
            for(Map.Entry<String, Integer> target : map.get(last).entrySet()){
                int count = target.getValue();
                if(count > 0){
                    res.add(target.getKey());
                    target.setValue(count - 1);
                    if(backTracking(ticketNum)) return true;
                    res.removeLast();
                    target.setValue(count);
                }
            }
        }
        return false;
    }

    public List<String> findItinerary(List<List<String>> tickets) {
        map = new HashMap<String, Map<String, Integer>>();
        res = new LinkedList<>();
        for(List<String> t : tickets){
            Map<String, Integer> temp;
            if(map.containsKey(t.get(0))){
                temp = map.get(t.get(0));
                temp.put(t.get(1), temp.getOrDefault(t.get(1), 0) + 1);
            }else{
                temp = new TreeMap<>();//升序Map
                temp.put(t.get(1), 1);
            }
            map.put(t.get(0), temp);

        }
        res.add("JFK");
        backTracking(tickets.size());
        return new ArrayList<>(res);
    }
}
 ```
 
 ## *51. N-Queens*
  ```
  class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> res = new ArrayList<>();
        int[] queens = new int[n];
        Arrays.fill(queens, -1);
        Set<Integer> col = new HashSet<Integer>();
        Set<Integer> d1 = new HashSet<Integer>();
        Set<Integer> d2 = new HashSet<Integer>();
        
        back(n,res,0,queens,col,d1,d2);
        return res;
    }
    
    private void back(int n,  List<List<String>> res, int row,int[] queens, Set<Integer> col,Set<Integer> d1,Set<Integer> d2){
        //1.stop
        if(row == n){
            //generate
            List<String> board = generate(queens);
            res.add(new ArrayList<>(board));
        }
        
        //2.loop
        for(int i = 0; i < n; i++){
            if(col.contains(i)){
                continue;
            }
            int d_1 = row - i;
            if(d1.contains(d_1)){
                continue;
            }
            int d_2 = row + i;
            if(d2.contains(d_2)){
                continue;
            }
            queens[row] = i;
            col.add(i);
            d1.add(d_1);
            d2.add(d_2);
            back(n, res, row+1,queens,col,d1,d2);
            queens[row] = -1;
            col.remove(i);
            d1.remove(d_1);
            d2.remove(d_2);
        }
    }
    
    private List<String> generate(int[] q){
        List<String> board = new ArrayList<String>();
        int n = q.length;
        for(int i = 0; i < n; i++){
            char[] tmp = new char[n];
            Arrays.fill(tmp,'.');
            tmp[q[i]] = 'Q';
            board.add(new String(tmp));
        }
        return board;
    }
}


   ```
   
 
 
  ## *37. Sudoku Solver*
  ``` 
   class Solution {
    public void solveSudoku(char[][] board) {
        solveSudokuHelper(board);
    }

    private boolean solveSudokuHelper(char[][] board){
       
        for (int i = 0; i < 9; i++){ // 遍历行
            for (int j = 0; j < 9; j++){ // 遍历列
                if (board[i][j] != '.'){ // 跳过原始数字
                    continue;
                }
                for (int k = 1; k <= 9; k++){ // (i, j) 这个位置放k是否合适
                    if (isValidSudoku(i, j, k, board)){
                        board[i][j] = (char)(k + '0');
                        if (solveSudokuHelper(board)){ // 如果找到合适一组立刻返回
                            return true;
                        }
                        board[i][j] = '.';
                    }
                }
                // 9个数都试完了，都不行，那么就返回false
                return false;
                // 因为如果一行一列确定下来了，这里尝试了9个数都不行，说明这个棋盘找不到解决数独问题的解！
                // 那么会直接返回， 「这也就是为什么没有终止条件也不会永远填不满棋盘而无限递归下去！」
            }
        }
        // 遍历完没有返回false，说明找到了合适棋盘位置了
        return true;
    }

    /**
     * 判断棋盘是否合法有如下三个维度:
     *     同行是否重复
     *     同列是否重复
     *     9宫格里是否重复
     */
    private boolean isValidSudoku(int row, int col, int val, char[][] board){
        // 同行是否重复
        for (int i = 0; i < 9; i++){
            if (board[row][i] -'0' == val){
                return false;
            }
        }
        // 同列是否重复
        for (int j = 0; j < 9; j++){
            if (board[j][col] -'0' == val){
                return false;
            }
        }
        // 9宫格里是否重复
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = startRow; i < startRow + 3; i++){
            for (int j = startCol; j < startCol + 3; j++){
                if (board[i][j] -'0' == val){
                    return false;
                }
            }
        }
        return true;
    }
}
   ```
