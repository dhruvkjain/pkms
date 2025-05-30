[[DSA_CP/index|DSA_CP/index]]

# Basics

**Print your Name N times using recursion**
```rust
fn solve(i:i32, n:i32){
    if i==n{
        return;
    } else {
        println!("hello");
        solve(i+1, n);
    }
}

fn main() {
    solve(0,3);
}
```

**Print from 1 to N using Recursion**
```rust
fn solve(i:i32, n:i32){
    if i==n{
        return;
    } else {
        println!("{}", i);
        solve(i+1, n);
    }
}

fn main() {
    solve(0,3);
}
```

**Printing integers from N to 1 (using Backtracking)**
```rust
fn solve(i:i32, n:i32){
    if i==n{
        return;
    } else {
        solve(i+1, n);
        println!("{}", i);
    }
}

fn main() {
    solve(0,3);
}
```

**Recursive way of calculating the sum of first N Natural Numbers:**
- Parameterized Way
	```rust
	fn solve(i:i32, sum:i32){
	    if i==0{
	        println!("{}", sum);
	        return;
	    } else {
	        solve(i-1, sum+i);
	    }
	}
	
	fn main() {
	    solve(5,0);
	}
	```
	
- Functional Way
	```rust
	fn solve(i:i32) -> i32 {
	    if i==0{
	        return 0;
	    } else {
	        return i + solve(i-1);
	    }
	}
	
	fn main() {
	    println!("{}", solve(5));
	}
	```

**Reverse an array**
```rust
fn main() {
    let mut v = vec![1, 2, 3, 4, 5];
    v.reverse();
    println!("{:?}", v);
}
```

**Given String is Palindrome or not**
```rust
use basics::*;

fn main() {
    let input = take_string_as_vec();
    let mut ispalin = true;
    let mut ptr1= 0;
    let mut ptr2 = input.len()-1;
    let mut i = 0;
    while i<=(input.len()>>1) {
        if input[ptr1] != input[ptr2] {
            ispalin = false;
            break
        }
        ptr1+=1;
        ptr2-=1;
        i+=1;
    }
    println!("{}", ispalin);
}
```

## **HashMap** (unordered map)
Rust uses `ahash` by default

**Given an array of integers: [1, 2, 1, 3, 2] and we are given some queries: [1, 3, 4, 2, 10]. For each query, we need to find out how many times the number appears in the array.**
```rust
use std::collections::HashMap;

fn main() {
    let arr = vec![1, 2, 1, 3, 2];
    let queries = vec![1, 3, 4, 2, 10];
	
    let mut freq_map = HashMap::<i32, usize>::new();
	
    for num in arr {
        *(freq_map.entry(num).or_insert(0)) += 1;
    }
	
    for query in queries {
        let result = freq_map.get(&query);
        let mut count=0;
        match result {
            Some(&num) => count = num,
            None => count = 0
        }
        println!("{} appears {} times", query, count);
    }
	
	// Instead of matching the Option returned from get we could have used
	// unwrap but it doesn't work on &{integer} so we can use .copied() that
	// converts and `Option<&usize>` into `Option<usize>` 
	for query in queries {
		let count = freq_map.get(&query).copied().unwrap_or(0);
	    println!("{} appears {} times", query, count);
    }
}
```

**Given an array of size N. Find the highest and lowest frequency element.**
can use Moore’s Voting Algorithm too to find highest frequency element
```rust
use std::collections::HashMap;

fn main() {
    let arr = vec![1, 2, 1, 3, 2];
    let mut freq_map = HashMap::<i32, usize>::new();
    
    for num in arr {
        *freq_map.entry(num).or_insert(0) += 1;
    }
	
    let mut min_freq = usize::MAX;
    let mut max_freq = usize::MIN;
    let mut min_ele = 0;
    let mut max_ele = 0;
	
    for (&num, &count) in &freq_map {
        if count < min_freq {
            min_freq = count;
            min_ele = num;
        }
        if count > max_freq {
            max_freq = count;
            max_ele = num;
        }
    }
	
    for key_value in &freq_map {
        println!("{} appears {} times", *key_value.0, *key_value.1);
    }
	
    println!("Maximum frequency element is {} with {} frequency", max_ele, max_freq);
    println!("Minimum frequency element is {} with {} frequency", min_ele, min_freq);
}
```

# Sorting
Rust uses TimSort by default which takes O(nlogn)

## Selection Sort
```rust
use basics::*;

fn main() {
    let mut arr = take_vector_int();
    let n = arr.len();
	
    for i in 0..n {
        let mut min = i;
        for j in i+1..n {
            if arr[j] < arr[i] {
                min = j;
            }
        }
        let temp = arr[i];
        arr[i] = arr[min];
        arr[min] = temp;
    }
	
    for i in arr{
        print!("{} ", i);
    }
}
```

## Bubble sort
```rust
use basics::*;

fn main() {
    let mut arr = take_vector_int();
    let n = arr.len();
	
    for i in 0..n {
        for j in i+1..n {
            if arr[j] < arr[j-1] {
                let temp = arr[j];
                arr[j] = arr[j-1];
                arr[j-1] = temp;
            }
        }
    }
	
    for i in arr{
        print!("{} ", i);
    }
}
```

## Insertion Sort
```rust
use basics::*;

fn main() {
    let mut arr = take_vector_int();
    let n = arr.len();
	
    for i in 1..n {
        if arr[i] < arr[i-1] {
            for j in (1..=i).rev() {
                if arr[j] < arr[j-1] {
                    let temp = arr[j];
                    arr[j] = arr[j-1];
                    arr[j-1] = temp;
                } 
            }
        }
    }
	
    for i in arr{
        print!("{} ", i);
    }
}
```

## Merge Sort
```rust
pub fn merge_sort(arr:Vec<usize>) -> Vec<usize>{
    let len = arr.len();
    if len <=1 {
        return arr
    }
    let mid = len/2;
	
    let arr1 = merge_sort(arr[..mid].to_vec());
    let arr2 = merge_sort(arr[mid..].to_vec());
    merge(arr1, arr2) 
}

pub fn merge(arr1:Vec<usize>, arr2:Vec<usize>) -> Vec<usize> {
    let mut arr3 = Vec::<usize>::new();
	
    let mut i = 0;
    let mut j = 0;
    while i < arr1.len() && j < arr2.len(){
        if arr1[i] < arr2[j] {
            arr3.push(arr1[i]);
            i+=1;
        } else {
            arr3.push(arr2[j]);
            j+=1;
        }
    }
	
    arr3.extend_from_slice(&arr1[i..]);
    arr3.extend_from_slice(&arr2[j..]);
	
    arr3
}

fn main() {
    let arr = take_vector_int();
	
    let sorted = merge_sort(arr);
	
    for i in sorted{
        print!("{} ", i);
    }
}
```

# Arrays

Check for rotated sorted array
```rust
pub fn check(nums: Vec<i32>) -> bool {
	let mut drop_count = 0;
	let l = nums.len();
	
	for i in 0..l {
		if nums[i] > nums[(i + 1) % l] {
			drop_count += 1;
		}
	}
	let is_sorted_or_rotated = drop_count <= 1;
	is_sorted_or_rotated
}
```

remove duplicates
```rust
pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
	let l = nums.len();
	let mut i = 0;
	for j in 1..l{
		if nums[i] != nums[j]{
			i += 1;
			nums[i] = nums[j];
		}
	}
	(i+1) as i32
}
```

rotate array by k places:
```rust
pub fn rotate(nums: &mut Vec<i32>, k: i32) {
	let var = k as usize;
	let l = nums.len();
	let r = (var % l);
	
	let temp = nums[(l-r)..].to_vec();
	nums.truncate(l-r);
	nums.splice(0..0, temp);
}
```

move zeros to end:
```rust
let mut left = 0;
for right in 0..nums.len() {
	println!("{}, {}", left, right);
	if nums[right] != 0 {
		nums.swap(left, right);
		left += 1;
	}
	println!("{:?}", nums);
}
```

find missing number in a range 
```rust
pub fn missing_number(nums: Vec<i32>) -> i32 {
	let l = nums.len();
	let mut xor1 = 0;
	let xor2 = match l%4 {
		0 => l,
		1 => 1,
		2 => l + 1,
		_ => 0,
	} as i32;
	for i in nums {
		xor1 ^= i;
	}
	xor1^xor2
}
```

***Using  prefix sum array***, subarray with maximum sum, return sum
```rust
pub fn max_sub_array(nums: Vec<i32>) -> i32 {
	let l = nums.len();
	
	if l == 1{
		return nums[0]
	}
	
	let mut sums:Vec<i32> = vec![0; l];
	sums[0] = nums[0];
	for i in 1..l {
		sums[i] = sums[i-1]+nums[i];
	}
	
	let mut max_ele = sums[0];
	let mut min_ele = 0;
	for i in sums {
		max_ele = max_ele.max(i-min_ele);
		min_ele = min_ele.min(i);
	}
	return max_ele
}
```

***Kadane's Algorithm*** for subarray with maximum sum, return sum
```rust
pub fn max_sub_array(nums: Vec<i32>) -> i32 {
	let l = nums.len();
	
	let mut max = nums[0];
	let mut sum = nums[0];
	for i in 1..l{
		sum = sum.max(0) + nums[i];
		max = max.max(sum);
	}
	return max
}
```

***Two Sum***, sum of two elements in an array is equal to target
```rust
use std::collections::HashMap;

impl Solution {
    pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
        let l = nums.len();
        let mut pos = Vec::new();
        let mut map = HashMap::new();
        
        for i in 0..l {
            map.insert(nums[i], i);
        }

        for i in 0..l {
            match map.get(&(target-nums[i])) {
                Some(value) => {
                    if *value!= i {
                        pos.push(i as i32);
                        pos.push(*value as i32);
                        break
                    }
                },
                None => {}
            }
        }
        pos
    }
}
```

***Dutch National flag algorithm***, Sort an array of 0's 1's and 2's
```rust
pub fn sort_colors(nums: &mut Vec<i32>) {
	let l = nums.len();

	let mut low = 0;
	let mut mid = 0;
	let mut high = l-1;

	while mid<=high {
		if nums[mid] == 0{
			nums.swap(low, mid);
			mid += 1;
			low += 1;
		} else if nums[mid] == 1 {
			mid += 1;
		} else {
			nums.swap(mid, high);
			if high !=0 {
				high -= 1;
			} else {
				break
			}
		}
	} 
}
```

***Moore’s Voting Algorithm***, majority element
```rust
pub fn majority_element(nums: Vec<i32>) -> i32 {
	let l = nums.len();
	
	let mut ele = nums[0];
	let mut cnt = 1;
	for i in 1..l {
		if cnt == 0 {
			ele = nums[i];
			cnt += 1;
		} else {
			if nums[i] == ele {
				cnt += 1;
			} else {
				cnt -= 1;
			}
		}
	}
	ele
}
```

***Next Permutation***
```rust
let mut nums = take_vector_int();
let l = nums.len();

// Step 1: Find the break point:
let mut ind = l + 1; // break point
for i in (0..l - 2).rev() {
	if nums[i] < nums[i + 1] {
		println!("{}, {}", nums[i], nums[i + 1]);
		ind = i;
		break;
	}
}

// If break point does not exist means we are at highest permutation:
if ind == l+1 {
	nums.reverse();
} else {
	println!("{}, {}", ind, nums[ind]);

	// Step 2: Find the next greater element
	//         and swap it with arr[ind]:
	for i in (0..l - 1).rev() {
		if nums[i] > nums[ind] {
			nums.swap(ind, i);
			println!("{:?}", nums);
			break;
		}
	}

	// Step 3: reverse the right half:
	nums[ind + 1..].reverse();
}
println!("{:?}", nums);
```

**Longest Consecutive Subsequence** 
```rust
use basics::*;
use std::collections::HashSet;

fn main() {
    let mut nums = take_vector_int();
    let l = nums.len();

    let mut set = HashSet::new();
    for i in 0..l {
        set.insert(nums[i]);
    }

    println!("{:?}", set);
    let mut longest = 0;
    let mut cnt = 1;
    for &i in &set {
        if !set.contains(&(i-1)){
            let mut j = i+1;
            loop {
                if set.contains(&j) {
                    cnt += 1;
                    j += 1; 
                } else {
                    longest = longest.max(cnt);
                    cnt = 1;
                    break
                }
            }
        }
    }
    longest = longest.max(cnt);
    println!("{}", longest);
}
```

***Rotate a square matrix***
90 = transpose + reverse row
180 = reverse row + reverse col
270 = transpose + reverse col
for 90 :
```rust
let mut matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]];

let l = matrix.len();
// Transpose of a matrix
for i in 0..l {
	for j in i+1..l {
		println!("{}, {}", i, j);
		let temp = matrix[i][j];
		matrix[i][j] = matrix[j][i];
		matrix[j][i] = temp;
	}
}
println!("{:?}", matrix);

// Reverse the rows
for i in 0..l/2 {
	let k = l-1-i;
	println!("{}", k);
	for j in 0..l {
		println!("{}, {} swap with {}, {}", j, i, j, k);
		let temp = matrix[j][i];
		matrix[j][i] = matrix[j][k];
		matrix[j][k] = temp;
	}
}

println!("{:?}", matrix);
```

***Spiral matrix***
```rust
 pub fn spiral_order(matrix: Vec<Vec<i32>>) -> Vec<i32> {
	let m = matrix.len();
	let n = matrix[0].len();

	let mut ans = Vec::new();
	let mut top = 0;
	let mut right = n-1;
	let mut bottom = m-1;
	let mut left = 0;

	while top <= bottom && left <= right {

		for i in left..=right {
			ans.push(matrix[top][i]);
		}
		top += 1;

		for i in top..=bottom {
			ans.push(matrix[i][right]);
		}
		if right == 0 { break; }
		right -= 1;

		if top <= bottom {
			for i in (left..=right).rev() {
				ans.push(matrix[bottom][i]);
			}
			if bottom == 0 { break; }
			bottom -= 1;
		}

		if left <= right {
			for i in (top..=bottom).rev() {
				ans.push(matrix[i][left]);
			}
			left += 1;
		}
	}

	ans
}
```

***Pascal's Triangle***
```rust
pub fn generate(num_rows: i32) -> Vec<Vec<i32>> {
	let mut ans = Vec::<Vec<i32>>::new();
	for row in 1..=num_rows as usize {
		ans.push(vec![1]);
		let mut ele = 1;
		
		for col in 1..row {
			ele = ele*(row-col);
			ele = ele/col;
			ans[row-1].push(ele as i32);
		}
	}
	ans
}
```

***Extended Boyer Moore’s Voting Algorithm***
```rust
use std::i32::MIN;

impl Solution {
    pub fn majority_element(nums: Vec<i32>) -> Vec<i32> {
        let l = nums.len();
        let mut ans = Vec::new();
		
        let mut cnt1 = 0;
        let mut cnt2 = 0;
        let mut ele1 = MIN;
        let mut ele2 = MIN;
		
        for i in 0..l {
            if cnt1==0 && ele2!=nums[i] {
                ele1 = nums[i];
                cnt1 = 1;
            } else if cnt2==0 && ele1!=nums[i] {
                ele2 = nums[i];
                cnt2 = 1;
            } else if ele1 == nums[i] {
                cnt1+=1;
            } else if ele2 == nums[i] {
                cnt2+=1;
            } else {
                cnt1-=1;
                cnt2-=1;
            }
        }
		
        cnt1 = 0;
        cnt2 = 0;
		
        for i in 0..l{
            if nums[i] == ele1 {
                cnt1+=1;
            }
            if nums[i] == ele2 {
                cnt2+=1;
            }
        }
		
        if cnt1 > l/3 {
            ans.push(ele1);
        }
        if cnt2 > l/3 {
            ans.push(ele2);
        }
		
        ans
    }
}
```

***3 Sum***, using 3 pointers
```rust
let mut nums = take_vector_int();
let l = nums.len();

let mut ans = Vec::<Vec<i32>>::new(); 
nums.sort();
let mut i = 0;
while i<l-2 {
	let mut j = i+1;
	let mut k = l-1;
	if i>0 && nums[i] == nums[i-1] {
		i += 1;
	} else {
		while j<k {
			let sum = nums[i]+nums[j]+nums[k];
			if sum < 0 {
				j+=1;
			} else if sum > 0 {
				k-=1;
			} else {
				ans.push(vec![nums[i], nums[j], nums[k]]);
				j+=1;
				k-=1;
				while j<k && nums[j] == nums[j-1] {
					j+=1;
				}
				while j<k && nums[k] == nums[k+1] {
					k-=1;
				}
			}
		}
		i+=1;
	}
}

println!("{:?}", ans);
```

***4 Sum***, using 4 pointer (similar to 3 Sum)
```rust
pub fn four_sum(mut nums: Vec<i32>, target: i32) -> Vec<Vec<i32>> {
	let l = nums.len();
	let mut ans = Vec::<Vec<i32>>::new(); 
	
	if l<4 {
		return ans;
	} 
	if l == 4 {
		let mut sum:i64 = 0;
		for i in 0..4 {
			sum += nums[i] as i64;
		}
		if sum == target as i64 {
			ans.push(vec![nums[0], nums[1], nums[2], nums[3]]);
		}
		return ans;
	}
	
	nums.sort();
	let mut i = 0;
	while i<l-3 {
		let mut j = i+1;
		if i>0 && nums[i] == nums[i-1]{
			i+=1;
		} else {
			while j < l-2 {
				let mut k = j+1;
				let mut m = l-1;
				if j>i+1 && nums[j] == nums[j-1] {
					j += 1;
				} else {
					while k<m {
						let sum:i64 = (nums[i]+nums[j]+nums[k]+nums[m]) as i64;
						if sum < target as i64 {
							k+=1;
						} else if sum > target as i64 {
							m-=1;
						} else {
							ans.push(vec![nums[i], nums[j], nums[k], nums[m]]);
							k+=1;
							m-=1;
							while k<m && nums[k] == nums[k-1] {
								k+=1;
							}
							while k<m && nums[m] == nums[m+1] {
								m-=1;
							}
						}
					}
					j+=1;
				}
			}
			i+=1;
		}
	}

	ans
}
```

***Length of the longest subarray with zero Sum***
```rust
use basics::*;
use std::collections::HashMap;

fn main() {
    let mut nums = take_vector_int();
    let l = nums.len();

    let mut max = 0;
    let mut sum = 0;
    let mut map = HashMap::<i32, usize>::new();
    
    for i in 0..l{
        sum += nums[i];
        if sum == 0 {
            max = i+1;
        } else {
            if let Some(&idx) = map.get(&sum) {
                max = max.max(i-idx);
            } else {
                map.insert(sum, i);
            }
        }
    }

    println!("{}", max);
}
```

Count the number of subarrays with given xor K
```rust
let mut arr = take_vector_int();
let mut k = take_int();

let l = arr.len();

let mut map = HashMap::<i32, usize>::new();
let mut cnt = 0;
let mut x = 0;

for i in 0..l {
	x ^= arr[i];
	if x == k {
		cnt+=1;
	} else {
		if let Some(&c) = map.get(&(x^k)) {
			cnt+=c;
		} else {
			*map.entry(x).or_insert(0) += 1;
		}
	}
}

println!("{}", cnt);
```

XOR Queries of a Subarray
```rust
pub fn xor_queries(arr: Vec<i32>, queries: Vec<Vec<i32>>) -> Vec<i32> {
	let l = arr.len();

	let mut x = Vec::<i32>::new();
	x.push(arr[0]);

	for i in 1..l {
		x.push(x[i-1]^arr[i]);
	}

	let mut ans = Vec::new();

	for q in queries {
		if q[0] == 0 {
			ans.push(x[q[1] as usize]);
		} else {
			ans.push( x[q[1] as usize]^x[(q[0]-1) as usize] )
		}
	}
	
	ans
}
```

