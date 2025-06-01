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

***Merge two Sorted Arrays Without Extra Space***
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

***Count Inversion***

***Reverse Pairs***
```cpp

```

# Binary Search
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

***Upper Lower Bounds***
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

