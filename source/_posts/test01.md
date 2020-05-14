title: 算法题练习记录 （一）
date: 2020-03-24
categories: 算法
tags:
- http

---

力扣算法题练习记录及小结。

<!-- more -->

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

解题方案一（最慢暴力for循环）
时间复杂度是O(n*(n-1))
```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    let arr = nums;
    let arrs = new Array()
    for(let i =  0; i < arr.length - 1; i++){
        for(let j = i+1; j < arr.length; j++){
            if ( arr[i] + arr[j] === target) {
                arrs.push(i, j)
                return arrs
            }
        }
    }
};
```

解题方案二（使用Map函数）
```
var twoSum = function(nums, target) {
    let map = new Map();
    let arr = new Array();
    for(let i in nnums) {
        map.set(nums[i], i)
    }

    for(let j = 0; j < nums.length - 1; j++) {
        if(map.has(target - nums[j]) && map.get(target - nums[j] !== j) {
            arr.push(map.get(target-nums[j]), j);
            return arr;
        })
    }
}
```

解题方案三 （尾递归）
```
var twoSum = function(nums, target, i = 0, maps = {}) {
    const map = maps
    if(map[target - nums[i] ] >= 0 ) {
        return [ map[target - nums[i] ], i]
    } else {
        map[ nums[i] ] = i;
        i++;
        if(i < nums.length - 1){
            return twoSum(nums, target, i, map)
        }else {
            return [ map[target - nums[i] ], i]
        }
        
        
    }
}
```