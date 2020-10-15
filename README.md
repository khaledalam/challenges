Challenges (my random notes)

Author: Khaled Alam (khaledalam.net@gmail.com)
© 2020
_______________________

- Quantum computing
- Primes
- RSA Cryptosystem
- Shor's Algorithm
- Ulam spiral
- Secure Hash Algorithms: SHA-2, SHA-256, SHA-512
- GNFS
- Quantum Fourier transform
- Knapsack
- Compress the 1GB file enwik9 to less than the current record of about 116MB
- PI
- Mersenne Primes
- Miller-Rabin Algortihm




------


## Primes:

on classical computer:

|Lower bound | Upper bound | Count primes | ~ Elapsed time | Threads | Sieve size | Options|
--- |--- |--- |--- |--- |--- |---
|0 | 1               | 4             | 0.0 sec       | 1  | 4096 KiB | printing|
|0 | 100             | 25            | 0.0 sec       | 1  | 4096 KiB | printing|
|0 | 1000            | 168           | 0.0 sec       | 1  | 4096 KiB | printing|
|0 | 10000           | 1229          | 0.0 sec       | 1  | 4096 KiB | printing|
|0 | 100000          | 9592          | 0.0 sec       | 1  | 4096 KiB | printing|
|0 | 1000000         | 78498         | 1.46 sec      | 1  | 4096 KiB | printing|
|0 | 10000000        | 664579        | 11.92 sec     | 1  | 4096 KiB | printing|
|0 | 100000000       | 5761455       | 124.70 sec    | 1  | 4096 KiB | printing|
|0 | 100000000       | 5761455       | 0.0 sec       | 8  | 4096 KiB | -|
|0 | 1000000000      | 50847534      | 0.0 sec       | 12 | 4096 KiB | -|
|0 | 10000000000     | 455052511     | 3.06 sec      | 12 | 4096 KiB | -|
|0 | 100000000000    | 4118054813    | 48.99 sec     | 12 | 4096 KiB | -|
|0 | 1000000000000   | 37607912018   | - sec     | 12 | 4096 KiB | -|

sequence: https://oeis.org/A006880


## Primes Figures:

5x5 and 6x6:<br>
<img src="primes_figures/nxn/primes_6x6.png" height="250">
<img src="primes_figures/nxn/primes_7x7.png" height="250">

<hr>

2-1000 and 2-10000:<br>
<img src="primes_figures/linear/primes_linear_2_1000.png" height="250">
<img src="primes_figures/linear/primes_linear_2_10000.png" height="250">

### Full => <a href="primes_figures.md">primes_figures.md</a>

-----
## Mersenne Primes:

```Author: Khaled Alam(khaledalam.net@gmail.com)

Mersenne Primes:
(2^P)-1 |     value      | time
-----------------------------------------
(2 ^  2)-1 =       3 | Elasped time is 0.00 seconds.
(2 ^  3)-1 =       7 | Elasped time is 0.00 seconds.
(2 ^  5)-1 =      31 | Elasped time is 0.00 seconds.
(2 ^  7)-1 =     127 | Elasped time is 0.00 seconds.
(2 ^ 13)-1 =    8191 | Elasped time is 0.05 seconds.
(2 ^ 17)-1 =  131071 | Elasped time is 1.10 seconds.
(2 ^ 19)-1 =  524287 | Elasped time is 5.79 seconds.
...
```

### Code: 
<details><summary>Mersenne Primes C++ Code using BigInteger</summary><p>

```c++
/**
 *    Author: Khaled Alam (ninjo)
 *    Email : khaledalam.net@gmail.com
 **/
#include <bits/stdc++.h>
#include "BigNumberCPP/bignumber.h"

using namespace std;

map<BigNumber, bool> vis;
map<BigNumber, bool> mem;

bool isPrime(BigNumber &N) {

	if (vis[N])
		return mem[N];

	if (N <= BigNumber(1)) {
		vis[N] = true;
		return mem[N] = false;
	}


	if (N <= BigNumber(3)) {
		vis[N] = true;
		return mem[N] = true;
	}

	BigNumber divBy2 = N / BigNumber(2);
	BigNumber divBy3 = N / BigNumber(3);

	if ((divBy2 * BigNumber(2)) == N || (divBy3 * BigNumber(3)) == N) {
		vis[N] = true;
		return mem[N] = false;
	}

	BigNumber itr(5);

	while (itr * itr <= N) {

		BigNumber divByI = N / itr;
		BigNumber divByIPlus2 = N / (itr + BigNumber(2));

		if (divByI * itr == N || divByIPlus2 * (itr + BigNumber(2)) == N) {
			vis[N] = true;
			return mem[N] = false;
		}

		itr += BigNumber(6);
	}

	vis[N] = true;
	return mem[N] = true;

}

int main() {
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
//	freopen("input.txt", "r", stdin); //freopen("output.txt", "w", stdout);

	BigNumber base(2);

	int exp = 1;
	printf("Author: Khaled Alam(khaledalam.net@gmail.com)\n\nMersenne Primes:\n(2^P)-1 |%4s value %4s | time\n-----------------------------------------\n", "", "");

	const int MX = 100;

	while (exp++ <= MX) {
		BigNumber F(base.pow(exp) - BigNumber(1));

		clock_t begin = clock();

		bool result = isPrime(F);

		clock_t end = clock();
		double elapsed_secs = double(end - begin) / CLOCKS_PER_SEC;

		if (result) {
			printf("(2 ^%3d)-1 = %7s | Elasped time is %.2lf seconds.\n",
					exp, F.getString().c_str(), elapsed_secs);
		}
	}

}

```
</p></details>


<br><br>

### Advanced:

(Probabilistic tests, Miller-Rabin, Fermat primality test, Carmichael numbers, ...)

output:
```
time: 0:00:00.000002,   (2^2)-1,      digits length: 1
time: 0:00:00.000283,   (2^3)-1,      digits length: 1
time: 0:00:00.000274,   (2^5)-1,      digits length: 2
time: 0:00:00.000276,   (2^7)-1,      digits length: 3
time: 0:00:00.000449,   (2^13)-1,     digits length: 4
time: 0:00:00.000477,   (2^17)-1,     digits length: 6
time: 0:00:00.000536,   (2^19)-1,     digits length: 6
time: 0:00:00.001249,   (2^31)-1,     digits length: 10
time: 0:00:00.002967,   (2^61)-1,     digits length: 19
time: 0:00:00.004160,   (2^89)-1,     digits length: 27
time: 0:00:00.005148,   (2^107)-1,    digits length: 33
time: 0:00:00.006455,   (2^127)-1,    digits length: 39
time: 0:00:00.072748,   (2^521)-1,    digits length: 157
time: 0:00:00.107656,   (2^607)-1,    digits length: 183
time: 0:00:01.026329,   (2^1279)-1,   digits length: 386
time: 0:00:03.711185,   (2^2203)-1,   digits length: 664
time: 0:00:03.835759,   (2^2281)-1,   digits length: 687
time: 0:00:10.750292,   (2^3217)-1,   digits length: 969
time: 0:00:22.843608,   (2^4253)-1,   digits length: 1281
time: 0:00:25.689716,   (2^4423)-1,   digits length: 1332
...
```


-----

## Quantum computer:

* Break 2048-bit RSA encryption in 8 hours



-----
## PI:

PI Benchmark Multi-Threaded:  (RAM)<br>
<img src="imgs/PI_Benchmark_Multi-Threaded.png" width="400">

