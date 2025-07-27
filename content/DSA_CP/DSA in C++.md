[[DSA_CP/index|DSA_CP/index]]

# Arrays

Merge Intervals
```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
	vector<vector<int>> res;
	sort(intervals.begin(), intervals.end());
	
	int start = intervals[0][0];
	int end = intervals[0][1];
	for (int i = 1; i<intervals.size(); i++) {
		int s = intervals[i][0], e = intervals[i][1];
		if (s <= end) {
			end = max(end, e);	
		} 
		if (s > end) {
			res.push_back({start, end});
			start = s;
			end = e;
		}
	}
	res.push_back({start, end});
	
	return res;
}
```

### Merge two Sorted Arrays Without Extra Space
```cpp
void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
	int i = m+n-1;
	int p2 = m-1;
	int p3 = n-1;
	while (p2 >= 0 && p3 >= 0) {
		if (nums1[p2] > nums2[p3]) {
			nums1[i] = nums1[p2];
			p2--;
		} else {
			nums1[i] = nums2[p3];
			p3--;
		}
		i--;
	}

	while (p3 >= 0) {
		nums1[i] = nums2[p3];
		p3--;
		i--;
	}
}
```

### Find All Duplicates in an Array (Negative marking)
because we are given that elements of array are in the range `[0 .. n]` we can uses elements as indexes
for every element in array mark it's absolute index -1 representing that element has been taken
```cpp
vector<int> findDuplicates(vector<int>& nums) {
	if(nums.size()==1) return {};
	// Negative marking
	// Whenever we meet the element we mark the value - 1 as negative
	vector<int> ans;
	for(int i = 0; i<nums.size(); i++) {
		if(nums[abs(nums[i])-1]>0)
			nums[abs(nums[i])-1] *=-1; // Mark it with negative
		else
			ans.push_back(abs(nums[i]));
	}
	return ans;
}
```


### Count Inversion

### Reverse Pairs
```cpp

```

# Binary Search
Think how you can eliminate any half

```cpp
void merge(vector<int>& arr, int low, int mid, int high) {
	vector<int> temp;
	int a = low;
	int b = mid+1;

	while (a <= mid && b <= high) {
		if (arr[a] < arr[b]) {
			temp.push_back(arr[a]);
			a++;
		} else {
			temp.push_back(arr[b]);
			b++;
		}
	}

	while (a <= mid) {
		temp.push_back(arr[a]);
		a++;
	}
    while (b <= high) {
		temp.push_back(arr[b]);
		b++;
	}

	for (int i = 0; i < temp.size(); ++i) {
        arr[low + i] = temp[i];
    }
}

void merge_sort(vector<int>& arr, int low, int high) {
	if (low>=high) {
		return;
	}

	int mid = (low+high)/2;
	merge_sort(arr, low, mid);
	merge_sort(arr, mid+1, high);
	merge(arr, low, mid, high);
}

int binary_search(vector<int>& arr, int x) {
	int pos = -1;
	int low = 0;
	int high = arr.size()-1;
	while (low<=high) {
		int mid = (low+high)/2;
		if (x<arr[mid]) {
			high = mid-1;
		} else if (x>arr[mid]) {
			low = mid+1;
		} else {
			pos = mid;
			break;
		}
	}
	return pos;
}

int main()
{
	vector<int> nums = take_vec_int();	
	int x;
	cin >> x;
	merge_sort(nums, 0, nums.size()-1);
	for (auto i: nums) {
		cout << i << " ";
	}
	cout << endl;
	cout << binary_search(nums, x) << endl;

	return 0;
}
```

### Upper Lower Bounds
```cpp
int lower_bound(vector<int>& arr, int x) {
	int pos = arr.size();
	int low = 0;
	int high = arr.size()-1;
	while (low<=high) {
		int mid = (low+high)/2;
		if (arr[mid]>=x) {
			pos = mid;
			high = mid-1;
		} else {
			low = mid+1;
		}
	}
	return pos;
}

int upper_bound(vector<int>& arr, int x) {
	int pos = arr.size();
	int low = 0;
	int high = arr.size()-1;
	while (low<=high) {
		int mid = (low+high)/2;
		if (arr[mid] > x) {
			pos = mid;
			high = mid - 1;
		} else {
			low = mid + 1;
		}
	}
	return pos;
}
```

Search in Rotated Sorted Array
```cpp
int search(vector<int>& nums, int target) {
	int l = nums.size()-1;
	int low = 0;
	int high = l;

	while (low<=high) {
		int mid = (low+high)/2;
		if (nums[mid] == target) {
			return mid;
		} 
		if (nums[low]<=nums[mid]) {
			if (target<=nums[mid] && target>=nums[low]) {
				high = mid-1;
			} else {
				low = mid+1;
			}
		} else {
			if (target>=nums[mid] && target<=nums[high]) {
				low = mid+1;
			} else {
				high = mid-1;
			}
		}
		
	}
	return -1;
}
```

Find Minimum element in Rotated Sorted Array (position of minimum = number of times it's rotated)
```cpp
int findMin(vector<int>& nums) {
	int pos = -1;
	int low = 0;
	int high = nums.size()-1;
	int min = INT_MAX;
	
	while (low<=high) {
		int mid = (low+high)/2;
		if (nums[low] <= nums[mid]) {
			if (nums[low] < min) {
				pos = low;
				min = nums[low];
			}
			low = mid+1;
		} else {
			if (nums[mid] < min) {
				pos = mid;
				min = nums[mid];
			}
			high = mid-1;
		}
	}
	
	return min;
}
```

Sorted Array with all elements twice except one element 
```cpp
int singleNonDuplicate(vector<int>& arr) {
	int n = arr.size();
	
	//Edge cases:
	if (n == 1) return arr[0];
	if (arr[0] != arr[1]) return arr[0];
	if (arr[n - 1] != arr[n - 2]) return arr[n - 1];
	
	int low = 1, high = n - 2;
	
	while (low<=high) {
		int mid = (low+high)/2;
	
		if (arr[mid] != arr[mid + 1] && arr[mid] != arr[mid - 1]) {
			return arr[mid];
		}
	
		if ((mid % 2 == 1 && arr[mid] == arr[mid - 1])
			|| (mid % 2 == 0 && arr[mid] == arr[mid + 1])) {
			low = mid + 1;
		}
		else {
			high = mid - 1;
		}
	}
	
	return -1;
}
```

Find any peak element in an array
```cpp
int findPeakElement(vector<int>& nums) {
	int n = nums.size();
	if (n == 1) return 0;
	if (n == 2) {
		if (nums[0]>nums[1]) {
			return 0;
		} else {
			return 1;
		}
	};

	int low = 0;
	int high = n-1;

	while (low<=high) {
		int mid = (low+high)/2;
		if (mid == 0) {
			if (nums[0]>nums[1]) {
				return 0;
			} else {
				return 1;
			}
		}
		if (mid == n-1) {
			if (nums[n-1]>nums[n-2]) {
				return n-1;
			} else {
				return n-2;
			}
		}
		if (nums[mid]>nums[mid-1] && nums[mid]>nums[mid+1]) {
			return mid;
		} 
		if (nums[mid]<nums[mid-1]) {
			high = mid-1;
		} else {
			low = mid+1;
		}
		// cout << low << " " << mid << " " << high << endl;
	}
	return -1;
}
```

## Binary Search on Answers find min or  max
when you can find a range in which answer lies and can eliminate some segments based on condition and can find min or max using the low high pointers.
**condition is always like after some elements all further elements are either possible or not possible.**

### Koko Eating Bananas
```cpp
int timeReq(vector<int>& piles, int k){
	int time = 0;
	for (auto i: piles) {
		time += ceil((double)i/(double)k);
	}

	return time;
}

int minEatingSpeed(vector<int>& piles, int h) {
	int n = piles.size();
	if (n==1) return ceil((double)piles[0]/(double)h);
	int max = *max_element(piles.begin(), piles.end());
	int low = 0;
	int high = max;
	int ans = -1;
	while (low<=high) {
		int mid = (low+high)/2;
		int time_req = timeReq(piles, mid);
		if (time_req <= h) {
			high = mid-1;
			ans =  mid;
		} else {
			low = mid+1;
		}
	}
	return ans;
}
```

Minimum days to make M bouquets:
```cpp
int adjacentKcount(vector<int> bloomDay, int k) {
	int cnt = 0;
	int curr_cnt = 0;
	int i = 0;
	while (i<=(bloomDay.size()-1)) {
		if (bloomDay[i]==0) {
			curr_cnt = 0;
			while (i<=(bloomDay.size()-1) && bloomDay[i] == 0) {
				curr_cnt += 1;
				i++;
				if (curr_cnt == k) break;
			}
			if (curr_cnt==k) cnt+=1;
			cout << i << " "<< curr_cnt << " " << cnt << endl;
		} else {
			i++;
			cout << i << endl;
		}
	}
	
	return cnt;
}

int minDays(vector<int>& bloomDay, int m, int k) {
	if (bloomDay.size() < m*k) return -1;
	
	int low = *min_element(bloomDay.begin(), bloomDay.end());
	int high = *max_element(bloomDay.begin(), bloomDay.end());
	
	cout << low << " " << high << endl;
	
	int min = -1;
	
	while (low<=high) {
		int mid = (low+high)/2;
		vector<int> temp;
		for (auto i: bloomDay){
			temp.push_back(max(i-mid, 0 ));
		}
		for (auto i: temp){
			cout << i << " ";
		}
		cout << endl;
		if (adjacentKcount(temp, k) >= m) {
			min = mid;
			cout << min << endl;
			high = mid-1;
		} else {
			low = mid+1;
		}
	}
	
	
	return min;
}
```

Find the Smallest Divisor Given a Threshold
```cpp
int smallestDivisor(vector<int>& nums, int threshold) {
	int low = 1;
	int high = *max_element(nums.begin(), nums.end());
	int ans = INT_MAX;

	while(low<=high) {
		int mid = (low+high)/2;
		int sum = 0;
		for (auto i: nums) {
			sum += ceil((double)i/mid);
			cout << ceil((double)i/mid) << " ";
		}
		cout << endl;
		if (sum<=threshold) {
			ans = min(mid, ans);
			high = mid-1;
		} else {
			low = mid+1;
		}
		cout << sum << " " << ans << " " << low << "-" << mid << "-" << high << endl;
	}
	
	return ans;
}
```

Capacity To Ship Packages Within D Days
```cpp
int daysReq (vector<int>& weights, int c) {
	int days = 0;
	int curr_days_sum = 0;
	// cout << "-------------" << endl;
	for (auto i: weights) {
		if ((curr_days_sum + i) > c) {
			curr_days_sum = i;
			days++;
		} else {
			curr_days_sum += i;
		}
		// cout << i << " " << days << " " << curr_days_sum << endl;
	}
	// cout << "-------------" << endl;
	return days+1;
}

int shipWithinDays(vector<int>& weights, int days) {
	int low = (*max_element(weights.begin(), weights.end()));
	int high = accumulate(weights.begin(), weights.end(), 0);
	
	int ans = -1;
	while (low<=high) {
		int mid = (low+high)/2;
		// cout << low << " " << mid << " " << high << endl;
		int days_req = daysReq(weights, mid); 
		if (days_req <= days) {
			ans = mid;
			high = mid-1;
		} else {
			low = mid+1;
		}
		// cout << ans << " " << days_req << endl; 
	}
	
	return ans;
}
```

Kth Missing Positive Number
```cpp
int findKthPositive(vector<int>& arr, int k) {
	if (k<arr[0]) {
		return k;
	}
	int n = arr.size()-1;
	if (k>arr[n]) {
		return k+(n+1);
	}
	int low = 0;
	int high = n;
	
	while (low<=high) {
		int mid = (low+high)/2;
		int missing = arr[mid]-(mid+1); 
		if (missing<k) {
			low = mid+1;
		} else {
			high = mid-1;
		}
		cout << low << " " << mid << " " << high << endl;
	}
	
	return k+high+1;
}
```

### Kth Smallest Element in a Sorted Matrix
```cpp
int countLessThan(vector<vector<int>>& matrix, int mid, int n) {
	int cnt = 0;
	for (int i=0; i<n; i++) {
		for (int j=0; j<n; j++) {
			if (matrix[i][j] <= mid) cnt++;
			else break;
		}
	}
	
	return cnt;
}

int kthSmallest(vector<vector<int>>& matrix, int k) {
	int n = matrix[0].size();
	int low = matrix[0][0], high = matrix[n-1][n-1];
	
	while (low<high) {
		int mid = low + (high-low)/2;
		int cnt = countLessThan(matrix, mid, n);
		
		if (cnt < k) {
			low = mid+1;
		} else {
			high = mid;
		}
	}
	
	return low;
}
```


## Binary Search on Answers find min(max) or max(min)
max(min) type => return high
min(max) type => return low
this is because low and high exchange at the lowest/highest possible answer.
so it's like :
```
0, 1, 2, 3, .......... n-1, n, n+1, ....... m
					    ^   ^     ^
					   low high
```
let `n` be highest possible so for max(min) we will return max i.e highest i.e `n`.
if `n` is highest possible the `n-1` is lowest for not possible answer i.e min(max).

### Aggressive Cows
max(min) type
```cpp
bool canWePlaceCows (vector<int>& stalls, int dist, int cows) {
	int n = stalls.size()-1;
	int cntCows = 1;
	int last = stalls[0];
	
	for (int i=0; i<=n; i++) {
		if (stalls[i]-last >= dist) {
			cntCows++;
			last = stalls[i];
		}
		if (cntCows >= cows) return true;
	}
	
	return false;
}

int aggressiveCows(vector<int> &stalls, int k) {
	sort(stalls.begin(), stalls.end());
	
	int n = stalls.size() - 1;
	int low = 1;
	int high = stalls[n] - stalls[0];
	
	while (low<=high) {
		int mid = (low+high)/2;
		if (canWePlaceCows(stalls, mid, k)) {
			low = mid+1;
		} else {
			high = mid-1;
		}
	}
	return high;
}
```

### Allocate Minimum Number of Pages
min(max) type
```cpp
bool canAllotPage(vector<int>& pageNumbers, int m, int capacity) {
	int n = pageNumbers.size()-1;
	int alloted = 0;
	int currSum = 0;
	for (int i=0; i<=n; i++) {
		if (currSum+pageNumbers[i] > capacity) {
			alloted++;
			currSum = pageNumbers[i];
		} else {
			currSum += pageNumbers[i];
		}
	}
	cout << capacity << " " << currSum << " " << alloted << endl;
	if (alloted >= m) {
		return true;
	} else {
		return false;
	}
}

int allocatePages(vector<int>& pageNumbers, int m) {
	int n = pageNumbers.size() - 1;
	if (m > (n+1)) return -1;
	int low = *max_element(pageNumbers.begin(), pageNumbers.end());
	int high = accumulate(pageNumbers.begin(), pageNumbers.end(), 0);

	while (low<=high) {
		int mid = (low+high)/2;
		cout << low << "-" << mid << "-" << high << endl;
		if (canAllotPage(pageNumbers, m, mid)) {
			low = mid+1;
		} else {
			high = mid-1;
		}
	}
	return low;
}
```

### Split Array - Largest Sum
min(max) type
```cpp
int countPartitions (vector<int>& nums, int capacity) {
	int n = nums.size()-1;
	int currSum = 0;
	int divisions = 1;
	for (int i=0; i<=n; i++) {
		if (currSum + nums[i] > capacity) {
			divisions++;
			currSum = nums[i];
		} else {
			currSum += nums[i];
		}
	}
	// cout << capacity << " " << currSum << " " <<  divisions << endl;
	return divisions;
}

int splitArray(vector<int>& nums, int k) {
	int n = nums.size()-1;
	int low = *max_element(nums.begin(), nums.end());
	int high = accumulate(nums.begin(), nums.end(), 0);

	while (low<=high) {
		int mid = (low+high)/2;
		// cout << low << "-" << mid << "-" << high << endl;
		int possibleDivisions = countPartitions(nums, mid);
		if (possibleDivisions == k) {
			high = mid-1;
		} else if (possibleDivisions > k) {
			low = mid+1;
		} else {
			high = mid-1;
		}
	}

	return low;
}
```

Painter's Partition Problem
min(max) type
```cpp
int partitionsReq(vector<int>& boards, int time) {
	int n = boards.size()-1;
	int partitions = 1;
	int timeSum = 0;
	
	for (int i=0; i<=n; i++) {
		if (timeSum + boards[i] > time) {
			timeSum = boards[i];
			partitions++;
		} else {
			timeSum += boards[i];
		}
	}
	// cout << time << " " << timeSum << " " << partitions << endl; 
	
	return partitions;
}

int paintersProblem (vector<int>& boards, int k) {
	int n = boards.size()-1;
	int low = *max_element(boards.begin(), boards.end());
	int high = accumulate(boards.begin(), boards.end(), 0);
	
	while (low<=high) {
		int mid = (low+high)/2;
		// cout << low << "-" << mid << "-" << high << endl;
		int partitions = partitionsReq(boards, mid);
		if (partitions > k) {
			low = mid+1;
		} else {
			high = mid-1;
		}
	}
	
	return low;
}
```

### Minimize Maximum Distance between Gas Stations
min(max) type, but we have to place them so the points are also variable not like Aggressive Cows questions where we had to place cows on given stalls, therefore for this problem we have to find optimal way to reduce maximum distance which is:
place new stations between largest gaps and distribute equal length of distance/gap between them.

but we can't apply BS directly too as low mid high are int's but we have long double positions
so what we will do is for `low <= high` we will check for the first 10^-6 values as it's specified in question that as limit.
and `low = mid+1` to `low = mid`
and `high = mid-1` to `high = mid`




# Strings

**ASCII values:**
- lowercase => 97 - 122
- uppercase => 65 - 90
- digits => 48 - 57

Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.
```cpp
bool isIsomorphic(string s, string t) {
	unordered_map<char, char> chars;
	unordered_map<char, char> chars2;
	for (int i=0; i<s.length(); i++) {
		if (chars.find(s[i]) != chars.end()) {
			if (chars[s[i]] != t[i] ) {
				return false;
			}
		} 
		if (chars2.find(t[i]) != chars2.end()) {
			if (chars2[t[i]] != s[i]) {
				return false;
			}
		} 
	
		chars[s[i]] = t[i]; 
		chars2[t[i]] = s[i];
	}
	
	return true;
}
```

### Sort characters by frequency
```cpp
string frequencySort(string s) {
  unordered_map<char, int> freq;

  for (char c: s) {
    freq[c]++;
  }

  vector<pair<char, int>> freqVec;
  for (auto [key, val]: freq) {
    freqVec.push_back({key, val});
  }

  sort(freqVec.begin(), freqVec.end(), [](auto& a, auto& b) {
    return a.second > b.second;
  });

  string res;
  for (auto p: freqVec) {
    while (p.second--) {
      res += p.first;
    }
  }

  return res;
}
```

### Count Number of Homogenous Substrings
A string is **homogenous** if all the characters of the string are the same.
```
Input: s = "abbcccaa"
Output: 13
Explanation: The homogenous substrings are listed as below:
"a"   appears 3 times.
"aa"  appears 1 time.
"b"   appears 2 times.
"bb"  appears 1 time.
"c"   appears 3 times.
"cc"  appears 2 times.
"ccc" appears 1 time.
3 + 1 + 2 + 1 + 3 + 2 + 1 = 13.
```
number of subarrays in a `'n'` length = `n(n+1) / 2`
therefore count for a single homogeneous substring = `n(n+1) / 2`

```cpp
int countHomogenous(string s) {
	s +=' ';
	const int modu = 1e9+7;
	long long count = 0;	
	long long curr_count = 1;
	char curr_char = s[0];

	for (int i=1; i<s.size(); i++) {
		cout << s[i] << endl;
		cout << curr_char << " " << curr_count << endl;
		if (s[i] == curr_char) {
			curr_count++;
		} else {
			count += (curr_count*(curr_count+1))/2;
			curr_char = s[i];
			curr_count = 1;
		}
	}
	
	return count%modu;
}
```

### Longest Palindromic Substring
```cpp
int expandAroundCenter(string s, int left, int right) {
	while (left >= 0 && right < s.length() && s[left] == s[right]) {
		left--;
		right++;
	}
	return right - left - 1;
} 
string longestPalindrome(string s) {
	if (s.empty()) {
		return "";
	}
	
	int start = 0;
	int end = 0;
	
	for (int i = 0; i < s.length(); i++) {
		int odd = expandAroundCenter(s, i, i);
		int even = expandAroundCenter(s, i, i + 1);
		int max_len = max(odd, even);
		
		if (max_len > end - start) {
			start = i - (max_len - 1) / 2;
			end = i + max_len / 2;
		}
	}
	
	return s.substr(start, end - start + 1);        
}
```

### Sum of Beauty of All Substrings
```cpp
int beautySum(string s) {
	int n = s.size();
	int totalSum=0;
	
	for (int i=0; i<n; i++) {
		unordered_map<char, int> mp;
		for (int j=i; j<n; j++) {
			mp[s[j]]++;
			int maxi = INT_MIN;
			int mini = INT_MAX;
			
			for (auto it: mp) {
				maxi = max(it.second, maxi);
				mini = min(it.second, mini);
			}
			
			totalSum += (maxi-mini);
		}
	}
	
	return totalSum;
}
```

## String Hashing
https://cp-algorithms.com/string/string-hashing.html


# Linked List
the fast slow pointers used here are very useful in array and in general traversal of any data structure.

### Middle element in a Linked List
two pointer fast and slow such that fast moves 2 * slows so in O(N/2) we can find middle element. 
```cpp
ListNode* middleNode(ListNode* head) {
	ListNode* slow = head;
	ListNode* fast = head;
	
	while (fast!=nullptr && fast->next!=nullptr) {
		slow = slow->next;
		fast = fast->next->next;
	}
	
	return slow;
}
```

### Reverse a linked list (iterative)
```cpp
ListNode* reverseList(ListNode* head) {
	ListNode* prev = nullptr;
	ListNode* next = head;
	ListNode* curr = head;
	
	while (next!=nullptr) {
		next = curr->next;
		curr->next = prev;
		prev = curr;
		curr = next;
	}
	
	return prev;
}
```

Reverse a linked list (recursive)
```cpp
ListNode* reverseList(ListNode* head) {
	if (head == nullptr || head->next == nullptr) {
		return head;
	}
	
	ListNode* new_head = reverseList(head->next);
	ListNode* next = head->next;
	next->next = head;
	head->next = nullptr;
	
	return new_head;
}
```

### Detect Loop in Linked List using 2 pointer (fast and slow)
inside loop, slow move by 1 and fast by 2 so each iteration causes distance between them to reduce by 1
```cpp
bool hasCycle(ListNode *head) {
	ListNode* slow = head;
	ListNode* fast = head;
	while (fast!=NULL && fast->next!=NULL) {
		slow = slow->next;
		fast = fast->next->next;
		if (slow==fast) return true;
	}
	return false;
}
```

### Find Starting point of Loop in Linked List (fast and slow)
If distance from starting of Linked List to starting of Loop is `d`, then
the collision point of fast and slow is always mid point of loop,
which is `d` so:
head <--- d ---> loop starting <--- d ---> collision point <---d ---> loop starting   
```cpp
ListNode *detectCycle(ListNode *head) {
	ListNode* slow = head;
	ListNode* fast = head;
	bool found = false;
	
	while (fast!=NULL && fast->next!=NULL) {
		slow = slow->next;
		fast = fast->next->next;
		if (slow==fast) {
			found = true;
			break;
		}
	}
	if (!found) {
		return NULL;
	}
	
	slow = head;
	while (slow!=fast) {
		slow = slow->next;
		fast = fast->next;
	}
	
	return fast;
}
```

Palindrome:
```cpp
ListNode* reverseList(ListNode* head) {
	if (head == nullptr || head->next == nullptr) {
		return head;
	}
	
	ListNode* new_head = reverseList(head->next);
	ListNode* next = head->next;
	next->next = head;
	head->next = nullptr;
	
	return new_head;
}

bool isPalindrome(ListNode* head) {
	ListNode* slow = head;
	ListNode* fast = head;
	
	while (fast->next!=nullptr&&fast->next->next!=nullptr) {
		slow = slow->next;
		fast = fast->next->next;
	}
	ListNode* newHead = reverseList(slow->next);
	ListNode* end = newHead;
	ListNode* start = head;
	while (end!=nullptr) {
		if (start->val != end->val) {
			return false;
		}
		start = start->next;
		end = end->next;
	}
	reverseList(slow->next);
	return true;
}
```

All odd nodes before and then even nodes:
```cpp
ListNode* oddEvenList(ListNode* head) {
	if (head==nullptr || head->next==nullptr) return head;
	ListNode* evenHead = head->next;
	ListNode* odd = head;
	ListNode* even = head->next;

	while (even!=nullptr && even->next!=nullptr) {
		odd->next = odd->next->next;
		even->next = even->next->next;
		odd = odd->next;
		even = even->next;
	}
	odd->next = evenHead;

	return head;
}
```

### Remove Nth node from the end (i.e. (length-N)th node)
To reach nth node from end, 
we first move fast pointer `n` nodes so now it has `length-n` nodes left 
so now if we start moving slow pointer from start till fast pointer cover `length-n` nodes
slow pointer cover `length-n` nodes too therefore reaching nth node from the end.  
```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
	if (head->next == nullptr) {
		delete head;
		return nullptr;
	}
	ListNode* fast = head;
	for (int i=0; i<n; i++){
		if (fast->next != nullptr) {
			fast = fast->next;
		} else {
			ListNode* temp = head->next;
			delete head;
			return temp;
		}
	}
	
	ListNode* slow = head;
	while (fast->next!=nullptr) {
		slow = slow->next;
		fast = fast->next;
	}
	ListNode* delNode = slow->next;
	slow->next = slow->next->next;
	delete delNode;
	
	return head;
}
```

Merge Sort for Linked List:
```cpp
ListNode* findMiddle(ListNode* head) {
	ListNode* prev = nullptr;
	ListNode* slow = head;
	ListNode* fast = head;
	
	while (fast!=nullptr && fast->next!=nullptr) {
		prev = slow;
		slow = slow->next;
		fast = fast->next->next;
	}
	
	return prev;
}

ListNode* mergeTwoSortedLinkedLists(ListNode* list1, ListNode* list2) {
	ListNode* dummyNode = new ListNode(-1);
	ListNode* temp = dummyNode;
	
	while (list1 != nullptr && list2 != nullptr) {
		if (list1->val <= list2->val) {
			temp->next = list1;
			list1 = list1->next;
		} else {
			temp->next = list2;
			list2 = list2->next;
		}
		temp = temp->next; 
	}
	
	if (list1 != nullptr) {
		temp->next = list1;
	} else {
		temp->next = list2;
	}
	
	return dummyNode->next;
}

ListNode* sortList(ListNode* head) {
	if (head == nullptr || head->next == nullptr) {
		return head;
	}
	
	ListNode* middle = findMiddle(head);
	ListNode* right = middle->next;
	middle->next = nullptr;
	ListNode* left = head;
	
	left = sortList(left);
	right = sortList(right);
	
	return mergeTwoSortedLinkedLists(left, right);
}
```

### Intersection Node of Two Linked Lists
```cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
	if (headA == NULL || headB == NULL) {
		return NULL;
	}
	ListNode* t1 = headA;
	ListNode* t2 = headB;
	
	while (t1!=t2) {
		t1 = t1->next;
		t2 = t2->next;
		
		if (t1 == t2) {
			return t1;
		}
		if (t1 == NULL) {
			t1 = headB;
		}
		if (t2 == NULL) {
			t2 = headA;
		}
	}
	
	return t1;
}
```

reverse K nodes groups:
```cpp
ListNode* reverseList(ListNode* head) {
	ListNode* prev = nullptr;
	ListNode* next = head;
	ListNode* curr = head;
	
	while (next!=nullptr) {
		next = curr->next;
		curr->next = prev;
		prev = curr;
		curr = next;
	}
	
	return prev;
}

ListNode* getKthNode(ListNode* temp, int k){
	k -= 1; 
	while(temp != nullptr && k > 0){
		k--; 
		temp = temp -> next; 
	}
	return temp; 
}

ListNode* reverseKGroup(ListNode* head, int k) {
	ListNode* temp = head; 
	ListNode* prevLast = nullptr; 
	
	while(temp != nullptr){
		ListNode* kThNode = getKthNode(temp, k); 
		if(kThNode == nullptr){
			if(prevLast){
				prevLast -> next = temp; 
			}
			break; 
		}
		
		ListNode* nextNode = kThNode -> next;
		kThNode -> next = nullptr; 
		reverseList(temp); 
		
		if(temp == head){
			head = kThNode;
		}else{
			prevLast -> next = kThNode; 
		}
		
		prevLast = temp; 
		temp = nextNode; 
	}
	
	return head; 
}
```


# Recursion
[Binary Exponentiation - Errichto](https://youtu.be/L-Wzglnm4dM?si=Mj1RGDW4XQHRt1hA)
```cpp
double myPow(double x, int n) {
	double res = 1;
	long long nn = n;
	if (nn<0) {
		nn = -1*nn;
		x = 1/x;
	}
	while (nn > 0) {
		if (nn%2 == 1) res*=x;
		x *= x;
		nn /= 2;
	}
	
	return res;
}
```

Generate Binary Strings Without Adjacent Zeros (Iterative)
```cpp
vector<string> generate_binary_strings(vector<string> bs, int n) {
	vector<string> temp = bs; 
    while (n--) {
        vector<string> current = temp;
        temp.clear();
		
        for (const auto& b : current) {
            if (b.back() == '0') {
                temp.push_back(b + '1');
            } else {
                temp.push_back(b + '0');
                temp.push_back(b + '1');
            }
        }
    }
    return temp;
}

vector<string> validStrings(int n) {
	return generate_binary_strings({"0", "1"}, n-1); 
}
```

Generate Binary Strings Without Adjacent Zeros (Recursive)
```cpp
vector<string> generate_binary_strings(vector<string> bs, int n) {
	if (n == 0) return bs;
	
	vector<string> current = bs;
	bs.clear();
	for (const auto& b : current) {
		if (b.back() == '0') {
			bs.push_back(b + '1');
		} else {
			bs.push_back(b + '0');
			bs.push_back(b + '1');
		}
	}
	
	return generate_binary_strings(bs, n-1);
}

vector<string> validStrings(int n) {
	return generate_binary_strings({"0","1"}, n-1);
}
```

## Recursive Backtracking
Recursive backtracking is complex, but very useful when we have to conditionally skip/create a branch of choices
we can understand recursive backtracking with problems like conditional subsets
how recursion might help ? -> In recursion if we know a branch won't yield the results we can simply skip whole branch. 

lets say we have an array and we want to generate subsets what can we do ?
Methods:
1) Use bitmask (2^n * n) -> use only if use want all subsets as it's same as recursive but is simpler than recursive  
2) Use Recursion -> for each element think if you want to take it or not sequentially
		Example: Combination Sum I or Subsets I 
3) Use Recursion -> sequentially pick how many elements you want in subsets
		Example: Combination Sum II or Subsets II


### Subsets II
```cpp
void findSubsets(int pos, vector<int>& nums, vector<vector<int>>& ans, vector<int>& ds){
	ans.push_back(ds);
	for (int i=pos; i<nums.size(); i++){
		if(i!=pos && nums[i] == nums[i-1]) continue;
		ds.push_back(nums[i]);
		findSubsets(i+1, nums, ans, ds);
		ds.pop_back();
	}
}

vector<vector<int>> subsetsWithDup(vector<int>& nums) {
	vector<vector<int>> ans;
	vector<int> ds;
	
	sort(nums.begin(), nums.end());
	findSubsets(0, nums, ans, ds);
	
	return ans;
}
```

### Combination Sum
all subsequences that sum up to target and **each element can be selected multiple times**
```cpp
void find_combinations (int pos, int target, vector<int>& candidates, vector<vector<int>>& ans, vector<int>& ds) {
	if (pos == candidates.size()) {
		if (target == 0){
			ans.push_back(ds);
		}
		return;
	}
	
	if (candidates[pos] <= target) {
		ds.push_back(candidates[pos]);
		// taking the same element again
		find_combinations(pos, target - candidates[pos], candidates, ans, ds);
		// after backtracking if this branch final ds does not sum up to target we pop back the element
		ds.pop_back();
	}
	
	// taking the next element
	find_combinations(pos+1, target, candidates, ans, ds);
}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
	vector<vector<int>> ans;
	vector<int> ds;
	
	find_combinations(0, target, candidates, ans, ds);
	return ans;
}
```

### Combination Sum II
all subsequences that sum up to target and **each element can be selected once only**

to solve this we can use similar method as used in Combination Sum, i.e at each point select or not select and then move ahead to next recursion with updated target and length of array. We just need to move to next element no matter what and use set to store.

```cpp
void find_unique_combinations (int pos, int target, vector<int>& candidates, set<vector<int>>& ans, vector<int>& ds) {
	if (pos == candidates.size()) {
		if (target == 0){
			if (ans.find(ds) == ans.end()) {
				ans.insert(ds);
			}
		}
		return;
	}
	
	if (candidates[pos] <= target) {
		ds.push_back(candidates[pos]);
		find_unique_combinations(pos+1, target - candidates[pos], candidates, ans, ds);
		ds.pop_back();
	}
	
    find_unique_combinations(pos+1, target, candidates, ans, ds);
}

vector<vector<int>> combinationSum2(vector<int> &candidates, int target) {
	set<vector<int>> ans;
	vector<int> ds;
	vector<vector<int>> res;
	sort(candidates.begin(), candidates.end());
	find_unique_combinations(0, target, candidates, ans, ds);
	for (auto i : ans) {
		res.push_back(i);
	}
	
	return res;
}
```

Here instead of picking not picking each element we pick every possible element that is unique for given length of subset
```cpp
void find_unique_combinations(int pos, int target, vector<int> &candidates, vector<vector<int>> &ans, vector<int> &ds) {
	if (target == 0) {
		ans.push_back(ds);
		return;
	}
	for (int i = pos; i < candidates.size(); i++) {
		// Skip duplicates at the same level
		if (i != pos && candidates[i] == candidates[i - 1])
			continue;
		// Stop if the current element is too large
		if (candidates[i] > target)
			break;
		// Include the current element
		ds.push_back(candidates[i]);
		find_unique_combinations(i + 1,target - candidates[i],candidates,ans,ds);
		ds.pop_back(); // Backtrack
	}
}

vector<vector<int>> combinationSum2(vector<int> &candidates, int target) {
	vector<vector<int>> ans;
	vector<int> ds;
	sort(candidates.begin(), candidates.end());
	find_unique_combinations(0, target, candidates, ans, ds);
	return ans;
}
```

## Standard Recursive questions

### Palindrome Partitioning
```cpp
bool ispalin(string s, int start, int end) {
	while (start<=end) {
		if (s[start++] != s[end--]) return false;
	}
	return true;
}

void func(int pos, string s, vector<vector<string>>& ans, vector<string>& ds) {
	if (pos == s.size()) {
		ans.push_back(ds);
		return;
	}
	
	for (int i=pos; i<s.size(); i++) {
		if (ispalin(s, pos, i)) {
			ds.push_back(s.substr(pos, i-pos+1));
			func(i+1, s, ans, ds);
			ds.pop_back();
		}
	}
}

vector<vector<string>> partition(string s) {
	vector<vector<string>> ans;
	vector<string> ds;
	
	func(0, s, ans, ds);
	return ans;
}
```

### Word Search in 2D Grid
```cpp
bool try_search(int pos, vector<vector<char>> board, string word, int i, int j, int n, int m) {
	if (pos == word.length()) return true;

	if (i < 0 || j < 0 || j == m || i == n || board[i][j] != word[pos] or board[i][j] == '!')
		return false;

	char temp = board[i][j]; 
	board[i][j] = '!';
	bool bottom = try_search(pos+1, board, word, i+1, j, n, m);
	bool right = try_search(pos+1, board, word, i, j+1, n, m);
	bool top = try_search(pos+1, board, word, i-1, j, n, m);
	bool left = try_search(pos+1, board, word, i, j-1, n, m);
	board[i][j] = temp;
	
	return top || bottom || right || left;
}

bool exist(vector<vector<char>>& board, string word) {
	int n = board.size();
	int m = board[0].size();
	for (int i=0; i<n; i++) {
		for (int j=0; j<m; j++) {
			if (board[i][j] == word[0]){
				if (try_search(0, board, word, i, j, n, m)) return true;
			} 
		}
	}
	return false;
}
```



# Bit Manipulation
[Bitwise operations for beginners - Codeforces](https://codeforces.com/blog/entry/73490)
https://youtu.be/LGrE0siZ-ZA?si=Ohjq6KG7pHqf8st0

### Even or Odd
```cpp
if (x%2 == 0)    // even
if (x&1 == 0)    // even (takes O(1) time dbetter way)
```
as last bit of every even number is `0`

### Check if number is prime
```cpp
bool is_prime(int n) {
	if (n<=1) return false;
	if (n == 2) return true;
	if ((n&1) == 0) return false;
	
	int root = sqrt(n);
	for (int i=3; i<=root; i+=2) {
		if (n%i == 0) {
			return false;
		}
	}
	return true;
}
```

### Prime factors of a number
if a number is divisible by `i`(lets say 2) then divide it till it can so that all multiples of `i` are removed and at last as we are iterating only till sqrt of n if a number is still in n then it is the prime factor above sqrt of n.
```cpp
vector<int> prime_factors(int n) {
	int root = sqrt(n);
	vector<int> factors;
	
	for (int i=2; i<=root; i++) {
		if (n%i == 0) {
			factors.push_back(i);
			while (n%i == 0) {
				n = n/i;
			}
		}
	}
	if (n!=1) factors.push_back(n);
	
	return factors;
}
```

### factors of a number
```cpp
vector<int> all_factors(int n) {
	vector<int> f1, f2;
	int root = sqrt(n);
	
	for (int i=1; i<=root; i++) {
		if (n%i == 0) {
			f1.push_back(i);
			if (i != n/i) {
				f2.insert(f2.begin(), n/i);
			}
		}
	}
	
	vector<int> result;
	result.reserve(f1.size() + f2.size());
	result.insert(result.end(), f1.begin(), f1.end());
	result.insert(result.end(), f2.begin(), f2.end());
	return result;
}
```

### Sieve of Eratosthenes
prime till n, mark all multiples of a number as 0 in an array
```cpp
vector<int> prime_till_n(int n) {
	int primes[n+1];
	for (int i=2; i<=n; i++) primes[i] = 1;
	
	for (int i=2; (i*i)<=n; i++) {
		if (is_prime(i) == 1) {
			for (int j=i*i; j<=n; j+=i){
				primes[j] = 0;
			}
		}
	}
	
	vector<int> result;
	for (int i=2; i<=n; i++) {
		if (primes[i] == 1) result.push_back(i);
	}
	
	return result;
}
```

### check if number is power of 2
```cpp
bool powerof2(int x){
	return x && !( x&(x-1) )
}
```

### Multiply or Divide a number by 2^k
-  `x * 2^k`  = `x<<k`
-  `x / 2^k  = x>>k`

### kth bit:
`1<<k = 2^k`

- Toggle kth bit: `x ^ (1<<k)`
- Set kth bit (make kth bit `1`): `x | (1<<k)`
- Unset kth bit (make kth bit `0`): `x & ~(1<<k)`
- Remove the last set bit (rightmost): `x & (x-1)`

### Swap 2 numbers using bit manipulation
- `x = x^y` => `x = x^y , y = y`
- `y = x^y` => `x = x^y , y = x^y^y = x`
- `x = x^y` => `x = x^y^x = y , y = x`

### Count number of set bits (traditional method)
```cpp
int count_set_bits(int n) {
	int cnt = 0;
	while (n>1) {
		cnt += (n&1);
		n = n>>1; 
	}
	if (n==1) cnt+=1;
	return cnt;
}
```

### Count number of set bits (each step we turn off the right most bit)
```cpp
int count_set_bits(int n) {
	int cnt = 0;
	while (n!=0) {
		n = n&(n-1);
		cnt++; 
	}
	return cnt;
}
```

### Divide without division operator
```cpp
int divide(int dividend, int divisor) {
	if(dividend == divisor) return 1;
	bool sign = true;
	if ((dividend>=0 && divisor<0) || (dividend<0 && divisor>0)) sign = false;
	long long ans=0;
	long long n = abs((long long)dividend);
	long long d = abs((long long)divisor);
	
	while (n>=d) {
		long long cnt = 0;
		while(n>=(d<<(cnt+1))){
			cnt++;
		}
		ans += (long long)1<<cnt;
		n = n - (d*((long long)1<<cnt));
	}
	
	if (ans>INT_MAX && sign) return INT_MAX;
	if (ans>INT_MAX && !(sign)) return INT_MIN;
	
	return sign ? ans : (-1*ans);
}
```

### XOR from 1 to n (follows a pattern)
```cpp
int xor_till_n(int n) {
	if (n%4 == 0) return n;
	else if (n%4 == 1) return 1;
	else if (n%4 == 2) return n+1;
	else return 0;
}
```

### Single Number I
all numbers appear twice except one that appear once
sol :- xor of all numbers 
```cpp
int singleNumber(vector<int>& nums) {
	int temp = 0;
	for (auto i: nums){
		temp ^= i;
	}
	return temp;
}
```

### Single Number II
all numbers appear thrice except one that appear once
sol 1:- count the number of set bits at a single position in all numbers, if it's multiple of 3 then that bit of answer is 0 otherwise it's 1 
```cpp
int singleNumber(vector<int>& nums) {
	int ans = 0;
	for (int i=0; i<31; i++) {
		int pos_bit = 1<<i;
		int cnt = 0;
		for (auto num: nums) {
			if ((num&pos_bit) != 0) {
				cnt++;
			}
		}
		if ((cnt%3) != 0) {
			ans |= pos_bit;
		}
	}
	return ans;
}
```

sol 2:- ones store numbers that appear once, if a number appear second time we store it in twos so, twos store numbers that appear twice and if number appear thrice we remove it from twos
- now think that all thrice appearing numbers are together at start and last one is the answer (it's for understanding but it all works out for any order because we are dealing with bits)
- now first we check if new number is not in twos (as not of number and number is zero) and if it's not in twos we add it in ones by xor (so that when we get second time it deletes and if it's first time xor of 0 and number occurs leading to number)
- now check if new number is already in ones
```cpp
int singleNumber(vector<int>& nums) {
	int ones = 0;
	int twos = 0;
	
	for (const int num : nums) {
		ones ^= (num & ~twos);
		twos ^= (num & ~ones);
	}
	
	return ones;
}
```

### Single Number III
two elements appear only once and all the other elements appear exactly twice.
```cpp
vector<int> singleNumber(vector<int>& nums) {
	int n = nums.size();
	long long xorr=0;
	for (auto num: nums) {
		xorr ^= num;
	}
	
	long long right_most_set_bit = (xorr&(xorr-1))^xorr;
	int bucket_1 = 0;
	int bucket_2 = 0;
	
	for (auto num: nums) {
		if (num&right_most_set_bit) {
			bucket_1 ^= num;
		} else {
			bucket_2 ^= num;
		}
	}
	
	return {bucket_1, bucket_2};
}
```

### Generate a Power set i.e all subsets i.e. all subsequences of an array
outer loop from `0` to `2^n` generates masks 
inner loop iterate through each bit in mask and if bit is `1` then take element from `nums` from that position.

we use mask as a map to guide us which element to take by `&` operator checking for each bit if we want to take that element represented by `i` bit.
```cpp
vector<vector<int>> subsets(vector<int>& nums) {
	int n = nums.size();
	vector<vector<int>> subsets;
	
	for (long long mask=0; mask<(1<<n); mask++) {
		vector<int> curr;
		for (int j=0; j<n; j++) {
			if (mask & (1<<j)) {
				curr.push_back(nums[j]);
			}
		}
		subsets.push_back(curr);
	}
	return subsets;
}
```

Subsequences with sum equal to k 
```cpp
int subsequence_sum_equal_k(vector<int>& nums, int k) {
	int n = nums.size();
	int curr_cnt = 0;
	
	for (long long mask=0; mask<(1<<n); mask++) {
		long long sum_of_this_subset = 0;
		for (int j=0; j<n; j++) {
			if (mask & (1<<j)) {
				sum_of_this_subset += nums[j];
			}
		}
		if (sum_of_this_subset == k) {
			curr_cnt++;
		}
	}
	
	return curr_cnt;
}
```

# Stacks and Queue
- Stack => Last In First Out (LIFO) rule
- Queue => First In First Out (FIFO) rule

stack using array
```cpp
class Stack {
	int size;
	int* arr;
	int top;
	
	public:
		Stack() {
			top = -1;
			size = 5;
			arr = new int[size];
		}
		
		void push(int n) {
			if (top+1 > size) {
				cout << "stack full" << endl;
			}
			top++;
			arr[top] = n;
		}
		
		int pop() {
			if (top-1 < 0) {
				cout << "stack empty" << endl;
			}
			int res = arr[top];
			top--;
			
			return res;
		}
		
		int Top() {
			return arr[top];
		}
		
		int Size() {
			return top+1;
		}
};
```

queue using array (this implementation keeps start at 0)
```cpp
class Queue {
	int size;
	int* arr;
	int start;
	int end;
	
	public:
		Queue() {
			start = -1;
			end = -1;
			size = 5;
			arr = new int[size];
		}
		
		void push(int n) {
			if (end+1 > size) {
				cout << "queue full" << endl;
				return;
			}
			if (start == -1) {
				start = 0;
			}
			end++;
			arr[end] = n;
		}
		
		int pop() {
			if (start == -1) {
				cout << "queue empty" << endl;
				return INT_MIN;
			}
			int res = arr[start];
			for (int i=1; i<=end; i++) {
				arr[i-1] = arr[i];
			}
			end--;
		
			return res;
		}
		
		int Top() {
			if (end == -1) {
				cout << "queue empty" << endl;
				return INT_MIN;
			}
			return arr[end];
		}
		
		int Size() {
			return end+1;
		}
};
```

### Stack using Queue
```cpp
class Stack_using_Queue {
	queue<int> q;
	
	public:
		void Push(int n) {
			q.push(n);
			for (int i=0; i<q.size()-1; i++) {
				q.push(q.front());
				q.pop();
			}
		}
		
		int Pop() {
			int temp = q.front();
			q.pop();
			return temp;
		}
		
		int Top() {
			return q.front();
		}
		
		int Size() {
			return q.size();
		}
};
```

### Queue using Stack
```cpp
class Queue_using_Stack {
	stack<int> s1, s2;
	
	public:
		void Push(int n) {
			while (!s1.empty()) {
				s2.push(s1.top());
				s1.pop();
			}
			s2.push(n);
			while (!s2.empty()) {
				s1.push(s2.top());
				s2.pop();
			}
		}
		
		int Pop() {
			int temp = s1.top();
			s1.pop();
			return temp;
		}
		
		int Top() {
			return s1.top();
		}
		
		int Size() {
			return s1.size();
		}
};
```

### Check for Balanced Parentheses
```cpp
bool isValid(string s) {
	stack<char> st;
	for (auto c: s) {
		if (c == '(' || c == '{' || c == '[') {
			st.push(c);
		} else {
			if (st.size() == 0) return false;
			if ((st.top() == '(' && c == ')') || (st.top() == '{' && c == '}') || (st.top() == '[' && c == ']')) {
				st.pop();
				continue;
			} else {
				return false;
			}
		}
	}
	return st.empty();
}
```

## Infix Prefix Postfix
### Infix to Postfix
```cpp
string infix_str = "a+b*(c^d-e)^(f+g*h)-i";
string postfix_str = "abcd^e-fgh*+^*+i-";
string prefix_str = "-+a*b^-^cde+f*ghi";
```
- if any operand add to result
- if opening bracket add to operators stack, if closing bracket pop and add to result till opening bracket comes
- if an operator 
	- if it's precedence is smaller than stack's top operator i.e. last operator then remove the operator till stack is empty of the new operator have greater precedence than that in stack top
	- finally add the new operator in stack
- at the end if any operators are left print out all
```cpp
string infix_to_postfix(string s) {
	stack<char> st;
	string res;
	
	for (auto c: s) {
		if ((c>='A' && c<='Z') || (c>='a' && c<='z') || (c>='0' && c<='9')) {
			res += c;
		} else if (c == '(') {
			st.push(c);
		} else if (c == ')') {
			while (st.top() != '(') {
				res += st.top();
				st.pop();
			}
			st.pop();
		} else {
			while (!st.empty() && prec(c)<=prec(st.top())) {
				res += st.top();
				st.pop();
			}
			st.push(c);
		}
	}
	
	while (!st.empty()) {
		res += st.top();
		st.pop();
	}
	
	return res;
}
```

### Infix to Prefix
- reverse the infix (make sure the brackets are reversed after reversing so that `(` comes before `)`)
- result = apply infix to postfix with condition that if `^` then pop till `<=` precedence otherwise pop till `<` precedence
- reverse the result (make sure the brackets are reversed after reversing so that `(` comes before `)`)
```cpp
string infix_to_prefix(string s) {
	stack<char> st;
	string res;

	reverse(s.begin(), s.end());
	for (int i = 0; i<s.length(); i++) {
        if (s[i] == '(') {
            s[i] = ')';
        } else if (s[i] == ')') {
            s[i] = '(';
        }
    }
	
	for (auto c: s) {
		if ((c>='A' && c<='Z') || (c>='a' && c<='z') || (c>='0' && c<='9')) {
			res += c;
		} else if (c == '(') {
			st.push(c);
		} else if (c == ')') {
			while (st.top() != '(') {
				res += st.top();
				st.pop();
			}
			st.pop();
		} else {
			if (c == '^') {
				while (!st.empty() && prec(c)<=prec(st.top())) {
					res += st.top();
					st.pop();
				}
			} else {
				while (!st.empty() && prec(c)<prec(st.top())) {
					res += st.top();
					st.pop();
				}
			}
			st.push(c);
		}
	}
	
	while (!st.empty()) {
		res += st.top();
		st.pop();
	}
	
	reverse(res.begin(), res.end());
	for (int i = 0; i<res.length(); i++) {
        if (res[i] == '(') {
            res[i] = ')';
        } else if (res[i] == ')') {
            res[i] = '(';
        }
    }
	
	return res;
}
```

### Postfix to Infix
- add operands to stack
- if encounter a operator takes last 2 operands from stack, add operator between them and wrap them into a single unit such that `top2 + operator + top1`
```cpp
string postfix_to_infix(string s) {
	stack<string> st;
	
	for (auto c: s) {
		if ((c>='A' && c<='Z') || (c>='a' && c<='z') || (c>='0' && c<='9')) {
			string temp;
			temp.push_back(c);
			st.push(temp);
		} else {
			string t1 = st.top();
			st.pop();
			string t2 = st.top();
			st.pop();
			string temp = '(' + t2 + c + t1 + ')';
			st.push(temp);
		}
	}
	
	return st.top();
}
```

### Prefix to Infix
- start iterator from end
- add operands to stack
- if encounter a operator takes last 2 operands from stack, add operator between them and wrap them into a single unit such that `top1 + operator + top2`
```cpp
string prefix_to_infix(string s) {
	stack<string> st;
	int i = s.length()-1;
	
	while (i >= 0) {
		char c = s[i];
		if ((c>='A' && c<='Z') || (c>='a' && c<='z') || (c>='0' && c<='9')) {
			string temp;
			temp.push_back(c);
			st.push(temp);
		} else {
			string t1 = st.top();
			st.pop();
			string t2 = st.top();
			st.pop();
			string temp = '(' + t1 + c + t2 + ')';
			st.push(temp);
		}
		i--;
	}
	
	return st.top();
}
```

### Postfix to Infix
- add operands to stack
- if encounter a operator takes last 2 operands from stack, add operator between them and wrap them into a single unit such that `operator + top2 + top1`
```cpp
string postfix_to_prefix(string s) {
	stack<string> st;
	
	for (auto c: s) {
		if ((c>='A' && c<='Z') || (c>='a' && c<='z') || (c>='0' && c<='9')) {
			string temp;
			temp.push_back(c);
			st.push(temp);
		} else {
			string t1 = st.top();
			st.pop();
			string t2 = st.top();
			st.pop();
			string temp = c + t2 + t1;
			st.push(temp);
		}
	}
	
	return st.top();
}
```

### Postfix to Infix
- add operands to stack
- if encounter a operator takes last 2 operands from stack, add operator between them and wrap them into a single unit such that `top1 + top2 + operator`
```cpp
string prefix_to_postfix(string s) {
	stack<string> st;
	int i = s.length()-1;
	
	while (i >= 0) {
		char c = s[i];
		if ((c>='A' && c<='Z') || (c>='a' && c<='z') || (c>='0' && c<='9')) {
			string temp;
			temp.push_back(c);
			st.push(temp);
		} else {
			string t1 = st.top();
			st.pop();
			string t2 = st.top();
			st.pop();
			string temp = t1 + t2 + c;
			st.push(temp);
		}
		i--;
	}
	
	return st.top();
}
```


## Monotonic Stack/Queue problems

### Next greater element
- start from end
- keep stack in ascending order and if a number is > top then remove from stack till we can place it in ascending order
```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
	int n = nums2.size()-1;
	unordered_map<int, int> reference;
	stack<int> nge;
	
	for (int i=n; i>=0; i--) {
		while (!nge.empty() && (nge.top() < nums2[i])) {
			nge.pop();
		}
		if (nge.empty()) {
			reference[nums2[i]] = -1;
		} else {
			reference[nums2[i]] = nge.top();
		}
		nge.push(nums2[i]);
	}
	
	vector<int> res(nums1.size(), -1);
	for (int i=0; i<nums1.size(); i++) {
		res[i] = reference[nums1[i]];
	}
	
	return res;
}
```

### Next greater element (circular)
- start from twice of end and use %n to get elements 
- so for first n iterations of loop only push pop will be executed and thus leaving us with the rightmost greater elements in order in stack.
- keep stack in ascending order and if a number is > top then remove from stack till we can place it in ascending order
```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
	int n = nums.size();
	vector<int> nge(n, -1);
	stack<int> st;
	
	for (int i=2*n-1; i >= 0; i--) {
		while (!st.empty() && st.top() <= nums[i % n]) {
			st.pop();
		}
		
		if (i < n) {
			if (!st.empty()) nge[i] = st.top();
		}
		st.push(nums[i % n]);
	}
	
	return nge;
}
```

### Trapping Rain Water
for any `arr[i]` we know it can store water above it of height `min(leftMax, rightMax) - arr[i]`

so for each position we need to find the maximum height on both sides of that position, which is equal to `prefixMax` and `suffixMax`, but finding all max would take `O(N)` for both prefix and suffix.
we can take `2 pointer` each pointing to the max.

- prefixMax suffixMax method
```cpp
int trap(vector<int>& arr) {
	int n = arr.size();
	
	int prefix[n], suffix[n];
	prefix[0] = arr[0];
	for (int i = 1; i < n; i++) {
		prefix[i] = max(prefix[i - 1], arr[i]);
	}
	suffix[n - 1] = arr[n - 1];
	for (int i = n - 2; i >= 0; i--) {
		suffix[i] = max(suffix[i + 1], arr[i]);
	}
	
	int waterTrapped = 0;
	for (int i = 0; i < n; i++) {
		waterTrapped += min(prefix[i], suffix[i]) - arr[i];
	}
	
	return waterTrapped;
}
```

- using 2 pointer
```cpp
int trap(vector<int>& height) {
	int n = height.size();
	int waterTrapped = 0; 
	int leftMax = 0, rightMax = 0;
	int l=0, r=n-1;
	
	while (l<r) {
		if (height[l] <= height[r]) {
			if (leftMax > height[l]) {
				waterTrapped += leftMax - height[l]; 
			} else {
				leftMax = height[l];
			}
			l = l+1;
		} else {
			if (rightMax > height[r]) {
				waterTrapped += rightMax - height[r];
			} else {
				rightMax = height[r];
			}
			r = r-1;
		}
	}
	
	return waterTrapped;
}
```

asteroid collision
```cpp
vector<int> asteroidCollision(vector<int>& asteroids) {
	int n = asteroids.size();
	vector<int> st;
	
	for (int i=0; i<n; i++) {
		if (asteroids[i] >= 0 ) st.push_back(asteroids[i]);
		else {
			while(!st.empty() && st.back()>0 && (st.back() < abs(asteroids[i]))){
				st.pop_back();
			}
			if (!st.empty() && st.back()==abs(asteroids[i])) {
				st.pop_back();
			} else if (st.empty() || st.back()<0) {
				st.push_back(asteroids[i]);
			}
		}
	}
	
	return st; 
}
```

### Sum of subarray minimums
- find after how many elements we get next smallest
- then PnC 
```cpp
vector<int> nextSmallerElement(vector<int>& nums) {
	int n = nums.size();
	vector<int> res(n, -1);
	stack<int> nse;
	
	for (int i=n-1; i>=0; i--) {
		while (!nse.empty() && (nums[nse.top()] >= nums[i])) {
			nse.pop();
		}
		if (nse.empty()) {
			res[i] = n;
		} else {
			res[i] = nse.top();
		}
		nse.push(i);
	}
	
	return res;
}

vector<int> prevSmallerElement(vector<int>& nums) {
	int n = nums.size();
	vector<int> res(n, -1);
	stack<int> pse;
	
	for (int i=0; i<n; i++) {
		while (!pse.empty() && (nums[pse.top()] > nums[i])) {
			pse.pop();
		}
		if (pse.empty()) {
			res[i] = -1;
		} else {
			res[i] = pse.top();
		}
		pse.push(i);
	}
	
	return res;
}

int sumSubarrayMins(vector<int>& arr) {
	int n = arr.size();
	int mod = 1e9+7;
	long long res = 0;
	
	vector<int> nse = nextSmallerElement(arr);
	vector<int> pse = prevSmallerElement(arr);
	
	for (int i=0; i<n; i++) {
		int left = i-pse[i];
		int right = nse[i]-i;
		res = (res + (right * left * (long long)1 * arr[i]) % mod) % mod; 
	}
	
	return res;
}
```

### Sum of subarray ranges
```cpp

```


### Remove K digits to create smallest number
greedy - at each `i` iteratively check if it is smaller than stack's top element, if it is, replace it.
```cpp
string removeKdigits(string num, int k) {
	int n = num.length();
	stack<char> st;
	
	for (int i=0; i<n; i++) {
		while (!st.empty() && k>0 && ((st.top()-'0') > (num[i]-'0'))) {
			st.pop();
			k--;
		}
		st.push(num[i]);
	}
	
	while (k>0) {
		st.pop();
		k--;
	}
	
	if (st.empty()) return "0";
	
	string res;
	while (!st.empty()) {
		res += st.top();
		st.pop();
	}
	
	while (res.size()!=0 && res.back() == '0') {
		res.pop_back();
	}
	
	reverse(res.begin(), res.end());
	
	if (res.empty()) return "0";
	
	return res;
}
```

### Largest Rectangle in Histogram
- to find the width for a height `i` we need the smallest previous and smallest next
- then maximum area by that height`i` is `heights[i] * (nse[i]-pse[i]-1)` 
```cpp
vector<int> nextSmallerElement(vector<int>& nums) {
	int n = nums.size();
	vector<int> res(n, -1);
	stack<int> nse;
	
	for (int i=n-1; i>=0; i--) {
		while (!nse.empty() && (nums[nse.top()] >= nums[i])) {
			nse.pop();
		}
		if (nse.empty()) {
			res[i] = n;
		} else {
			res[i] = nse.top();
		}
		nse.push(i);
	}
	
	return res;
}

vector<int> prevSmallerElement(vector<int>& nums) {
	int n = nums.size();
	vector<int> res(n, -1);
	stack<int> pse;
	
	for (int i=0; i<n; i++) {
		while (!pse.empty() && (nums[pse.top()] >= nums[i])) {
			pse.pop();
		}
		if (pse.empty()) {
			res[i] = -1;
		} else {
			res[i] = pse.top();
		}
		pse.push(i);
	}
	
	return res;
}

int largestRectangleArea(vector<int>& heights) {
	int res = 0;
	vector<int> pse = prevSmallerElement(heights);
	vector<int> nse = nextSmallerElement(heights);
	
	for (int i=0; i<heights.size(); i++) {
		res = max(res, (heights[i]*(nse[i] - pse[i]-1)));
	}
	
	return res;
}
```

### Maximal Rectangle
- treat each row as a histogram and column as heights, now use the previous question
- to find heights use a prefix sum on each col with condition that if encountered 0 then reset sum 
```cpp
 vector<int> nextSmallerElement(vector<int>& nums) {
	int n = nums.size();
	vector<int> res(n, -1);
	stack<int> nse;
	
	for (int i=n-1; i>=0; i--) {
		while (!nse.empty() && (nums[nse.top()] >= nums[i])) {
			nse.pop();
		}
		if (nse.empty()) {
			res[i] = n;
		} else {
			res[i] = nse.top();
		}
		nse.push(i);
	}
	
	return res;
}

vector<int> prevSmallerElement(vector<int>& nums) {
	int n = nums.size();
	vector<int> res(n, -1);
	stack<int> pse;
	
	for (int i=0; i<n; i++) {
		while (!pse.empty() && (nums[pse.top()] >= nums[i])) {
			pse.pop();
		}
		if (pse.empty()) {
			res[i] = -1;
		} else {
			res[i] = pse.top();
		}
		pse.push(i);
	}
	
	return res;
}

int largestRectangleArea(vector<int>& heights) {
	int res = 0;
	vector<int> pse = prevSmallerElement(heights);
	vector<int> nse = nextSmallerElement(heights);
	
	for (int i=0; i<heights.size(); i++) {
		res = max(res, (heights[i]*(nse[i] - pse[i]-1)));
	}
	
	return res;
}

int maximalRectangle(vector<vector<char>>& matrix) {
	int rows = matrix.size();
	int cols = matrix[0].size();
	vector<vector<int>> prefixSum(rows, vector<int>(cols, 0));
	int maxArea = 0;
	
	for (int j=0; j<cols; j++) {
		int curr_sum=0;
		for (int i=0; i<rows; i++) {
			curr_sum += matrix[i][j]-'0';
			if (matrix[i][j] == '0') curr_sum = 0;
			prefixSum[i][j] = curr_sum; 
		}
	}
	
	for (int i=0; i<rows; i++) {
		maxArea = max(maxArea, largestRectangleArea(prefixSum[i]));
	}
	
	return maxArea;
}
```

### Sliding Window maximum
maintain a dequeue that has elements in descending order.
```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
	if (k==1) return nums;
	int n = nums.size();
	vector<int> swmax;
	deque<int> dq;
	
	for (int i=0; i<n; i++) {
		// this if ensures we maintain correct window 
		if (!dq.empty() && dq.front() <= i - k) {
			dq.pop_front();
		}
		// maintain dq in decending order (front element is largest)
		while (!dq.empty() && nums[dq.back()] <= nums[i]) {
			dq.pop_back();
		}
		dq.push_back(i);
	
		if (i >= k-1) swmax.push_back(nums[dq.front()]); 
	}
	
	return swmax;
}
```






# Sliding Window and 2 Pointers


### Longest Substring Without Repeating Characters
- use sliding window i.e. 2 pointers and an unordered map 
```cpp
int lengthOfLongestSubstring(string s) {
	if (s.length() == 0) return 0; 
	int start=0, end=0, res=0;
	unordered_map<char, int> mp;
	
	for (int end = 0; end < s.length(); end++) {
        if (mp.find(s[end]) != mp.end() && mp[s[end]] >= start) {
            start = mp[s[end]] + 1; // Move start to after the last occurrence
        }
        res = max(res, end - start + 1); // Update max length
        mp[s[end]] = end; // Update last seen position
    }
	
	return res;
}
```

### Max Consecutive Ones III
```cpp
int longestOnes(vector<int>& nums, int k) {
	int n = nums.size();
	int start = 0, temp = k, res = 0;
	
	for (int end=0; end<n; end++) {
		if (nums[end] == 0) {
			temp--;
		}
		while (temp < 0) {
			if (nums[start] == 0) {
				temp++;
			}
			start++;
		}
		res = max(res, end-start+1);
	}
	
	return res;
}
```

### Longest Repeating Character Replacement
```cpp
int characterReplacement(string s, int k) {
	int n = s.length();
    unordered_map<char, int> mp;
    int start = 0, res = 0, maxFreq = 0;
    
    for (int end = 0; end < n; end++) {
        mp[s[end]]++;
        maxFreq = max(maxFreq, mp[s[end]]); // Track max frequency in window
        
        // If window size - max frequency > k, shrink window
        if (end - start + 1 - maxFreq > k) {
            mp[s[start]]--;
            start++;
        }
        res = max(res, end - start + 1);
    }
    
    return res;
}
```

### Binary Subarrays With Sum
This questions seems similar to **count subarrays with sum equal to k** which is solved by using an `unordered_map` of `prefix sums` so that we can in O(1) search if we have encountered `curr_sum - goal` sum and if we have then we can directly add it's count to ans     
```cpp
int numSubarraysWithSum(vector<int>& nums, int goal) {
	int n = nums.size();
	unordered_map<int, int> mp;
	mp[0] = 1; // Initialize for subarrays starting at index 0
	int res = 0, curr_sum = 0;
	
	for (int i = 0; i<n; i++) {
		curr_sum += nums[i];
		if (mp.find(curr_sum-goal)!=mp.end()) {
			res += mp[curr_sum-goal];
		}
		mp[curr_sum]++;
	}
	
	return res;
}
```

This method takes O(N) time and O(N) space too, but we can also sum elements with Sliding Window using 2 pointers. 
but here's the catch, in Sliding Window we continuously move the `end` pointer and updates `start` upon reaching a condition, 
In this question we would shrink the window (`start++`, `curr_sum -= nums[start]`) as soon as `curr_sum == goal`. This assumes only one subarray ending at end has sum goal, but multiple subarrays can end at end with the same sum due to different combinations of 1s and 2s.

Example: `nums = [1,1,2]`, `goal = 4`. 
Subarrays `[1,1,2]` (sum = 1+1+2 = 4) and `[1,2]` (sum = 1+2 = 3, but we will miss this because we shrink the window after finding `[1,1,2]`). 
**This approach counts only one subarray per window.**

So what we can use the above **prefix sum with hash map** approach.
And **Sliding Window (At Most K)** approach too


### Number of Substrings Containing All Three Characters
Brute force generate all substrings and count frequency
Brute force optimized:
We know that if a substring has `((cnt_a > 0) && (cnt_b > 0) && (cnt_c > 0))` then all substrings that contains that substring are valid so we directly add `n-j` to total count and break that iteration of `j`.
```cpp
int numberOfSubstrings(string s) {
	int n = s.length();
	int res = 0;
	
	for (int i=0; i<n; i++) {
		int cnt_a=0, cnt_b=0, cnt_c=0;
		for (int j=i; j<n; j++) {
			if (s[j] == 'a') cnt_a++;
			if (s[j] == 'b') cnt_b++;
			if (s[j] == 'c') cnt_c++; 
			if (cnt_a && cnt_b && cnt_c) {
				res += n-j;
				break;	
			}
		}
	}
	
	return res;
}
```

Optimal: 
we keep track of last occurrence of a, b and c and when we encounter a character such that all of them becomes > -1, we for sure know that each character has appeared more than once so the least last seen position is the minimal window and anything bigger than that on the left side will be valid.
instead of our usual approach of increasing the window from start by one we find minimal window the expand it to left.
```cpp
int numberOfSubstrings(string s) {
	int n = s.length();
	int res = 0;
	int pos_a=-1, pos_b=-1, pos_c=-1;
	
	for (int i=0; i<n; i++) {
		if (s[i] == 'a') pos_a = i;
		if (s[i] == 'b') pos_b = i;
		if (s[i] == 'c') pos_c = i;
		if (pos_a > -1 && pos_b > -1 && pos_c > -1) {
			int mini1 = min(pos_a, pos_b);
			int mini2 = min(mini1, pos_c);
			res += mini2+1;
		}
	}
	
	return res;
}
```

### Maximum Points You Can Obtain from Cards
Brute Force: Recursion
use recursion where each branch is element taking either from front or back.
```cpp
int rec_sum (vector<int>& cardPoints, int n, int sum, int l, int r, int k) {
	if (k==0) {
		return sum;
	}
	
	int s1 = cardPoints[l]+rec_sum(cardPoints, n, sum, l+1, r,  k-1);
	int s2 = cardPoints[r]+rec_sum(cardPoints, n, sum, l, r-1, k-1);
	return max(s1, s2);
}

int maxScore(vector<int>& cardPoints, int k) {
	int n = cardPoints.size();
	if (k>n) return accumulate(cardPoints.begin(), cardPoints.end(), 0);
	return rec_sum(cardPoints, n, 0, 0, n-1,  k);
}
```

Optimal: Prefix Sum and Sliding Window
Sliding Window of size `n-k`, we will subtract the sum of sliding window from prefix sum leaving us with the sum of k elements distributed on both sides.
```cpp
int maxScore(vector<int>& cardPoints, int k) {
	int n = cardPoints.size();
	int total_sum = accumulate(cardPoints.begin(), cardPoints.end(), 0);
	if (k>n) return total_sum;
	
	vector<int> prefix_sum(n, 0);
	prefix_sum[0] = cardPoints[0];
	for (int i=1; i<n; i++) {
		prefix_sum[i] = cardPoints[i] + prefix_sum[i-1];
	}
	
	int l=-1, r=n-k;
	int res=0;
	for (int i=n-k; i<n; i++) {
		res += cardPoints[i];
	}
	
	l++;
	r++;
	while (r<=n) {
		res = max(res, total_sum - (prefix_sum[r-1] - prefix_sum[l]));
		l++;
		r++;
	}
	
	return res;
}
```

### Subarrays with K Different Integers
instead of finding subarrays with == k distinct integers find subarrays with <=k and <=k-1 distinct integers.
```cpp
int subarraysWithAtMostKDistinct(vector<int>& nums, int k) {
	int n = nums.size();
	int l=0, r=0, res=0;
	unordered_map<int, int> mp;
	
	while (r < n) {
		mp[nums[r]]++;
		
		// shrink window till less than k
		while (mp.size() > k) {
			mp[nums[l]]--;
			if (mp[nums[l]] == 0) {
				mp.erase(nums[l]);
			}
			l++;
		}
		
		// total valid subarray with <=k distinct elements are all elements
		// till r so length of a window == new valid subarrays
		res += r-l+1;
	}
	
	return res;
}

int subarraysWithKDistinct(vector<int>& nums, int k) {
	return (subarraysWithAtMostKDistinct(nums, k) - subarraysWithAtMostKDistinct(nums, k-1));
}
```




# Heap (Priority Queue)
A binary tree where elements are arranged is called heap
two types: min heap and max heap

if a node in binary tree is represented by `i` then it's children are `2*i + 1` and `2*i + 2`.

Max Heap in cpp using priority queue
```cpp
priority_queue<int> maxHeap;

// Insert elements
maxHeap.push(3);
maxHeap.push(1);
maxHeap.push(4);
maxHeap.push(2);

// Print and remove elements (largest first)
cout << "Max Heap elements (in descending order): ";
while (!maxHeap.empty()) {
	cout << maxHeap.top() << " ";
	maxHeap.pop();
}
cout << endl;
```

Min Heap in cpp using priority queue
```cpp
priority_queue<int, vector<int>, greater<int>> minHeap;

// Insert elements
minHeap.push(3);
minHeap.push(1);
minHeap.push(4);
minHeap.push(2);

// Print and remove elements (smallest first)
cout << "Min Heap elements (in ascending order): ";
while (!minHeap.empty()) {
	cout << minHeap.top() << " ";
	minHeap.pop();
}
cout << endl;
```

### Task Scheduler
- while priority queue is not empty {
	- greedly allot n distinct elements if not possible then add idle 
	- replace the alloted elements in priority_queue with a decrement in frequency
- }
```cpp
int leastInterval(vector<char>& tasks, int n) {
	int time = 0;
	
	unordered_map<char, int> mp;
    for (char c : tasks) {
        mp[c]++;
    }
    
    priority_queue<pair<int, char>> pq;
    for (auto [c, f] : mp) {
        pq.push({f, c});
    }
	
    while (!pq.empty()) {
        vector<pair<int, char>> temp;
        int cycle = n + 1; // Maximum tasks in one cycle
        while (cycle > 0 && !pq.empty()) {
            auto [f, c] = pq.top();
            pq.pop();
            if (f > 1) {
                temp.push_back({f - 1, c}); // Decrement frequency
            }
            time++; // One task scheduled
            cycle--;
        }
        // Restore tasks for next cycle
        for (auto p : temp) {
            pq.push(p);
        }
        // Add idle time if queue is not empty
        if (!pq.empty()) {
            time += cycle;
        }
    }
	
	return time;
}
```

Optimal:
- find the number of elements required to fill the max number of elements
- then fill it with other task and order don't matter
```cpp
int leastInterval(vector<char>& tasks, int n) {
    vector<int> freq(26, 0); // Step 1: Frequency array for tasks A-Z
	
    for (auto x : tasks) {
        freq[x - 'A']++; // Step 2: Count frequency of each task
    }
	
    sort(freq.begin(), freq.end(), greater<int>()); // Step 3: Sort frequencies in descending order
	
    int gap = (freq[0] - 1) * n; // Step 4: Calculate initial gaps (idle slots)
	
    for (int i = 1; i < freq.size(); i++) { // Step 5: Fill gaps with other tasks
        gap = gap - min(freq[0] - 1, freq[i]);
    }
	
    return tasks.size() + max(0, gap); // Step 6: Return total time
}
```

Hands of Straights
```cpp
bool isNStraightHand(vector<int>& hand, int groupSize) {
	if (hand.size()%groupSize != 0) return false;
	map<int, int> mp;
	for (int i: hand) {
		mp[i]++;
	}
	
	while (!mp.empty()) {
		int temp = mp.begin()->first;
		for (int i = 0; i < groupSize; i++) {
			if (mp[temp + i] == 0) {
				return false;
			}
			mp[temp + i]--;
			if (mp[temp + i] < 1) {
				mp.erase(temp + i);
			}
		}
	}
	
	return true;
}
```

### Kth Largest Element in a Stream
```cpp
class KthLargest {
    int k;
    priority_queue<int, vector<int>, greater<int>> pq;
public:
    KthLargest(int k, vector<int>& nums) {
        this->k = k;
        for (int num: nums) {
            if (pq.size() < k) {
                pq.push(num);
            } else {
                if (num > pq.top()) {
                    pq.pop();
                    pq.push(num);
                }
            }
        }
    }
    
    int add(int val) {
        if (pq.size() >= k) {
			if (val > pq.top()) {
				pq.pop();
				pq.push(val);
			}
		} else {
			pq.push(val);
		}
        return pq.top();
    }
};
```

### Find Median from Data Stream
```cpp
class MedianFinder {
    priority_queue<int> maxpq; // largest at top so, numbers < median
    priority_queue<int, vector<int>, greater<int>> minpq;  // smallest at top so, numbers > median
public:
    MedianFinder() {}
    
    void addNum(int num) {
        // Push to appropriate heap
        if (maxpq.empty() || num <= maxpq.top()) {
            maxpq.push(num);
        } else {
            minpq.push(num);
        }

        // Balance heaps
        if (maxpq.size() > minpq.size()+1) {
            minpq.push(maxpq.top());
            maxpq.pop();
        } else {
            if (minpq.size() > maxpq.size()) {
                maxpq.push(minpq.top());
                minpq.pop();
            }
        }
    }
    
    double findMedian() {
        // if equal size find mean of mids
        if (maxpq.size() == minpq.size()) {
            return ((maxpq.top() + minpq.top()) / 2.0); 
        } else {
            return maxpq.top();
        }
    }
};
```


# Greedy

### Valid Parenthesis String
```cpp
bool checkValidString(string s) {
	int min_open = 0;   // '*' acts as ')'
	int max_open = 0;   // '*' acts as '('
	for (char c : s) {
		if (c == '(') {
			min_open++;
			max_open++;
		} else if (c == ')') {
			min_open--;
			max_open--;
			if (max_open < 0) return false;
			if (min_open < 0) min_open = 0;
		} else {
			min_open--;
			max_open++;
			if (min_open < 0) min_open = 0;
		}
	}
	return min_open == 0;
}
```

### Jump Game
only problem is when we encounter 0 as if all positive numbers we can definetly reach end by even one one jumps
keep track of max u can reach so that if any point you encounter a `i` that you can't reach/cross-over in any way then return false  
```cpp
bool canJump(vector<int>& nums) {
	int n = nums.size();
	int max_reach = 0;
	
	for (int i=0; i<n; i++) {
		if (i > max_reach) {
			return false;
		}
		max_reach = max(i+nums[i], max_reach);
	}
	
	return true;
}
```

### Jump Game II
find minimum number of jumps
we keep track of range `l -> r` where we can reach
```cpp
int jump(vector<int>& nums) {
	int n = nums.size();
	int cnt = 0, l = 0, r = 0;
	
	while (r < n-1) {
		int max_reach = 0;
		for (int i=l; i<=r; i++) {
			max_reach = max(i+nums[i], max_reach);
		}
		l = r+1;
		r = max_reach;
		cnt++;
	}
	
	return cnt;
}
```

### Candy
Brute Force:
- find minimum number of candies that satisfies from left
- find minimum number of candies that satisfies from right
- maximum of both would satisfy both 
```cpp
int candy(vector<int>& ratings) {
	int n = ratings.size();
	int cnt = 0;
	vector<int> l(n, 0);
	vector<int> r(n, 0);
	l[0] = 1;
	r[n-1] = 1;
	
	for (int i=1; i<n; i++) {
		if (ratings[i] > ratings[i-1]) {
			l[i] = l[i-1]+1;
		} else {
			l[i] = 1;
		}
	}
	
	for (int i=n-2; i>=0; i--) {
		if (ratings[i] > ratings[i+1]) {
			r[i] = r[i+1]+1;
		} else {
			r[i] = 1;
		}
	}
	
	int sum = 0;
	for (int i=0; i<n; i++) {
		sum += max(l[i], r[i]);
	}
	
	return sum;
}
```

Optimal: Peaks
we know peaks would have the largest number of candies so if we can find peaks then we can give every candies accordingly.
main observation => sum from top of peak == sum from bottom of peak, so after reaching peak we can start alloting from 1 to bottom then compare bottom candies and peak and take maximum of them.  
```cpp
int candy(vector<int>& ratings) {
	int n = ratings.size();
	int sum=1, i=1;
	while (i<n) {
		if (ratings[i] == ratings[i-1]) {
			sum++; 
			i++;
			continue;
		}
		
		int peak = 1;
		while (i<n && ratings[i] > ratings[i-1]) {
			peak++;
			sum += peak;
			i++;
		}
		int down = 1;
		while (i<n && ratings[i] < ratings[i-1]) {
			sum += down;
			down++;
			i++;
		}
		
		if (down > peak) {
			sum += down-peak;
		}
	}  
	return sum;
}
```

### Insert Interval
try to optimize with binary search as they are sorted
```cpp
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
	int n = intervals.size();
	vector<vector<int>> res;
	
	int i = 0;
	while (i < n && intervals[i][1] < newInterval[0]) {
		res.push_back(intervals[i]);
		i++;
	}
	
	while (i < n && intervals[i][0] <= newInterval[1]) {
		newInterval[0] = min(newInterval[0], intervals[i][0]);
		newInterval[1] = max(newInterval[1], intervals[i][1]);
		i++;
	}
	
	res.push_back(newInterval);
	while (i < n) {
		res.push_back(intervals[i]);
		i++;
	}
	
	return res;
}
```

### Non-overlapping Intervals
- sort according to end time
- increase counter if end time for a interval is smaller than it's previous one
```cpp
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
	int res = 0;
	
	sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) {
		return a[1] < b[1];
	});
	int prev_end = intervals[0][1];
	
	for (int i = 1; i < intervals.size(); i++) {
		if (prev_end > intervals[i][0]) {
			res++;
		} else {
			prev_end = intervals[i][1];
		}
	}
	
	return res;
}
```







# Binary Trees

Types:
- Full BT                -> either 2 or 0 children
- Complete BT      -> all levels except last have 0 or children, last level / leaf nodes should be as left as possible
- Perfect BT          -> all leaf nodes are at same level
- Balanced BT       -> height of tree must be less than equal to log(N) 
- Degenerate BT   -> each node has a single child (basically a linked list)

Traversal:
- Depth First Technique (DFS):
	- Inorder       (Left Root Right)
	- Preorder    (Root Left Right)
	- Postorder  (Left Right Root)
- Breadth First Technique (BFS):
	visit all nodes at a level before moving to next

## Traversals
### Recursive DFS Traversal
```cpp
// Inorder
void inorder(TreeNode* node, vector<int>& res) {
	if (node == nullptr) return;
	inorder(node->left, res);
	res.push_back(node->val);
	inorder(node->right, res);
}
vector<int> inorderTraversal(TreeNode* root) {
	vector<int> res;
	inorder(root, res);
	return res;
}


// Preorder
void preorder(TreeNode* node, vector<int>& res) {
	if (node == nullptr) return;
	res.push_back(node->val);
	preorder(node->left, res);
	preorder(node->right, res);
}
vector<int> preorderTraversal(TreeNode* root) {
	vector<int> res;
	preorder(root, res);
	return res;
}


// Postorder
void postorder(TreeNode* node, vector<int>& res) {
	if (node == nullptr) return;
	postorder(node->left, res);
	postorder(node->right, res);
	res.push_back(node->val);
}
vector<int> postorderTraversal(TreeNode* root) {
	vector<int> res;
	postorder(root, res);
	return res;
}
```

### BFS Traversal
```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
	vector<vector<int>> res;
	if (root == nullptr) return res;
	queue<TreeNode*> q;
	q.push(root);
	
	while (!q.empty()) {
		vector<int> level;
		int size = q.size();
		for (int i=0; i<size; i++) {
			TreeNode* node = q.front();
			q.pop();
			
			if(node->left!=NULL){
				q.push(node->left);
			}
			if(node->right!=NULL){
				q.push(node->right);
			}
			
			level.push_back(node->val);
		}
		
		res.push_back(level);
	}
	
	return res; 
}
```

### Iterative DFS traversal
```cpp
// Preorder (Root Left Right)
// as we are using stack i.e. LIFO we push right first then left
vector<int> preorderTraversal(TreeNode* root) {
	vector<int> res;
	if (root == nullptr) return res;
	
	stack<TreeNode*> st;
	st.push(root);
	while (!st.empty()) {
		TreeNode* node = st.top();
		st.pop();
		res.push_back(node->val);
		
		if (node->right != nullptr) {
			st.push(node->right);
		}
		if (node->left != nullptr) {
			st.push(node->left);
		}
	}
	
	return res;
}


// Inorder (Left Root Right)
vector<int> inorderTraversal(TreeNode* root) {
	vector<int> res;
	if (root == nullptr) return res;
	
	stack<TreeNode*> st;
	TreeNode* node = root;
	while (true) {
		if (node != nullptr) {
			st.push(node);
			node = node->left;
		} else {
			if (st.empty()) break;
			node = st.top();
			st.pop();
			res.push_back(node->val);
			node = node->right; 
		}
	}
	
	return res;
}
```

### Maximum Depth of Binary tree
```cpp
int max_depth(TreeNode* node) {
	if (node == nullptr) return 0;
	
	int d1 = max_depth(node->left);
	int d2 = max_depth(node->right);
	return max(d1, d2)+1;
}

int maxDepth(TreeNode* root) {
	return max_depth(root);
}
```

### Check for Balanced Binary Tree
```cpp
int max_depth(TreeNode* node) {
	if (node == nullptr) return 0;
	
	int d1 = max_depth(node->left);
	if (d1 == -1) return -1; 
	int d2 = max_depth(node->right);
	if (d2 == -1) return -1;
	
	if (abs(d1-d2) > 1) return -1;
	
	return max(d1, d2)+1;
}

bool isBalanced(TreeNode* root) {
	if (root == nullptr) return true; 
	return max_depth(root)!=-1;
}
```

### Diameter of Binary Tree
```cpp
int fnc (TreeNode* node, int& ans) {
	int lh = 0, rh = 0;
	if (node->left != nullptr) {
		lh = fnc(node->left, ans);
	}
	if (node->right != nullptr) {
		rh = fnc(node->right, ans);
	}
	ans = max(lh+rh+1, ans);
	
	return max(lh+1, rh+1);
}

int diameterOfBinaryTree(TreeNode* root) {
	int ans = INT_MIN;
	int temp = fnc(root, ans);
	return ans-1;
}
```

Maximum Path Sum
```cpp
int fnc (TreeNode* node, int& ans) {
	int lh = 0, rh = 0;
	if (node->left != nullptr) {
		lh = max(lh, fnc(node->left, ans));
	}
	if (node->right != nullptr) {
		rh = max(rh, fnc(node->right, ans));
	}
	ans = max(lh+rh+node->val, ans); 
	
	return max(lh+node->val, rh+node->val);
}
int maxPathSum(TreeNode* root) {
	int ans = INT_MIN;
	int temp = fnc(root, ans);
	return ans;
}
```

### Same Tree
```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
	if (p == nullptr || q == nullptr) {
		return p == q;
	}
	return (p->val == q->val) && isSameTree(p->left, q->left) && isSameTree(p->right, q->right); 
}
```

### Vertical Order Traversal of a Binary Tree
```cpp
vector<vector<int>> verticalTraversal(TreeNode* root) {
	vector<vector<int>> res;
	if (root == nullptr) return res;
	
	queue<pair<TreeNode*, pair<int, int>>> q;
	map<int, map<int, priority_queue<int, vector<int>, greater<int>> >> mp;
	q.push({root, {0, 0}});
	
	while (!q.empty()) {
		int size = q.size();
		
		for (int i=0; i<size; i++) {
			auto [node, cords] = q.front();
			int row = cords.first;
			int col = cords.second;
			q.pop();
			mp[col][row].push(node->val);
			
			if (node->left != nullptr) {
				q.push({node->left, {row+1, col-1}});
			}
			if (node->right != nullptr) {
				q.push({node->right, {row+1, col+1}});
			}
		}
	}
	
	for (auto& [col, temp]: mp) {
		vector<int> vals;
		for (auto& [row, pq]: temp) {
			while (!pq.empty()) {
				vals.push_back(pq.top());
				pq.pop();
			}
		}
		res.push_back(vals);
	}
	
	return res;
}
```

### Binary Tree Right Side View
```cpp
vector<int> rightSideView(TreeNode* root) {
	vector<int> res;
	if (root == nullptr) return res;
	
	queue<TreeNode*> q;
	q.push(root);
	while (!q.empty()) {
		int size = q.size();
		
		for (int i=0; i<size; i++) {
			TreeNode* node = q.front();
			q.pop();
			if (i == size-1) res.push_back(node->val);
			if (node->left) q.push(node->left);
			if (node->right) q.push(node->right);
		}
	}
	
	return res;
}
```

### Symmetric Tree
```cpp
bool isMirror(TreeNode* n1, TreeNode* n2) {
	if (n1 == nullptr && n2 == nullptr) {
		return true;
	}
	if (n1 == nullptr || n2 == nullptr) {
		return false;
	}
	return n1->val == n2->val && isMirror(n1->left, n2->right) && isMirror(n1->right, n2->left);
}

bool isSymmetric(TreeNode* root) {
	return isMirror(root->left, root->right);
}
```

### Lowest Common Ancestor (LCA)
```cpp
bool root_to_node_path(TreeNode* node, int val, vector<TreeNode*>& path) {
	if (node == nullptr) return false;
	
	path.push_back(node);
	if (node->val == val) return true;
	if (root_to_node_path(node->left, val, path) || root_to_node_path(node->right, val, path)) {
		return true;
	}
	
	path.pop_back();
	return false;
}

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
	vector<TreeNode*> v1, v2;
	bool t1 = root_to_node_path(root, p->val, v1);
	bool t2 = root_to_node_path(root, q->val, v2);
	
	TreeNode* res = nullptr;
	int mini = min(v1.size(), v2.size());
	for (int i=0; i<mini; i++) {
		cout << v1[i]->val << " " << v2[i]->val << endl;
		if (v1[i] == v2[i]) {
			res = v1[i];
		}
	}
	
	return res;
}
```

### All Nodes Distance K in Binary Tree
- make a parent child map (creating parent pointers)
- do a dfs till k from target
```cpp
vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
	vector<int> res;
	if (!root) return res;
	
	unordered_map<TreeNode*, TreeNode*> parent;
	queue<TreeNode*> q;
	q.push(root);
	
	while (!q.empty()) {
		TreeNode* node = q.front();
		q.pop();
		if (node->left) {
			parent[node->left] = node;
			q.push(node->left);
		}
		if (node->right) {
			parent[node->right] = node;
			q.push(node->right);
		}
	}
	
	unordered_set<TreeNode*> visited;
	q.push(target);
	visited.insert(target);
	int dist = 0;
	
	while (!q.empty()) {
		int size = q.size();
		if (dist == k) {
			for (int i = 0; i < size; i++) {
				TreeNode* node = q.front();
				q.pop();
				res.push_back(node->val);
			}
			return res;
		}
		for (int i = 0; i < size; i++) {
			TreeNode* node = q.front();
			q.pop();
			if (node->left && !visited.count(node->left)) {
				q.push(node->left);
				visited.insert(node->left);
			}
			if (node->right && !visited.count(node->right)) {
				q.push(node->right);
				visited.insert(node->right);
			}
			if (parent.count(node) && !visited.count(parent[node])) {
				q.push(parent[node]);
				visited.insert(parent[node]);
			}
		}
		dist++;
	}
	
	return res;
}
```

### Count Complete Tree Nodes
```cpp
int countNodes(TreeNode* root) {
	if (root == nullptr) return 0;
	
	int lh = find_left_height(root);
	int rh = find_right_height(root);
	
	if (lh == rh) return (1<<lh) - 1;
	return 1 + countNodes(root->left) + countNodes(root->right);
}

int find_left_height(TreeNode* node) {
	int height = 0;
	while (node) {
		height++;
		node = node->left;
	}
	return height;
}

int find_right_height(TreeNode* node) {
	int height = 0;
	while (node) {
		height++;
		node = node->right;
	}
	return height;
}
```

## Unique Binary Tree
if you are given Preorder and Postorder you can't construct a unique Binary Tree
For Example: 
	Preorder -> 123 
	Postorder -> 321

if we have Inorder and Preorder we can construct a unique Binary Tree out of it 
similarly if we have Inorder and Postorder we can construct a unique Binary Tree of it.

### Construct Binary Tree from Preorder and Inorder Traversal
```cpp
TreeNode* buildsubTree(map<int, int> mp, vector<int>& preorder, int preStart, int preEnd, vector<int>& inorder, int inStart, int inEnd) {
	if (preStart > preEnd || inStart > inEnd) return nullptr;
	
	TreeNode* root = new TreeNode(preorder[preStart]);
	int inRoot = mp[preorder[preStart]];
	int leftCnt = inRoot - inStart;
	
	root->left = buildsubTree(mp, preorder, preStart+1, preStart+leftCnt, inorder, inStart, inRoot-1);
	root->right = buildsubTree(mp, preorder, preStart+leftCnt+1, preEnd, inorder, inRoot+1, inEnd);
		
	return root;
}

TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
	map<int, int> mp;
	for (int i=0; i<inorder.size(); i++) {
		mp[inorder[i]] = i;
	}
	
	return buildsubTree(mp, preorder, 0, preorder.size()-1, inorder, 0, inorder.size()-1);
}
```

### Serialize and Deserialize Binary Tree
```cpp
class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root) return "";
        
        string res;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            if (node) {
                res += to_string(node->val) + ",";
                q.push(node->left);
                q.push(node->right);
            } else {
                res += "#,";
            }
        }
        return res;
    }

    vector<string> parseString(const string& s) {
        vector<string> result;
        stringstream ss(s);
        string token;
        while (getline(ss, token, ',')) {
            result.push_back(token);
        }
        return result;
    }
	
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data.empty()) return nullptr;
		
        vector<string> parsed = parseString(data);
        if (parsed.empty()) return nullptr;
		
        queue<TreeNode*> q;
        TreeNode* root = new TreeNode(stoi(parsed[0]));
        q.push(root);
        int i = 1;
		
        while (!q.empty() && i < parsed.size()) {
            TreeNode* node = q.front();
            q.pop();
			
            // Left child
            if (i < parsed.size() && parsed[i] != "#") {
                node->left = new TreeNode(stoi(parsed[i]));
                q.push(node->left);
            }
            i++;
			
            // Right child
            if (i < parsed.size() && parsed[i] != "#") {
                node->right = new TreeNode(stoi(parsed[i]));
                q.push(node->right);
            }
            i++;
        }
		
        return root;
    }
};
```

## Morris Traversal

Morris Traversal => Threaded Binary Tree
```cpp
// Inorder (Left Root Right)
vector<int> inorderTraversal(TreeNode* root) {
	vector<int> res;
	if (root == nullptr) return res;
	
	TreeNode* curr = root;
	while (curr!=nullptr) {
		// if curr is root without left node
		if (curr->left == nullptr) {
			res.push_back(curr->val);
			curr = curr->right;
		} else {
			// when curr have left node
			TreeNode* prev = curr->left;
			while (prev->right && prev->right != curr) {
				prev = prev->right;
			}
			// if thread already not exist
			if (prev->right == nullptr) {
				// create thread from rightmost of the left subtree to root 
				prev->right = curr;
				// as inorder after root move left
				curr = curr->left;
			} else {
				// when thread already exist and we return back to curr 
				// i.e. prev->right == curr
				
				// cut the thread
				prev->right = nullptr;
				res.push_back(curr->val);
				// as inorder and left is done move right
				curr = curr->right;
			}
			
		}
	}
	
	return res;
}


// Preorder (Root Left Right)
vector<int> preorderTraversal(TreeNode* root) {
	vector<int> res;
	if (root == nullptr) return res;
	
	TreeNode* curr = root;
	while (curr!=nullptr) {
		// if curr is root without left node
		if (curr->left == nullptr) {
			res.push_back(curr->val);
			curr = curr->right;
		} else {
			// when curr have left node
			TreeNode* prev = curr->left;
			while (prev->right && prev->right != curr) {
				prev = prev->right;
			}
			// if thread already not exist
			if (prev->right == nullptr) {
				// create thread from rightmost of the left subtree to root 
				prev->right = curr;
				res.push_back(curr->val);
				// as preorder after root move left
				curr = curr->left;
			} else {
				// when thread already exist and we return back to curr 
				// i.e. prev->right == curr
				
				// cut the thread
				prev->right = nullptr;
				// as preorder and left is done move right
				curr = curr->right;
			}
			
		}
	}
	
	return res;
}
```

### Flatten Binary Tree to Linked List
reverse preorder traversal
```cpp
class Solution {
    TreeNode* prev = nullptr;
public:
    // reverse pre order traversal (Right Left Root)
    void flatten(TreeNode* node) {
        if (node == nullptr) return;
        flatten(node->right);
        flatten(node->left);
	
        node->right = prev;
        node->left = nullptr;
        prev = node;
    }
};
```

# Binary Search Tree (BST)

> **Note:** Inorder of a BST is always in sorted order

### Insert a node in BST (Iterative)
```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
	if (root == nullptr) return new TreeNode(val);
	
	TreeNode* curr = root;
	while (true) {
		if (curr->val < val) {
			if (curr->right) curr = curr->right;
			else {
				curr->right = new TreeNode(val);
				break;
			}
		} else {
			if (curr->left) curr = curr->left;
			else {
				curr->left = new TreeNode(val);
				break;
			} 
		}
	}
	
	return root;
}
```

### Insert a node in BST (Recursive)
- in base case we return the new node and the assign it to `root->left` or `root->right` which ever encountered base case first 
```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
	if(root == nullptr){
		return new TreeNode(val);
	}
	if(root->val > val){
		root->left = insertIntoBST(root->left, val);
	}
	else{
		root->right = insertIntoBST(root->right, val);
	}
	return root;
}
```

### Delete Node in a BST
- start normal iteration according to the value of key
- if key is found then we can replace it with smallest value in the Right Subtree 
	- why ?
	- because smallest value in the right subtree is greater than all values in left and smaller than all values in right
	- therefore maintaining BST property
- after replacing the value we call function with root as right subtree and key as the smallest value in the right subtree
- so when we reach that smallest value we basically reach a leaf node 
	- therefore we trigger `if (!root->left) return root->right;` or `if (!root->right) return root->left;` part of code which return `nullptr`
	- therefore completely removing the key
```cpp
TreeNode* deleteNode(TreeNode* root, int key) {
	if (!root) return nullptr;
	
	if (root->val < key) {
		root->right = deleteNode(root->right, key);
	} else if (root->val > key) {
		root->left = deleteNode(root->left, key);
	} else {
		if (!root->left) return root->right;
		if (!root->right) return root->left;
		
		TreeNode* ln = root->right;
		while (ln->left) {
			ln = ln->left;
		}
		root->val = ln->val;
		root->right = deleteNode(root->right, ln->val);
	}
	
	return root;
}
```

### Kth Smallest Element in BST
inorder traversal gives sorted order of all values
```cpp
void inorder(TreeNode* node, int& k, int& ans) {
	if (node == nullptr) return;
	inorder(node->left, k, ans);
	k--;
	if (k==0) {
		ans = node->val;
		return;
	}
	inorder(node->right, k, ans);
}

int kthSmallest(TreeNode* root, int k) {
	int ans = 0;
	inorder(root, k, ans);
	return ans;
}
```

### Validate Binary Search Tree
```cpp
bool isValidNode(TreeNode* node, long mini, long maxi) {
	if (node == nullptr) return true;
	if (node->val >= maxi || node->val <=mini) return false;
	
	return isValidNode(node->left, mini, node->val) && isValidNode(node->right, node->val, maxi);
}

bool isValidBST(TreeNode* root) {
	return isValidNode(root, LONG_MIN, LONG_MAX);
}
```

### Lowest Common Ancestor of a Binary Search Tree
```cpp
bool root_to_node_path(TreeNode* node, TreeNode* search, vector<TreeNode*>& path) {
	if (node == nullptr) return false;
	
	path.push_back(node);
	if (node->val == search->val) return true;
	if (node->val > search->val) return root_to_node_path(node->left, search, path);
	else return root_to_node_path(node->right, search, path);
	
	path.pop_back();
	return false;
}

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
	vector<TreeNode*> v1, v2;
	bool t1 = root_to_node_path(root, p, v1);
	bool t2 = root_to_node_path(root, q, v2);
	
	TreeNode* res = nullptr;
	int mini = min(v1.size(), v2.size());
	for (int i=0; i<mini; i++) {
		cout << v1[i]->val << " " << v2[i]->val << endl;
		if (v1[i] == v2[i]) {
			res = v1[i];
		}
	}
	
	return res;
}
```

### Construct Binary Search Tree from Preorder Traversal
```cpp
TreeNode* fnc (vector<int>& preorder, int start, int end) {
	if (start>end || end>=preorder.size()) return nullptr;
	
	TreeNode* node = new TreeNode(preorder[start]);
	int j=start+1;
	while (j<preorder.size() && preorder[j] < preorder[start]) {
		j++;
	}
	node->left = fnc (preorder, start+1, j-1);
	node->right = fnc (preorder, j, end); 
	
	return node;
}

TreeNode* bstFromPreorder(vector<int>& preorder) {
	if (preorder.size() == 0) return nullptr;
	
	TreeNode* root = fnc(preorder, 0, preorder.size()-1);
	return root;
}
```

BST Inorder Iterator using stack
```cpp
class BSTIterator {
    stack<TreeNode*> st;
public:
    BSTIterator(TreeNode* root) {
        while (root) {
            st.push(root);
            root = root->left;
        }
    }
    
    int next() {
        TreeNode* node = st.top();
        st.pop();
        TreeNode* curr = node;
        if (curr->right) {
            curr = curr->right;
            while (curr) {
                st.push(curr);
                curr = curr->left;
            }
        }
		
        return node->val;
    }
    
    bool hasNext() {
        return !st.empty();
    }
};
```

### Recover Binary Search Tree
only 2 nodes are swapped
```cpp
class Solution {
    TreeNode* first;
    TreeNode* middle;
    TreeNode* last;
    TreeNode* prev;
public:
    void inorder(TreeNode* root) {
        if (root == nullptr) return;
			
        inorder(root->left);
        if (prev != nullptr && (root->val < prev->val)) {
            if (first == nullptr) {
                first = prev;
                middle = root;
            } else {
                last = root;
            }
        }
        prev = root;
        inorder(root->right);
    }
	
    void recoverTree(TreeNode* root) {
        first = middle = last = nullptr;
        prev = new TreeNode(INT_MIN);
        inorder(root);
        
        if (first && last) swap(first->val, last->val);
        else if (first && middle) swap(first->val, middle->val);
    }
};
```


# Graphs

Graphs(V, E) =>   V -> vertices and E -> edges
A Graph can be Directed or Undirected and cyclic or acyclic

- **Path:** contains a lot of vertices and each of them are reachable, a vertex cannot appear twice in a path. 
- **Degree of a vertex:** number of edges that go inside or outside of a vertex.
	- **Indegree of vertex:** number of edges that go inside of a vertex.
	- **Outdegree of vertex:** number of edges that go outside of a vertex.
- **Connected Components:** if there exist a path between any two vertices of that graph

**Properties:**
- Total degree of a graph = 2 * Number of edges.

**Representation:**
- Adjacency Matrix (Space -> O(N^2))
	```cpp
	int n, m;
	cin >> n >> m;
	int adj[n+1][m+1];
	for (int i=0; i<m; i++) {
		int u, v;
		cin >> u >> v;
		adj[u][v] = 1;
		adj[v][u] = 1;
	}
	```
	For weighted graphs instead of `1` store the weight
	
- Adjacency List (Space -> O(2 * E))
	```cpp
	int n, m;
	cin >> n >> m;
	vector<int> adj[n+1];
	for (int i=0; i<m; i++) {
		int u, v;
		cin >> u >> v;
		adj[u].push_back(v);
		adj[v].push_back(u);
	}
	```
	For weighted graphs store a pair for all n vertices `vector<pair<int, int>> adj[n+1]` 


## Traversals
For all traversals we need to now about number of connected components
so we keep a visited array 
This way we can keep track that vertices are visited therefore visiting all connected commponents
```cpp
vector<bool> visited(n+1, false);
for (int i=1; i<n+1; i++) {
	if (!visited[i]) {
		traversal(i, visited);
	}
}
```

### Breadth First Search(BFS) - Iterative
```cpp
vector<int> bfsOfGraph(int v, int start, vector<int> adj[]) {
	vector<int> bfs;
	
	vector<bool> visited(v, false);
	visited[start] = true;
	
	queue<int> q;
	q.push(start);
	while (!q.empty()) {
		int vertex = q.front();
		q.pop();
		bfs.push_back(vertex);
		
		for (int it: adj[vertex]){
			if (!visited[it]) {
				visited[it] = true;
				q.push(it);
			}
		}
	}
	
	return bfs;
}
```

### Depth First Search(DFS) - Iterative
```cpp
vector<int> dfsOfGraph(int v, int start, vector<int> adj[]) {
	vector<int> dfs;
	
	vector<bool> visited(v, false);
	visited[start] = true;
	
	stack<int> st;
	st.push(start);
	while (!st.empty()) {
		int vertex = st.top();
		st.pop();
		dfs.push_back(vertex);
		
		// traverse in reverse order to match the recursive solution 
		for (auto it = adj[vertex].rbegin(); it != adj[vertex].rend(); ++it) {
			if (!visited[*it]) {
				visited[*it] = true;
				st.push(*it);
			}
		}
	}
	
	return dfs;
}
```

### Depth First Search(DFS) - Recursive
```cpp
void dfsRecursive(int vertex, vector<int> adj[], vector<bool>& visited, vector<int>& dfs) {
    visited[vertex] = true;
    dfs.push_back(vertex);
    for (int neighbor : adj[vertex]) {
        if (!visited[neighbor]) {
            dfsRecursive(neighbor, adj, visited, dfs);
        }
    }
}

vector<int> dfsOfGraph(int v, int start, vector<int> adj[]) {
    vector<int> dfs;
    vector<bool> visited(v, false);
    
    dfsRecursive(start, adj, visited, dfs);
    return dfs;
}
```

### Number of Provinces
- equivalent to find number of connected components
```cpp
int findCircleNum(vector<vector<int>>& isConnected) {
	int n = isConnected.size();
	vector<bool> visited(n, false);
	
	int provinces = 0;
	queue<int> q;
	for (int i=0; i<n; i++) {
		if (!visited[i]) {
			q.push(i);
			visited[i] = true;
			while (!q.empty()) {
				int vertex = q.front();
				q.pop();
				for (int i=0; i<isConnected[vertex].size(); i++){
					if (isConnected[vertex][i] == 1 && !visited[i]) {
						visited[i] = true;
						q.push(i);
					}
				}
			}
			provinces++;
		} 
	}
	
	return provinces;
}
```

### Rotten Oranges
- start bfs from all rotten locations at same time
```cpp
int orangesRotting(vector<vector<int>>& grid) {
	int m = grid.size();
	int n = grid[0].size();
	vector<vector<bool>> visited(m, vector<bool>(n, false));
	
	queue<pair<int, int>> q;
	int fresh = 0;
	for (int row = 0; row < m; row++) {
		for (int col = 0; col < n; col++) {
			if (grid[row][col] == 2) {
				q.push({row, col});
				visited[row][col] = true;
			} else if (grid[row][col] == 1) {
				fresh++;
			}
		}
	}
	if (fresh == 0) return 0;
	
	int time = 0;
	while (!q.empty()) {
		int size = q.size();
		bool rotted = false;
		
		for (int i=0; i<size; i++) {
			auto [row, col] = q.front();
			q.pop();
			if (0 <= row+1 && row+1 < m && !visited[row+1][col] && grid[row+1][col] == 1) {
				visited[row+1][col] = true;
				q.push({row+1, col});
				fresh--;
				rotted = true;
			}
			if (0 <= row-1 && row-1 < m && !visited[row-1][col] && grid[row-1][col] == 1) {
				visited[row-1][col] = true;
				q.push({row-1, col});
				fresh--;
				rotted = true;
			}
			if (0 <= col+1 && col+1 < n && !visited[row][col+1] && grid[row][col+1] == 1) {
				visited[row][col+1] = true;
				q.push({row, col+1});
				fresh--;
				rotted = true;
			}
			if (0 <= col-1 && col-1 < n && !visited[row][col-1] && grid[row][col-1] == 1) {
				visited[row][col-1] = true;
				q.push({row, col-1});
				fresh--;
				rotted = true;
			} 
		}
		
		if(rotted) time++;
	}
	
	return fresh == 0 ? time : -1;
}
```

### Cycle Detection in undirected Graph (DFS)
Without tracking the parent, visiting a neighbor that is the parent node could be mistaken for a cycle.
```cpp
bool hasCycle(vector<vector<int>>& graph) {
    int n = graph.size();
    unordered_set<int> visited;
    stack<pair<int, int>> stk; // Pair of (node, parent)
	
    for (int start = 0; start < n; ++start) { // Handle disconnected components
        if (visited.find(start) != visited.end()) continue;
        
        stk.push({start, -1}); // Start node has no parent
        visited.insert(start);
		
        while (!stk.empty()) {
            auto [node, parent] = stk.top();
            stk.pop();
			
            for (int neighbor : graph[node]) {
                if (neighbor == parent) continue; // Skip the parent node
                if (visited.find(neighbor) != visited.end()) {
                    return true; // Cycle detected (non-parent visited node)
                }
                visited.insert(neighbor);
                stk.push({neighbor, node});
            }
        }
    }
    return false; // No cycle found
}
```

### Distance of Nearest Cell having 0
```cpp
vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
	int m = mat.size(), n = mat[0].size();
	vector<vector<int>> dist(m, vector<int>(n, -1));
	queue<pair<int, int>> q;
	
	for (int i = 0; i < m; ++i) {
		for (int j = 0; j < n; ++j) {
			if (mat[i][j] == 0) {
				q.push({i, j});
				dist[i][j] = 0;
			}
		}
	}
	
	vector<pair<int, int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
	
	while (!q.empty()) {
		auto [row, col] = q.front();
		q.pop();
		
		for (auto [dr, dc] : dirs) {
			int newRow = row + dr, newCol = col + dc;
			if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n && dist[newRow][newCol] == -1) {
				dist[newRow][newCol] = dist[row][col] + 1;
				q.push({newRow, newCol});
			}
		}
	}
	
	return dist;
}
```

### Surrounded Regions
```cpp
void bfs(vector<vector<char>>& board, int sr, int sc, int m, int n) {
	queue<pair<int, int>> q;
	vector<pair<int, int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
	
	board[sr][sc] = 'm';
	q.push({sr,sc});
	
	while (!q.empty()) {
		int size = q.size();
		
		for (int i=0; i<size; i++) {
			auto [row, col] = q.front();
			q.pop();
			
			for (int i=0; i<4; i++) {
				int newRow = row + directions[i].first;
				int newCol = col + directions[i].second;
				
				if (newRow >= 0 && newRow < m &&
					newCol >= 0 && newCol < n &&
					board[newRow][newCol] == 'O') {
						board[newRow][newCol] = 'm';
						q.push({newRow, newCol});
					}
			}
		}
	
	}
}

void solve(vector<vector<char>>& board) {
	int m = board.size();
	int n = board[0].size();
	
	for (int j = 0; j < n; j++) {
		if (board[0][j] == 'O') bfs(board, 0, j, m, n); 
		if (board[m-1][j] == 'O') bfs(board, m-1, j, m, n);
	}
	
	for (int i = 0; i < m; i++) {
		if (board[i][0] == 'O') bfs(board, i, 0, m, n);
		if (board[i][n-1] == 'O') bfs(board, i, n-1, m, n);
	}
	
	for (int i=0; i<m; i++) {
		for (int j=0; j<n; j++) {
			if (board[i][j] == 'O') {
				board[i][j] = 'X';
			} else if (board[i][j] == 'm') {
				board[i][j] = 'O';
			}
		}
	}
}
```

### Word Ladder
- we replace each character of startWord and form new words that exist in wordList
- we make(don't store it just generate next possible words in bfs traversal) a tree from startWord to endWord and keep track of levels
```cpp
int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
	unordered_set<string> s;
	for (string i: wordList) {
		s.insert(i);
	}
	
	int cnt = 1;
	queue<pair<string, int>> q;   // pair of word, level
	q.push({beginWord, 1});
	while (!q.empty()) {
		int size = q.size();
		
		for (int i=0; i<size; i++) {
			auto [str, lvl] = q.front();
			q.pop();
			
			if (str == endWord) {
				return lvl;
			}
			
			for (int i=0; i<str.length(); i++) {
				char ch = str[i];
				for (char c='a'; c<='z'; c++) {
					str[i] = c;
					if (s.find(str) != s.end()) {
						s.erase(str);
						q.push({str, lvl+1});
					}
				}
				str[i] = ch;
			}
		}
	}
	
	return 0;
}
```

## Is Graph Bipartite? (using coloring)
if a graph has odd length of loop then it can't be bipartite
```cpp
bool isBipartite(vector<vector<int>>& graph) {
	int n = graph.size();
	vector<int> color(n, -1);
	
	for (int start = 0; start < n; ++start) {    // for disconnected components
		if (color[start] != -1) continue;
		
		color[start] = 0;
		stack<pair<int, int>> st;
		st.push({start, 0});
		while (!st.empty()) {
			auto [node, co] = st.top();
			st.pop();
			
			for (int i=0; i<graph[node].size(); i++) {
				if (color[graph[node][i]] == -1) {
					if (co == 1) color[graph[node][i]] = 0;
					else if (co == 0) color[graph[node][i]] = 1;
					st.push({graph[node][i], color[graph[node][i]]});
				} else if (color[graph[node][i]] == co) {
					return false;
				}
			}
		}
	}
	
	return true;
}
```

### Is Cyclic DAG ?
- three colors => 0 = unvisited, 1 = visiting, 2 = visited
- it's cyclic if we visit a node of same path which is represented by 1 i.e. currently visiting
```cpp
bool is_cyclic_dag(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> visited(n, 0); // 0 = unvisited, 1 = visiting, 2 = visited
	
    for (int i = 0; i < n; i++) {
        if (visited[i] != 0) continue;
        
        stack<pair<int, bool>> st; // {node, isExiting}
        st.push({i, false}); // entering node
        
        while (!st.empty()) {
            auto [node, isExiting] = st.top();
            st.pop();
            
            if (isExiting) {
                visited[node] = 2; // fully processed
                continue;
            }
            
            if (visited[node] == 1) {
                // Back edge found — cycle detected
                return true;
            }
            
            if (visited[node] == 0) {
                visited[node] = 1; // mark as visiting
                st.push({node, true}); // post-visit step
                
                for (int neighbor : graph[node]) {
                    if (visited[neighbor] != 2) {
                        st.push({neighbor, false}); // pre-visit step
                    }
                }
            }
        }
    }
	
    return false; // no cycles
}
```

## Topo Sort (using DFS)
- only applicable on DAG
- directed graph should appear in it's linear ordering (if u->v then u should come before v)
```cpp
vector<int> toposort(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<bool> visited(n, false);
    stack<int> ans; // Stack for topological order
	
    for (int i = 0; i < n; ++i) {
        if (visited[i]) continue;
        
	    stack<int> st; // Stack for DFS traversal
        st.push(i); // Start DFS from unvisited node
        // here we won't mark i as visited as we haven't pushed it's neighbors in stack
        while (!st.empty()) {
            int node = st.top();
            st.pop();
            
            if (!visited[node]) {
                visited[node] = true;
                st.push(node); // Re-push node for post-order processing
                
                // Push unvisited neighbors
                for (int neighbor : graph[node]) {
                    if (!visited[neighbor]) {
                        st.push(neighbor);
                    }
                }
            } else {
                ans.push(node); // Add to result stack after processing neighbors
            }
        }
    }
	
    // Transfer from ans stack to result vector
    vector<int> result;
    while (!ans.empty()) {
        result.push_back(ans.top());
        ans.pop();
    }
    
    return result;
}
```

### Topo Sort (using BFS called Kahn's algo)
repeatedly remove nodes without any dependencies from the graph
- we keep track of in-degree of all node.
- then in bfs decrease degree of all neighbors
- if at any point we can't find a node with degree 0 though we have nodes with degree > 0 then it's because of cycle
```cpp
vector<int> toposort(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> res;
    vector<int> degree(n, 0);
    
    // find in-degree of all nodes
    for (auto i: graph) {
        for (auto j: i) {
            degree[j]++;
        }
    }
    
    // enter all 0 degree nodes i.e. nodes that have no dependencies initially
    queue<int> q;
    for (int i=0; i<n; i++) {
        if (degree[i] == 0) q.push(i);
    }
    
    // BFS: Process level by level
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        res.push_back(node);
		
        for (int neighbor : graph[node]) {
            degree[neighbor]--;
            if (degree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }
    
    // Check for cycle
    if (res.size() != n) return {};
    
	return res;
}
```

## Shortest Path
- For Undirected, Unweighted graph
	- -> bfs (O(E+V))
- For Undirected, Weighted (+ve only) graph
	- -> dijkstra
- For Undirected, Weighted (both +ve and -ve) graph 
	- -> bellman ford
- For Unweighted DAG 
	- -> bfs
- For Weighted (both +ve and -ve) DAG
	- -> topo sort + relaxation

**Single Source Shortest Path Algos**:
- Dijkstra => O((E+V) * log(V)) => pick the path with the shortest distance 
- Bellman Ford => O(EV) => relax edges v-1 times, if in v'th relaxation any change in shortest path occur then graph have negative cycle/s

**All Pairs Shortest Path Algos**:
- Floyd Warshal => O(V^3) => use an 3D-DP array to store the shortest path from `i` to `j` using `k` node

### Shortest Path in Weighted DAG (Topo Sort)
```cpp
vector<int> toposort(vector<vector<pair<int, int>>>& graph) {
    int n = graph.size();
    vector<int> res;
    vector<int> degree(n, 0);
    
    // find in-degree of all nodes
    for (auto i: graph) {
        for (auto j: i) {
            degree[j.first]++;
        }
    }
    
    // enter all 0 degree nodes i.e. nodes that have no dependencies initially
    queue<int> q;
    for (int i=0; i<n; i++) {
        if (degree[i] == 0) q.push(i);
    }
    
    // BFS: Process level by level
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        res.push_back(node);
		
        for (auto neighbor : graph[node]) {
            degree[neighbor.first]--;
            if (degree[neighbor.first] == 0) {
                q.push(neighbor.first);
            }
        }
    }
    
    // Check for cycle
    if (res.size() != n) return {};
    
	return res;
}

vector<int> shortest_path_dag(int start, int v, int e, vector<vector<int>>& edges) {
    vector<vector<pair<int, int>>> graph(v, vector<pair<int, int>>());
    for (int i=0; i<e; i++) {
        int u = edges[i][0];
        int v = edges[i][1];
        int w = edges[i][2];
        graph[u].push_back({v, w});
    }
    
    vector<int> temp = toposort(graph);
    vector<int> dist(v, INT_MAX);
    dist[start] = 0;
    
    for (int i=0; i<temp.size(); i++) {
        if (dist[temp[i]] == INT_MAX) continue;
        for (auto neighbor: graph[temp[i]]) {
            dist[neighbor.first] = min(dist[neighbor.first], dist[temp[i]] + neighbor.second);
        }
    }
    
    return dist;
}

int main()
{
    int v = 6, e = 7;
    vector<vector<int>> edges= {{0,1,2},{0,4,1},{4,5,4},{4,2,2},{1,2,3},{2,3,6},{5,3,1}};
	
	vector<int> res = shortest_path_dag(0, v, e, edges);
	for (int i: res) {
	    cout << i << " ";
	}
	cout << endl;
	
	return 0;
}
```

### Shortest Path in Weighted (+ve only) graph (Djikstra)
Dijkstra => pick the path with the shortest distance 
```cpp
vector<int> djisktra (int v, int start, vector<vector<pair<int, int>>>& graph) {
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq; // node index, distance from start
    pq.push({start, 0});
     
    vector<int> dist(v, INT_MAX);
    dist[start] = 0;
    
    while (!pq.empty()) {
        auto node = pq.top();
        pq.pop();
        
        for (auto neighbor: graph[node.first]) {
            int new_dist = node.second + neighbor.second;
            if (new_dist < dist[neighbor.first]) {
                dist[neighbor.first] = node.second + neighbor.second; 
                pq.push({neighbor.first, dist[neighbor.first]});
            }
        }
    }
    
    return dist;
}

vector<int> shortest_path_pos(int start, int v, int e, vector<vector<int>>& edges) {
    vector<vector<pair<int, int>>> graph(v, vector<pair<int, int>>());
    for (int i=0; i<e; i++) {
        int u = edges[i][0];
        int v = edges[i][1];
        int w = edges[i][2];
        graph[u].push_back({v, w});
    }
    
    return djisktra(v, start, graph);
}

int main()
{
    int v = 6, e = 7;
    vector<vector<int>> edges= {{0,1,2},{0,4,1},{4,5,4},{4,2,2},{1,2,3},{2,3,6},{5,3,1}};
	
	vector<int> res = shortest_path_pos(0, v, e, edges);
	for (int i: res) {
	    cout << i << " ";
	}
	cout << endl;
	
	return 0;
}
```

### Cheapest Flights Within K Stops
```cpp
int dijkstra(int v, int src, int dst, int k, vector<vector<pair<int, int>>>& graph) {
	// {price, city, stops}
	priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> pq;
	pq.push({0, src, 0}); // {price, city, stops}
	
	vector<vector<int>> dist(v, vector<int>(k + 2, INT_MAX)); // Track dist per city and stops
	dist[src][0] = 0;
	
	while (!pq.empty()) {
		auto [price, city, stops] = pq.top();
		pq.pop();
		
		if (city == dst) return price; // Return as soon as destination is reached
		
		if (stops <= k) { // Allow up to k stops (k+1 edges)
			for (auto [new_city, travel_price] : graph[city]) {
				int new_price = price + travel_price;
				if (new_price < dist[new_city][stops + 1]) {
					dist[new_city][stops + 1] = new_price;
					pq.push({new_price, new_city, stops + 1});
				}
			}
		}
	}
	
	return dist[dst][k + 1] == INT_MAX ? -1 : dist[dst][k + 1];
}

int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
	vector<vector<pair<int, int>>> graph(n);
	for (int i = 0; i < flights.size(); i++) {
		int u = flights[i][0];
		int v = flights[i][1];
		int w = flights[i][2];
		graph[u].push_back({v, w});
	}
	
	return dijkstra(n, src, dst, k, graph);
}
```

### Number of ways to arrive at destination
```cpp
int djikstra(int n, int src, int dst, vector<vector<pair<int, int>>>& graph) {
	const long long mod_val = 1e9 + 7;
	priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<pair<long long, int>>> pq;
	pq.push({0, src});
	
	vector<pair<long long, long long>> min_times(n, {LLONG_MAX, 0});
	min_times[src] = {0, 1};
	
	while (!pq.empty()) {
		auto [time, node] = pq.top();
		pq.pop();
		
		if (time > min_times[node].first) continue;
		
		for (auto [neighbor, time_taken]: graph[node]) {
			long long new_time_taken = time_taken + time;
			if (new_time_taken < min_times[neighbor].first) {
				min_times[neighbor] = {new_time_taken, min_times[node].second};
				pq.push({new_time_taken, neighbor});
			} else if (new_time_taken == min_times[neighbor].first) {
				min_times[neighbor].second = (min_times[neighbor].second + min_times[node].second) % mod_val;
			} 
		}
	}
	
	return min_times[dst].first == LLONG_MAX ? 0 : min_times[dst].second % mod_val;
}

int countPaths(int n, vector<vector<int>>& roads) {
	 vector<vector<pair<int, int>>> graph(n);
	for (int i = 0; i < roads.size(); i++) {
		int u = roads[i][0];
		int v = roads[i][1];
		int w = roads[i][2];
		graph[u].push_back({v, w});
		graph[v].push_back({u, w});
	}
	
	return djikstra(n, 0, n-1, graph);
}
```

### Bellman Ford Algorithm
```cpp
vector<int> bellman_ford(int start, int v, vector<vector<int>>& edges) {
    vector<int> dist(v, 1e8);
    dist[start] = 0;
    
    for (int i=0; i<v-1; i++) {
        for (auto it: edges) {
            int u = it[0];
            int v = it[1];
            int w = it[2];
            if (dist[u] != 1e8 && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
            }
        }
    }
    
    // nth relaxation to check for negative cycle 
    for (auto it: edges) {
        int u = it[0];
        int v = it[1];
        int w = it[2];
        if (dist[u] != 1e8 && dist[u] + w < dist[v]) return {-1};
    }
    
    return dist;
}
```

### Floyd Warshal Algorithm
```cpp
void floyd_warshal(vector<vector<int>>&matrix) {
	int n = matrix.size();
	// initalize
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			// if not reachable i.e. -1 then assign infinity
			if (matrix[i][j] == -1) {
				matrix[i][j] = 1e9;
			}
			if (i == j) matrix[i][j] = 0;
		}
	}
	
	// find shortest via all k's
	for (int k = 0; k < n; k++) {
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j]);
			}
		}
	}
	
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			// reverse the infinity assigned to unreachable nodes
			if (matrix[i][j] == 1e9) {
				matrix[i][j] = -1;
			}
		}
	}
}
```

### Find the City With the Smallest Number of Neighbors at a Threshold Distance
```cpp
void floyd_warshal(vector<vector<int>>&matrix) {
	int n = matrix.size();
	// initalize
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			// if not reachable i.e. -1 then assign infinity
			if (matrix[i][j] == -1) {
				matrix[i][j] = 1e9;
			}
			if (i == j) matrix[i][j] = 0;
		}
	}
	
	// find shortest via all k's
	for (int k = 0; k < n; k++) {
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j]);
			}
		}
	}
	
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < n; j++) {
			// reverse the infinity assigned to unreachable nodes
			if (matrix[i][j] == 1e9) {
				matrix[i][j] = -1;
			}
		}
	}
}

int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
	vector<vector<int>> dist(n, vector<int> (n, -1));
	for (auto it: edges) {
		int u = it[0];
		int v = it[1];
		int w = it[2];
		dist[u][v] = w;
		dist[v][u] = w;
	}
	
	floyd_warshal(dist);
	
	int neighbor_cities = INT_MAX, res = 0;
	for (int i=0; i<n; i++) {
		int cnt = 0;
		for (int j=0; j<n; j++) {
			if (i!=j && dist[i][j] <= distanceThreshold) cnt++; 
		}
		if (cnt <= neighbor_cities) {
			neighbor_cities = cnt;
			res = i;
		}
	}
	
	return res;
}
```

## Minimum Spanning Tree (MST)
Spanning Tree => A tree with `n` nodes, `n-1` edges and all nodes are reachable from other nodes
Minimum Spanning Tree => Spanning Tree in a graph with minimum sum of edge weights

### Prim's Algorithm
```cpp
vector<pair<int, int>> prims_mst(int v, vector<vector<vector<int>>>& graph) {
    priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>> pq;
    vector<int> visited(v, 0);
    vector<pair<int, int>> mst;
    
    pq.push({0, 0, -1}); // weight, node, parent
    int mst_sum = 0;
	
	// between parent to node greedily pick the least weight
    while (!pq.empty()) {
        auto it = pq.top();
        pq.pop();
        
        int wt = it[0];
        int node = it[1];
        int parent = it[2];
        
        if (visited[node]) continue;
        visited[node] = 1;
        
        if (parent != -1) {
            mst.push_back({parent, node});
        }
        mst_sum += wt;
        
        for (auto jt : graph[node]) {
            int neighbor = jt[0];
            int ewt = jt[1];
            if (!visited[neighbor]) {
                pq.push({ewt, neighbor, node});
            }
        }
    }
    
    cout << "MST sum: " << mst_sum << endl;
    return mst;
}

int main() {
    int V = 5;
    vector<vector<int>> edges = {
        {0, 1, 2}, {0, 2, 1}, {1, 2, 1},
        {2, 3, 2}, {3, 4, 1}, {4, 2, 2}
    };
	
    vector<vector<vector<int>>> adj(V);
    for (auto it : edges) {
        int u = it[0], v = it[1], w = it[2];
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    }
	
    vector<pair<int, int>> res = prims_mst(V, adj);
    for (auto [u, v] : res) {
        cout << u << " " << v << endl;
    }
	
    return 0;
}
```

## Disjoint Set / Union Find
used when array/graph is dynamic 

It gives us two options:
- finding ultimate parent ~ alpha(n) ~ O(1)
- union ~ alpha(n) ~ O(1) => add new value to array/graph
	- union by rank
	- union by size

```cpp
class DisjointSet {
    vector<int> rank, union_size, parent;

public:
    DisjointSet(int n) {
        rank.resize(n+1, 0);
        union_size.resize(n+1, 1);
        parent.resize(n+1);
        for (int i=0; i<=n; i++) {
            parent[i] = i;
        }
    }
    
    int find_ultimate_parent(int node) {
        if (node == parent) return node;
        // path compression
        return parent[node] = find_ultimate_parent(parent[node]);
    }
    
    void union_by_rank(int u, int v) {
        int ultimate_parent_u = find_ultimate_parent(u);
        int ultimate_parent_v = find_ultimate_parent(v);
        
        if (ultimate_parent_u == ultimate_parent_v) return;
        
        if (rank[ultimate_parent_u] < rank[ultimate_parent_v]) {
            parent[ultimate_parent_u] = ultimate_parent_v;
        } else if (rank[ultimate_parent_u] > rank[ultimate_parent_v]) {
            parent[ultimate_parent_v] = ultimate_parent_u;
        } else {
            parent[ultimate_parent_v] = ultimate_parent_u;
            rank[ultimate_parent_u]++;
        }
    }
    
    void union_by_size(int u, int v) {
        int ultimate_parent_u = find_ultimate_parent(u);
        int ultimate_parent_v = find_ultimate_parent(v);
        
        if (ultimate_parent_u == ultimate_parent_v) return;
        
        if (union_size[ultimate_parent_u] < union_size[ultimate_parent_v]) {
            parent[ultimate_parent_u] = ultimate_parent_v;
            union_size[ultimate_parent_v] += union_size[ultimate_parent_u];
        } else {
            parent[ultimate_parent_v] = ultimate_parent_u;
            union_size[ultimate_parent_u] += union_size[ultimate_parent_v];
        }
    }
};
```

### Kruskal's Algorithm
```cpp
class DisjointSet {
    vector<int> union_size, parent;
public:
    DisjointSet(int n) {
        union_size.resize(n+1, 1);
        parent.resize(n+1);
        for (int i=0; i<=n; i++) {
            parent[i] = i;
        }
    }
    
    int find_ultimate_parent(int node) {
        if (node == parent[node]) return node;
        // path compression
        return parent[node] = find_ultimate_parent(parent[node]);
    }
    
    void union_by_size(int u, int v) {
        int ultimate_parent_u = find_ultimate_parent(u);
        int ultimate_parent_v = find_ultimate_parent(v);
        
        if (ultimate_parent_u == ultimate_parent_v) return;
        
        if (union_size[ultimate_parent_u] < union_size[ultimate_parent_v]) {
            parent[ultimate_parent_u] = ultimate_parent_v;
            union_size[ultimate_parent_v] += union_size[ultimate_parent_u];
        } else {
            parent[ultimate_parent_v] = ultimate_parent_u;
            union_size[ultimate_parent_u] += union_size[ultimate_parent_v];
        }
    }
};

int kruskal_mst(int v, vector<pair<int, pair<int, int>>>& edges) {
    // edges => {weight, {u, v}}
    
    sort(edges.begin(), edges.end());
    
    DisjointSet ds(v);
    int mst_sum = 0;
    for (auto it: edges) {
        int wt = it.first;
        int u = it.second.first;
        int v = it.second.second;
        
        if (ds.find_ultimate_parent(u) != ds.find_ultimate_parent(v)) {
            mst_sum += wt;
            ds.union_by_size(u, v);
        }
    }
    
    return mst_sum;
}
```

### Number of Operations to Make Network Connected
```cpp
class DisjointSet {
	vector<int> union_size, parent;
public:
	DisjointSet(int n) {
		union_size.resize(n+1, 1);
		parent.resize(n+1);
		for (int i=0; i<=n; i++) {
			parent[i] = i;
		}
	}
	
	int find_ultimate_parent(int node) {
		if (node == parent[node]) return node;
		// path compression
		return parent[node] = find_ultimate_parent(parent[node]);
	}
	
	void union_by_size(int u, int v) {
		int ultimate_parent_u = find_ultimate_parent(u);
		int ultimate_parent_v = find_ultimate_parent(v);
		
		if (ultimate_parent_u == ultimate_parent_v) return;
		
		if (union_size[ultimate_parent_u] < union_size[ultimate_parent_v]) {
			parent[ultimate_parent_u] = ultimate_parent_v;
			union_size[ultimate_parent_v] += union_size[ultimate_parent_u];
		} else {
			parent[ultimate_parent_v] = ultimate_parent_u;
			union_size[ultimate_parent_u] += union_size[ultimate_parent_v];
		}
	}
};

int makeConnected(int n, vector<vector<int>>& connections) {
	DisjointSet ds(n);
	int cnt = 0; // Count redundant edges
	for (auto& it : connections) {
		int u = it[0];
		int v = it[1];
		if (ds.find_ultimate_parent(u) == ds.find_ultimate_parent(v)) {
			cnt++;
		} else {
			ds.union_by_size(u, v);
		}
	}
	
	// Count components
	unordered_set<int> components;
	for (int i = 0; i < n; i++) {
		components.insert(ds.find_ultimate_parent(i));
	}
	int num_components = components.size();
	
	// Need (num_components - 1) edges to connect all components
	if (cnt >= num_components - 1) {
		return num_components - 1;
	}
	return -1; // Not enough edges to connect
}
```

### Most Stones Removed with Same Row or Column
```cpp
class DisjointSet {
	vector<int> union_size, parent;
	public: 
		DisjointSet(int n) {
			union_size.resize(n+1, 1);
			parent.resize(n+1);
			for (int i=0; i<n+1; i++) parent[i]=i;
		}
		
		int find_ultimate_parent(int node) {
			if (parent[node] == node) return node;
			return parent[node] = find_ultimate_parent(parent[node]);
		}
		
		void union_by_size(int u, int v) {
			int ultimate_parent_u = find_ultimate_parent(u);
			int ultimate_parent_v = find_ultimate_parent(v);
			
			if (ultimate_parent_u == ultimate_parent_v) return;
			
			if (union_size[ultimate_parent_u] < union_size[ultimate_parent_v]) {
				parent[ultimate_parent_u] = ultimate_parent_v;
				union_size[ultimate_parent_v] += union_size[ultimate_parent_u];
			} else {
				parent[ultimate_parent_v] = ultimate_parent_u;
				union_size[ultimate_parent_u] += union_size[ultimate_parent_v];
			}
		}
};

int removeStones(vector<vector<int>>& stones) {
	// we can use dfs on each stone to find all connected stones
	// then from each of connected stone component remove all except one stone
	// therefore the formula boils down to (no of stones - no of connected components)
	
	// but we will use DSU to group them
	// but DSU have a 1D array so we will convert columns
	// rows number from 0 to no_of_rows then columns are (column_number + no_of_rows + 1)   
	int no_of_rows = 0;
	int no_of_cols = 0;
	
	for (auto it: stones) {
		no_of_rows = max(no_of_rows, it[0]);
		no_of_cols = max(no_of_cols, it[1]);
	}
	
	DisjointSet ds(no_of_rows + no_of_cols + 1);
	
	// connect stones columns and rows to represent a connected component
	for (auto it: stones) {
		int stone_row = it[0];
		int stone_col = it[1] + no_of_rows + 1;
		ds.union_by_size(stone_row, stone_col);
	}
	
	// no of unique ultimate parents of stone  == no of components
	unordered_set<int> s;
	for (auto it: stones) {
		int stone_row = it[0];
		s.insert(ds.find_ultimate_parent(stone_row));
	}
	
	return stones.size() - s.size();
}
```

### Making A Large Island
```cpp
class DSU{
	public: 
		vector<int> union_size, parent;
		DSU(int n) {
			union_size.resize(n+1, 1);
			parent.resize(n+1);
			for (int i=0; i<n+1; i++) parent[i] = i;
		}
		
		int find_ultimate_parent(int node) {
			if (parent[node] == node) return node;
			return parent[node] = find_ultimate_parent(parent[node]);
		}
		
		void union_by_size(int u, int v) {
			int ultimate_parent_u = find_ultimate_parent(u);
			int ultimate_parent_v = find_ultimate_parent(v);
			if (ultimate_parent_u == ultimate_parent_v) return;
			
			if (ultimate_parent_u < ultimate_parent_v) {
				parent[ultimate_parent_u] = ultimate_parent_v;
				union_size[ultimate_parent_v] += union_size[ultimate_parent_u]; 
			} else {
				parent[ultimate_parent_v] = ultimate_parent_u;
				union_size[ultimate_parent_u] += union_size[ultimate_parent_v];
			}
		}
};

int largestIsland(vector<vector<int>>& grid) {
	int n = grid.size();
	vector<pair<int, int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
	vector<pair<int, int>> zeros;
	DSU ds(n*n);
	
	for (int i=0; i<n; ++i) {
		for (int j=0; j<n; j++) {
			if (grid[i][j] == 0) {
				zeros.push_back({i, j});
				continue;
			}
			for (auto it: directions) {
				int newRow = it.first+i;
				int newCol = it.second+j;
				if (newRow >= 0 && newRow < n &&
					newCol >= 0 && newCol < n &&
					grid[newRow][newCol] == 1) {
						int nodeNo = i * n + j;
						int adjNodeNo = newRow * n + newCol;
						ds.union_by_size(nodeNo, adjNodeNo);
				}
			}
		}
	}
	
	// entire grid is of one's
	bool allOnes = zeros.empty();
	if (allOnes) return n * n;
	
	int res = 0;
	for (auto it: zeros) {
		int row = it.first;
		int col = it.second;
		set<int> components;
		for (auto it: directions) {
			int newRow = it.first+row;
			int newCol = it.second+col;
			if (newRow >= 0 && newRow < n &&
				newCol >= 0 && newCol < n &&
				grid[newRow][newCol] == 1) {
					components.insert(ds.find_ultimate_parent(newRow*n+newCol));
			}
		}
		int total_size = 0;
		for (auto it: components) {
			total_size += ds.union_size[it];
		}
		res = max(res, total_size+1);
	}
	
	return res;
}
```

### Swim in Rising Water
```cpp
int swimInWater(vector<vector<int>>& grid) {
	int n = grid.size();
	vector<vector<int>> directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
	vector<vector<bool>> visited(n, vector<bool>(n, false));
	priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<>> pq;
	
	pq.push({grid[0][0], {0, 0}});
	visited[0][0] = true;
	int maxElevation = grid[0][0];
	
	while (!pq.empty()) {
		auto [elevation, pos] = pq.top();
		int row = pos.first, col = pos.second;
		pq.pop();
		
		maxElevation = max(maxElevation, elevation);
		if (row == n-1 && col == n-1) return maxElevation;
		
		for (auto& dir : directions) {
			int newRow = row + dir[0], newCol = col + dir[1];
			if (newRow >= 0 && newRow < n && newCol >= 0 && newCol < n && !visited[newRow][newCol]) {
				visited[newRow][newCol] = true;
				pq.push({grid[newRow][newCol], {newRow, newCol}});
			}
		}
	}
	return maxElevation;
}
```

## Bridges in Graph

a bridge is any edges in graph that on removal will create a new connected component

when we do a dfs traversal if we have visited an adjacent node then we skip that, this adjacent nodes are ancestors of the current node. 

The skipped edges are called back edges while the edges of the resultant tree formed due to dfs are called forward edges.

```cpp
void dfs(int node, int parent, int timer, vector<vector<int>>& adj, 
         vector<vector<int>>& bridges, vector<int>& visited,
         vector<int>& in_time, vector<int>& lowest_time) {
        
    visited[node] = 1;
    in_time[node] = timer;
    lowest_time[node] = timer;
    timer++;
        
    for (auto it: adj[node]) {
        if (it == parent) continue;
        
        if (visited[it] == 0) {
            dfs(it, node, timer, adj, bridges, visited, in_time, lowest_time);
            
            // take lowest of lowest_time of node and lowest_time of it
            lowest_time[node] = min(lowest_time[node], lowest_time[it]);
            // check if node to it is a bridge
            if (lowest_time[it] > in_time[node]) {
                bridges.push_back({it, node});
            }
        } else {
            lowest_time[node] = min(lowest_time[node], lowest_time[it]);
        }
    }
}

vector<vector<int>> tarjan_bridges(int n, vector<vector<int>>& adj) {
    vector<int> visited(n, 0);
    vector<int> in_time(n);
    vector<int> lowest_time(n);
    vector<vector<int>> bridges;
    
    int timer = 1;
    dfs(0, -1, timer, adj, bridges, visited, in_time, lowest_time);
    return bridges;
}
```

## Articulation Points in Graph

node that on removal create new connected components























# DP

Methods to solve:
- Tabulation (Iterative) -> bottom up
- Memoization (Recursive) -> top down

find recurrence relation in problems

### Fibonacci numbers
in recursion we end up solving same subproblems which are called overlapping subproblems
so we memoization the condition of subproblem with solution using matrix, map, table etc
```cpp
// Recursion
int fib (int n) {
	if (n <= 1) return n;
	return fib(n-1) + fib(n-2);
}

// Dynamic Programming
// Memoization -> Recursive -> top down
int fib (int n, vector<int>& dp) {
	if (n <= 1) return n;
	if (dp[n] != -1) return dp[n];
	return dp[n] = fib(n-1, dp) + fib(n-2, dp);
}
int main() {
	int n = 7;
	vector<int> dp(n+1, -1);
	cout << fib(n, dp) << endl;
	
	return 0;
}

// Tabulation -> Iterative -> bottom up
int main() {
	int n = 7;
	vector<int> dp(n+1, -1);
	
	dp[0] = 0, dp[1] = 1;
	for (int i=2; i<=n; i++) {
		dp[i] = dp[i-1] + dp[i-2];
	}
	cout << dp[n] << endl;
	
	return 0;
}

// Tabulation space optimized
int main() {
	int n = 7;
	
	int prev2 = 0, prev = 1;
	for (int i=2; i<=n; i++) {
		int curr = prev + prev2;
		prev2 = prev;
		prev = curr;
	}
	cout << prev << endl;
	
	return 0;
}
```

## 1D DP

- Try to represent the problem in terms of index
- write all possible ways an index can behave according to the question
- sum up, find min, find max etc. according to the question 

### Climbing Stairs
```cpp
// Recursive
int fnc(int stair) {
	if (stair == 0 || stair == 1) return 1;
	int c1 = fnc(stair-1);
	int c2 = fnc(stair-2);
	return c1+c2; 
}

int climbStairs(int n) {
	return fnc(n);
}

// Memoization
int fnc(int stair, vector<int>& dp) {
	if (stair == 0 || stair == 1) return 1;
	int c1=0, c2=0;
	if (dp[stair-1] != -1) c1 = dp[stair-1];
	else {
		c1 = fnc(stair-1, dp);
		dp[stair-1] = c1;
	}
	if (dp[stair-2] != -1) c2 = dp[stair-2];
	else {
		c2 = fnc(stair-2, dp);
		dp[stair-2] = c2;
	}
	return c1+c2; 
}

int climbStairs(int n) {
	vector<int> dp(n+1, -1);
	return fnc(n, dp);
}
```

### House Robber
question => don't pick adjacent elements
```cpp
int fnc(int i, vector<int>& nums, vector<int>& memo) {
	if (i >= nums.size()) return 0;
	if (memo[i] != -1) return memo[i];
	
	int skip = fnc(i + 1, nums, memo);
	int rob = nums[i] + fnc(i + 2, nums, memo);
	
	return memo[i] = max(skip, rob);
}

int rob(vector<int>& nums) {
	if (nums.empty()) return 0;
	vector<int> memo(nums.size(), -1);
	return fnc(0, nums, memo);
}
```

### House Robber II
question => don't pick adjacent elements where elements are in circle
apply algo on start to end-1 and start+1 to end then return max of both 
```cpp
int fnc(int start, int end, vector<int>& nums) {
	int prev_rob = 0, max_rob = 0;
	for (int i=start; i<=end; i++) {
		int temp = max(max_rob, prev_rob + nums[i]);
		prev_rob = max_rob;
		max_rob = temp;
	}
	return max_rob;
}

int rob(vector<int>& nums) {
	int n = nums.size();
	if (n == 0) return 0;
	if (n == 1) return nums[0];
	if (n == 2) return max(nums[0], nums[1]);
	
	int case1 = fnc(0, n - 2, nums);
	int case2 = fnc(1, n - 1, nums);
	
	return max(case1, case2);
}
```

## 2D DP

- count unique paths
- count unique paths with obstacles
- min path sum
- max path sum
- triangle problem
- 2 start points

### Count unique paths
question => unique paths from `[0,0]` to `[m-1, n-1]`

we can think of dfs but for each cell we go right and down so TC => `O(2^(m*n))` SC => `O(n-1 + m-1)`
```cpp
int uniquePaths(int m, int n) {
	int cnt = 0;
	
	stack<pair<int, int>> st;
	st.push({0, 0});
	while (!st.empty()) {
		auto [row, col] = st.top();
		st.pop();
		
		if (row == m-1 && col == n-1) {
			cnt++;
			continue;
		}
		if (row+1 < m) st.push({row+1, col});
		if (col+1 < n) st.push({row, col+1});
	}
	
	return cnt;
}

// gives TLE
```

so we need to memoize the repeating subproblems TC => `O(n*m)` SC => `O(n-1 + m-1) + O(n*m)`
```cpp
int fnc (int row, int col, int m, int n, vector<vector<int>>& dp) {
	if (row == m-1 && col == n-1) return 1;
	if (row >= m || col >= n) return 0;
	
	if(dp[row][col] != -1) return dp[row][col];
	int right = fnc(row, col+1, m, n, dp);
	int down = fnc(row+1, col, m, n, dp);
	
	return dp[row][col] = right+down;
}

int uniquePaths(int m, int n) {
	vector<vector<int>> dp(m, vector<int>(n, -1));
	return fnc(0 , 0, m, n, dp);
}
```

we are using recursion stack space `O(n-1 + m-1)` which can be optimized by tabulation i.e. iterative bottom up approach
```cpp
int uniquePaths(int m, int n) {
	vector<vector<int>> dp(m, vector<int>(n, 0));
	
	for (int i=0; i<m; ++i) {
		for (int j=0; j<n; ++j) {
			if (i == 0 && j == 0) dp[i][j] = 1;
			else {
				int left = 0, up = 0;
				if (i>0) up = dp[i-1][j];
				if (j>0) left = dp[i][j-1];
				dp[i][j] = up + left;
			}
		}
	}
	
	return dp[m-1][n-1];
}
```

### Count Unique paths without obstacle
```cpp
int fnc (int row, int col, int m, int n, vector<vector<int>>& dp, vector<vector<int>>& obstacleGrid) {
	if (row >= m || col >= n) return 0;
	if (obstacleGrid[row][col] == 1) return 0;
	if (row == m-1 && col == n-1) return 1;
	
	if(dp[row][col] != -1) return dp[row][col];
	int right = fnc(row, col+1, m, n, dp, obstacleGrid);
	int down = fnc(row+1, col, m, n, dp, obstacleGrid);
	
	return dp[row][col] = right+down;
}

int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
	int m = obstacleGrid.size();
	int n = obstacleGrid[0].size();
	vector<vector<int>> dp(m, vector<int>(n, -1));
	
	return fnc(0 , 0, m, n, dp, obstacleGrid);
}
```

### Minimum Path Sum in Grid
```cpp
int fnc (int row, int col, int m, int n, vector<vector<int>>& dp, vector<vector<int>>& grid) {
	if (row >= m || col >= n) return 1e7;
	if (row == m-1 && col == n-1) return grid[row][col];
	
	if(dp[row][col] != -1) return dp[row][col];
	int right = grid[row][col] + fnc(row, col+1, m, n, dp, grid);
	int down = grid[row][col] + fnc(row+1, col, m, n, dp, grid);
	
	return dp[row][col] = min(right, down);
}

int minPathSum(vector<vector<int>>& grid) {
	int m = grid.size();
	int n = grid[0].size();
	vector<vector<int>> dp(m, vector<int>(n, -1));
	
	return fnc(0 , 0, m, n, dp, grid);
}
```

### Minimum Path Sum in Triangle
```cpp

```



















