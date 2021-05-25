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
 
