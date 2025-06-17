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
> `x * 2^k`  = `x<<k`
> `x / 2^k  = x>>k`

### kth bit:
`1<<k = 2^k`

> Toggle kth bit: `x ^ (1<<k)`
> Set kth bit (make kth bit `1`): `x | (1<<k)`
> Unset kth bit (make kth bit `0`): `x & ~(1<<k)`
> Remove the last set bit (rightmost): `x & (x-1)`

### Swap 2 numbers using bit manipulation
`x = x^y` => `x = x^y , y = y`
`y = x^y` => `x = x^y , y = x^y^y = x`
`x = x^y` => `x = x^y^x = y , y = x`

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



