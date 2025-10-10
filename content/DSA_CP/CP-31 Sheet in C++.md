[[DSA_CP/index|DSA_CP/index]]

# 800 Rating

### Jagged Swaps
Determine whether it is possible to sort the permutation after a finite number of operations, where an operations means:
Select an index i from 2 to n−1 such that `(a[i]−1 < a[i]) && (a[i] > a[i]+1)`, then Swap `a[i]` and `a[i]+1`

**Sol**: as all number except `a[0]` can be rearranged 
```cpp
if (a[0] == 1) cout << "YES" << endl;
else cout << "NO" << endl;
```

### Doremy's Paint 3
An array b1,b2,…,bn of positive integers is good if there exists a `k` such that `b1+b2 = b2+b3 = … = bn−1+bn = k`

**Sol**: We need <= 2 different numbers as we need to make every even number equal and same for odd number 
```cpp
void jagged_swaps() {
    int n;
    cin >> n;
    map<int, int> mp;
    
    // Input
    int a[n];
    for (int i=0; i<n; ++i) {
        cin >> a[i];
        mp[a[i]]++;
    }
    
    // If more than 3 different numbers 
    // can't divide them correctly so that sum is equal
    if (mp.size() >= 3) {
        cout << "No" << endl;
        return;
    }
    
    auto [num1, temp1] = *mp.begin();
    auto [num2, temp2] = *mp.rbegin();
    
    // can differ by one for odd length array 
    if (abs(temp1-temp2) <= 1) {
        cout << "Yes" << endl;
    } else {
        cout << "No" << endl;
    }
    
    return;
}
```

### Aleksa and Stack
generate an array for a given length `n` where `3 * a[i+2]` is not divisible by `a[i] + a[i+1]` for each i (1 ≤ i ≤ n−2)

**Sol**: odd numbers from 1 to n
odd + odd = even 
and 3 * odd = odd
so  if we write n odd numbers then the conditions hold

### Vasilije in Cacak
given three positive integers: n, k, and x, determine if you can choose k distinct integers between 1 and n, such that their sum is equal to x.

minimum sum of k numbers = sum of 1 to k numbers
maximum sum of k numbers = sum of last k number i.e. n-k to n
```cpp
void solve() {
    int n, x, k;
    cin >> n >> k >> x;
    
    int total = (n*(n+1))/2;
    
    int k_upper = total - ((n-k)*(n-k+1))/2;
    int k_lower = (k*(k+1))/2;
    
    if(x >= k_lower && x <= k_upper) cout << "Yes" << endl;
    else cout << "No" << endl;
    return;
}
```



