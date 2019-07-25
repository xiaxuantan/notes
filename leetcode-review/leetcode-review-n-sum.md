### 1. Two Sum

无序数组，找数组里面的两个数，其和等于一个给定的target值。

**Solution**

建立一个值到索引的哈希表

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = nums.length - 1; i >= 0; i--) {
            if (map.containsKey(target - nums[i])) {
                return new int[]{i, map.get(target - nums[i])};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}
```

###15. 3Sum

给定无序数组，找到所有的三元组，使得其和等于0。

**Solution 1**

先排序，建立值到索引的哈希表，再枚举三元组的前两个数，复杂度O(n^2)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++)
            map.put(nums[i], i);
        List ans = new ArrayList<>();
        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < nums.length - 1; j++) {
                if (nums[i] + nums[j] > 0) break;
                if (j - i > 1 && nums[j] == nums[j - 1]) continue;
                if (!map.containsKey(-nums[i] - nums[j])) continue;
                if (map.get(-nums[i] - nums[j]) <= j) continue;
                ans.add(new int[]{nums[i], nums[j], -nums[i] - nums[j]});
            }
        }
        return ans;
    }
}
```

**Solution 2**

先排序，枚举第一个数，用两个指针收缩第二第三个数。

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int i, j, k;
        for (i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            j = i + 1;
            k = nums.length - 1;
            while (j < k) {
                if (nums[i] + nums[j] + nums[k] < 0) j++;
                else if (nums[i] + nums[j] + nums[k] > 0) k--;
                else {
                    ans.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    j++;
                    while (nums[j] == nums[j - 1] && j < k) j++;
                }
            }
        }
        return ans;
    }
}
```

### 16. 3Sum Closest

给定无序数组，找到某个三元组使得其和和给定的target值最接近

**Solution**

思路和3Sum一样，枚举第一个，收缩头尾就行。

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int i, j, k, ans = 0, minDiff = Integer.MAX_VALUE;
        for (i = 0; i < nums.length - 2; i++) {
            j = i + 1;
            k = nums.length - 1;
            while (j < k) {
                if (Math.abs(nums[i] + nums[j] + nums[k] - target) < minDiff) {
                    ans = nums[i] + nums[j] + nums[k];
                    minDiff = Math.abs(ans - target);
                }
                if (nums[i] + nums[j] + nums[k] < target) j++;
                else k--;
            }
        }
        return ans;
    }
}
```

### 18. 4Sum

给定无序数组，找到所有的四元组，使得其和等于target。

**Solution**

先排序，枚举前两个数，用两个指针收缩第三第四个数。

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        int i, j, k, l;
        List<List<Integer>> ans = new ArrayList<>();
        for (i = 0; i < nums.length - 3; i++) {
            for (j = i + 1; j < nums.length - 2; j++) {
                k = j + 1;
                l = nums.length - 1;
                while (k < l) {
                    if (nums[i] + nums[j] + nums[k] + nums[l] < target) k++;
                    else if (nums[i] + nums[j] + nums[k] + nums[l] > target) l--;
                    else {
                        ans.add(Arrays.asList(nums[i], nums[j], nums[k], nums[l]));
                        k++;
                        while (k < l && nums[k] == nums[k - 1]) k++;
                    }
                }
                while (j + 1 < nums.length && nums[j + 1] == nums[j]) j++;
            }
            while (i + 1 < nums.length && nums[i + 1] == nums[i]) i++;
        }
        return ans;
    }
}
```

### 167. Two Sum II - Input array is sorted

给定有序数组，求一个二元组满足和为target

**Solution 1**

类似Two Sum用哈希表

**Solution 2**

排好序，所以直接两个指针收缩就行

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int i = 0, j = nums.length - 1;
        while (i < j) {
            if (nums[i] + nums[j] == target)
                return new int[]{i + 1, j + 1};
            if (nums[i] + nums[j] < target) i++;
            else j--;
        }
        return null;
    }
}
```

### 170. Two Sum III - Data structure design

动态维护一个数组，并且判断能不能做find

### 259. 3Sum Smaller

给定无序数组，求满足和小于target的三元组的数目（不需要判重）。

**Solution**

先排序，枚举第一个数，用两个指针收缩第二第三个数。

```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int i, j, k, counter = 0;
        for (i = 0; i < nums.length - 2; i++) {
            j = i + 1;
            k = nums.length - 1;
            while (j < k) {
                if (nums[i] + nums[j] + nums[k] < target) {
                    counter += (k - j);
                    j++;
                } else k--;
            }
        }
        return counter;
    }
}
```

