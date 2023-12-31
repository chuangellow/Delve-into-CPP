# Chapter 1. Recursion

## 1. Recursion Algorithm

遞迴在數學上的定義為：**直接或間接定義自己**，而在程式上的定義為：**直接或間接呼叫自己**

通常遞迴的使用時機為：

1. 根據定義來實作
2. 以遞迴來進行窮舉暴搜

而Recursion Algorithm的種類主要有三種：

- Direct Recursion：一個function可以自我呼叫，則稱之為Direct Recursion
- Indirect Recursion：一個function可能會呼叫其他的function，而這個被他呼叫function可能呼叫回原來的function，形成一個call cycle，則稱之為Indirect Recursion
- Tail Recursion：在程式的END敘述之前，存在一個recursive call的敘述，即為tail recursion，又會為direct recursion的一種

## 2. Recurence Relation

如果一個程式有用到自我呼叫也就是遞迴的關係，則其總執行時間可以寫作如下：

若設計一個遞迴的program，其所需執行時間為：$T(n)$，其中n為輸入大小

而此程式每次遞迴都會產生a個子問題，且每個子問題的大小為：$\frac{n}{b}$

則除了遞迴迭代外所需的時間為$f(n)$，則可以寫出此遞迴的執行時間如下：

$$
T(n)=aT(\frac{n}{b})+f(n)
$$

而如果其中：a, b都會為大於等於1的實數、f(n)會為隨著n增加的正成長函數

欲解此一Time function有多種方法，分別為：

- 展開代入法
- Recursion Tree法
- Master Theory
- Extended Master Theory
- 離散數學中的特徵值法
- 近似法

由上列的方法可以猜測出一Time function的upper bound或是lower bound，但是欲驗證其正確性必須透過substitution method來驗證

substitution method的概念就是透過數學歸納法的證明，只是我們不必去證起始步驟，因為只要在取夠大的c時，起始步驟必定會滿足

之後，我們透過inductive step的假設，來證得所猜的bound的正確性

猜測的流程應為：

先判斷子問題的分法，若為：

$$
T(n)=aT(\frac{n}{b})+f(n)
$$

則先用Master Theory來判斷，若是不能用Master Theory的case，再來用展開代入法求

而其他種的則可以用Recursion Tree，先判斷他的level cost是否一致，若一樣，則為：$\theta(f(n)*logn)$，反之若為等比關係，則為：$\theta(f(n))$

## 3. Recursion By definition

如同上述所說，遞迴的使用條件通常為

1. 根據定義來實作
2. 以遞迴來進行窮舉暴搜

根據定義的遞迴實作上通常很容易，困難之處在於構思出遞迴關係。

### factorial

數學定義為：
$$
n!=\begin{cases}1, \ \ if \ n=0 \\ (n-1)!· n \ \ if \ n>0\end{cases}
$$

```
long long int factorial(long long int n) {
	if (n == 0) return 1;
	return (n * (factorial(n - 1)));
}
```

### Fibonacci

數學定義為：

$$
F_n=\begin{cases}0 \ \ if \ n=0 \\1 \ \ if \ n=1\\ F_{n-1}+F_{n-2} \ \ otherwise\end{cases}
$$

```
long long int fibonacci(long long int n) {
	if (n == 0 || n == 1) return n;
	return (fibonacci(n - 1) + fibonacci(n - 2));
}
```

### Combination

數學定義為：

$$
\binom nm=\begin{cases}1 \ \ if \ n=m\ or \ m=0 \\\binom {n-1}m+\binom {n-1}{m-1}\end{cases}
$$

```
long long int combination(long long int n, long long int m) {
	if (n == m || m == 0) return 1;
	return combination(n-1, m) + combination(n-1, m-1);
}
```

### Composition Function

問題定義為：

>Q-1-2. 合成函數(2) (APCS201902)
>
> 令 f(x)=2x–3;g(x,y)=2x+y–7;h(x,y,z)=3x–2y+z。本題要計算一個合成函數的值，例如 h(f(5),g(3,4),3)=h(7,3,3)=18。 
> 
> Time limit: 1秒
> 
> 輸入格式:輸入一行，長度不超過1000，它是一個f, g, 與h的合成函數，但所有的括弧與逗號都換成空白。輸入的整數絕對值皆不超過 1000。
> 
> 輸出: 輸出函數值。最後答案與運算過程不會超過正負 10 億的區間。 
> 
> 範例輸入:
h f 5 g 3 4 3

> 範例輸出:
18

關鍵在於如何定義遞迴式，遞迴的時間為何，還有邊界條件是什麼，
這裡一次會讀入一個值，可能有 ```h, f, g``` 或是 number 幾種結果。

如果是 ```h, f, g```，需要繼續 parse，直到得到一個 number，才回傳該 number，並依照 ```h, f, g``` 的定義來計算。

```
int eval() {
	int val, x, y, z;
	char token[10];
	if (scanf("%d", &val) == 1) {
		return val;
	}
	scanf("%s", token);
	if (token[0] == 'f') {
		x = eval();
		return 2 * x - 3;
	}
	else if (token[0] == 'g') {
		x = eval();
		y = eval();
		return 2 * x + y - 7;
	}
	else if (token[0] == 'h') {
		x = eval();
		y = eval();
		z = eval();
		return 3 * x - 2 * y + z;
	}
}
```

### 棍子中點切割

>P-1-3. 棍子中點切割
>
> 有一台切割棍子的機器，每次將一段棍子會送入此台機器時，機器會偵測棍子上標示的可切割點，然後計算出最接近中點的切割點，並於此切割點將棍子切割成兩段，切割後的每一段棍子都會被繼續送入機器進行切割，直到每一段棍子都沒有切割點為止。請注意，如果最接近中點的切割點有二，則會選擇座標較小的切割點。每一段棍 子的切割成本是該段棍子的長度，輸入一根長度 L 的棍子上面 N 個切割點位置的座 標，請計算出切割總成本。
> 
> Time limit: 1秒
> 
> 輸入格式:第一行有兩個正整數 N 與 L。第二行有 N 個正整數，依序代表由小到大的 切割點座標 p[1]~p[N]，數字間以空白隔開，座標的標示的方式是以棍子左端為 0， 而右端為L。N < 5e4，L < 1e9。
> 
> 輸出: 切割總成本點
> 
> 範例輸入:
4 10
1 2 4 6

> 範例輸出:
22


可以容易從子問題想見該題目的遞迴方法為，每次切割後就是在解剩餘一段棍子左端和右端的棍子的子問題，

而邊界條件為棍子長度小於1時，因此步驟為：

```
1. Find the mid point
2. Search for the cutting point
3. Calculate the cost
```

關鍵在於找 cutting point 時，可以使用 binary search 降低複雜度至 $O(\log(n))$

這邊可以使用 jumping binary search

```
int pos = left;
for (int jump = (right - left) / 2; jump >= 0; jump /=2) {
	while (pos + jump < right && p[pos + jump] < target) {
		pos += jump;
	}
}
```

或者使用 STL 裡已有的 ```lower_bound(s, t, x)```，即可在 $[s, t)$ 範圍中找到第一個大於等於 x 的值。

詳見 [cpp reference](https://en.cppreference.com/w/cpp/algorithm/lower_bound) 的介紹，注意的是如果給定的切割點座標不為排序過的，就無法使用。

### 二維黑白影像編碼

>Q-1-5 二維黑白影像編碼
>
> 假設 n 是 2 的冪次，也就是存在某個非負整數 k 使得 n = $2^k$ 。將一個 n*n 的黑白 影像以下列遞迴方式編碼:
如果每一格像素都是白色，我們用 0 來表示;
如果每一格像素都是黑色，我們用 1 來表示; 否則，並非每一格像素都同色，先將影像均等劃分為四個邊長為 n/2 的小正方形 後，然後表示如下:先寫下 2，之後依續接上左上、右上、左下、右下四塊的編碼。
輸入編碼字串 S 以及影像尺寸 n，請計算原始影像中有多少個像素是 1。
> 
> Time limit: 1秒
> 
> 輸入格式:第一行是影像   S，字串長度小於 1,100,000。第二行為正整數 n，$1\le n \le 1024$，中 n 必為2的冪次。
> 
> 輸出: 輸出共有多少個像素為 1
> 
> 範例輸入:
> 
2020020100010
>
>8
>
> 範例輸出:
17

可以從一個例子輕鬆想出遞迴的方式在於解切割為 $n/2$ 大小的左上至右下的子問題，

而原問題像素為 1 的結果會為左上至右下子問題的解的和。

舉例來說：

```
2020020100010
```

第一個遇到 $2$，就代表下面要解四個子問題分別為：

1. $0$
2. $200201000$
3. $1$
4. $0$

遞迴終止條件在遇到 0 和 1 時，遇到 0， count 為 0，
而遇到 1 時， count 為 $n^2$

但該題關鍵反而在實作時，要如何得到下一個要處理的 character 的位置，

因為下一個處理的 character 位置必須在遞迴呼叫解完前一個子問題才會得知，

舉例來說，解完在

```
2020020100010
```

左下的 subblock 的 pos 必須解完右上的 subblock 才會知道。

因此，遞迴返回的東西必須包含 pos 和 count。

這邊就以一個 struct 來包裝遞迴返回的兩個值：

```
typedef struct Result {
	long long int count;
	int pos;
} Result;
```

```
Result eval(int pos, char encoded_string[], int n) {
	if (pos >= strlen(encoded_string) || n == 0) return {0, pos};
	Result lu, ru, lb, rb;
	lu.count = ru.count = lb.count = rb.count = 0;
	lu.pos = ru.pos = lb.pos = rb.pos = pos;
	switch (encoded_string[pos]) {
		case '0':
			return {0, pos+1};
			break;
		case '1':
			return {n*n, pos+1};
			break;
		case '2':
			lu = eval(pos+1, encoded_string, n/2);
			ru = eval(lu.pos, encoded_string, n/2);
			lb = eval(ru.pos, encoded_string, n/2);
			rb = eval(lb.pos, encoded_string, n/2);
			return {lu.count+ru.count+lb.count+rb.count, rb.pos};
			break;
		default:
			return {0, pos+1};
			break;
	}
}
```

## 4. Recursion to Enumeration

Enumeration 和 Brute-force 都是透過窮舉出所有可能的解來搜尋答案的演算法策略。

Brute-force 通常是窮舉某種可能的組合結構，如：所有排列、所有子集合等等。

而遞迴的窮舉則是以樹狀圖方式來展開所有可能的組合，因此又稱作 Tree searching algorithm。

下面為一例子：

### Subarray Sum

> P-1-6. 最接近的區間和
> 
假設陣列 A[1..n]中存放著某些整數，另外給了一個整數 K，請計算哪一個連續區段的和最接近 K 而不超過 K。

要找一連續區段，可以透過左右注標來定義：$[i, j]$，因此，問題變成了窮舉下面的組合：$1\le i \le j \le n$

窮舉所有的組合為：$O(n^2)$，並迴圈內必須計算連續區間和，最簡單的方法為用一個迴圈計算，時間複雜度為：$O(n^3)$，方法為：

```
int best = K;
for (int i = 1; i <= n; i++) {
	for (int j = i; j <= n; j++) {
		int sum = 0;
		for (int k = i; k <= j; k++) {
			sum += A[k];
		}
		if (sum <= K && K - sum < best) {
			best = K - sum;
		}
	}
}
```
但計算連續區間和是可以再優化的，因為每次固定左端 A[i] 遍歷所有 j 時， sum 只多了 A[j] 而已。

```
int best = K;
for (int i = 1; i <= n; i++) {
	int sum = 0;
	for (int j = i; j <= n; j++) {
		sum += A[j];
		if (sum <= K && K - sum < best) {
			best = K - sum;
		}
	}
}
```
這樣一來就變為 $O(n^2)$ 了。

另一種方式是利用常見的技巧為 accumulative sum，做前處理在 $O(n)$ 預先算好，

舉例來說：
```A = [2,3,1,2,4,3]```

accumulative sum 為:

```accumulative_sum = [0,2,5,6,8,12,15]```

我們可以透過一個簡單的減法來得到連續區間和

```
sum(A[1:3]) = accumulative_sum[4] - accumulative_sum[1] 
```

事實上，該題可以利用 two-pointer 的技巧來避免窮舉所有可能的解，

透過 two-pointer，可以維護一個滿足 constraint 的最佳的連續區間。

進而在 $O(n)$ 時間內找到最佳解。

```
vector<int> solve(vector<int> nums, int k) {
	vector<int> best_idx(2, 0);
	int best = k;
	int left = 0, right = 0, sum = 0;
	int size = nums.size();
	while (right < size) {
		sum += nums.at(right);
		while (sum > k) {
			sum -= nums.at(left);
			left++;
		}
		if (sum <= k && k - sum < best) {
			best = k - sum;
			best_idx.at(0) = left;
			best_idx.at(1) = right;
		}
		right++;
	}
	return best_idx;
}
```

### Enumerate Through All of the Subset

> P-1-7. 子集合乘積

> 輸入 n 個正整數，請計算其中有多少組合的相乘積除以 P 的餘數為 1，每個數字可以選取或不選取但不可重複選，輸入的數字可能重複。P=10009，0<n<26。 
> 
> 輸入第一行是 n，第二行是 n 個以空白間隔的正整數。
> 
> 輸出有多少種組合。若輸入為{1, 1, 2}，則有三種組合，選第一個 1，選第 2 個 1，以及選兩個 1。
> 
> time limit = 1 sec。

這題我們可以透過窮舉所有可能的 subset，計算相乘乘積後，再除上 P。

關鍵在於如何表示集合，我們可以透過一個 n bit vector 來表達一個有 n 個元素的集合。

舉例來說，假設一集合為 ```{1, 1, 2}``` 共有 3 個元素，可以用 000~111 表示所有子集合，其中 1 代表該 index 元素在子集合中，0 則否。

透過窮舉 bit vector，可以窮舉所有可能的子集合，並計算該子集合元素的 multiplication。

```
int solve(vector<int> nums, int p) {
	int comb = 0;
	int size = nums.size();
	for (int s = 1; s < (1 << size); s++) {
		long long int prod = 1;
		for (int i = 0; i < size; i++) {
			if (s & (1 << i)) {
				prod = (prod * nums[i]) % p;
			}
		}
		comb += (prod == 1)? 1: 0;
	}
	return comb;
}
```

而同時，我們也可以利用遞迴式來 enumerate 所有子集合。

考慮子問題為在取到集合的第 i 個元素時，可以決定取或不取，

不取的方法為不將第 i 個元素乘到當前累積的集合元素乘積。

```
void solve(int pos, int prod, int *ans, vector<int> nums, int p, int n) {
	if (pos >= n) {
		if (prod == 1) *ans += 1;
		return;
	}
	solve(pos+1, (prod * nums.at(pos)) % p, ans, nums, p, n);
	solve(pos+1, prod, ans, nums, p, n);
	return;
}
```

### Subset Sum

## 5. Intuition of Designing a Recurence Relation

1. 先構思該問題的子問題是什麼
2.  如何由子問題的解兜出原問題的解
3.  邊界條件為何？遞迴何時終止