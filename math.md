# Math 

## Kahan Summation 

In numerical analysis, the Kahan summation algorithm, also known as compensated summation,[1] significantly reduces the numerical error in the total obtained by adding a sequence of finite-precision floating-point numbers, compared to the obvious approach. This is done by keeping a separate running compensation (a variable to accumulate small errors), in effect extending the precision of the sum by the precision of the compensation variable.


```bash
function KahanSum(input)
    // Prepare the accumulator.
    var sum = 0.0
    // A running compensation for lost low-order bits.
    var c = 0.0
    // The array input has elements indexed input[1] to input[input.length].
    for i = 1 to input.length do
        // c is zero the first time around.
        var y = input[i] - c
        // Alas, sum is big, y small, so low-order digits of y are lost.         
        var t = sum + y
        // (t - sum) cancels the high-order part of y;
        // subtracting y recovers negative (low part of y)
        c = (t - sum) - y
        // Algebraically, c should always be zero. Beware
        // overly-aggressive optimizing compilers!
        sum = t
    // Next time around, the lost low part will be added to y in a fresh attempt.
    next i

    return sum

```
```cpp
// C++ program to illustrate the
// floating-point error
#include<bits/stdc++.h>
using namespace std;

double floatError(double no) 
{ 
	double sum = 0.0; 
	for(int i = 0; i < 10; i++) 
	{ 
		sum = sum + no; 
	} 
	return sum; 
} 

// Driver code
int main()
{
	cout << setprecision(16);
	cout << floatError(0.1);
}

// This code is contributed by rutvik_56

```

```cpp
// C++ program to illustrate the
// Kahan summation algorithm 
#include <bits/stdc++.h> 
using namespace std; 

// Function to implement the Kahan
// summation algorithm
double kahanSum(vector<double> &fa)
{
	double sum = 0.0;

	// Variable to store the error
	double c = 0.0;

	// Loop to iterate over the array
	for(double f : fa) 
	{
		double y = f - c;
		double t = sum + y; 
		
		// Algebraically, c is always 0
		// when t is replaced by its
		// value from the above expression.
		// But, when there is a loss,
		// the higher-order y is cancelled
		// out by subtracting y from c and
		// all that remains is the
		// lower-order error in c
		c = (t - sum) - y; 
		sum = t;
	}
	return sum;
}

// Function to implement the sum
// of an array
double sum(vector<double> &fa)
{
	double sum = 0.0;

	// Loop to find the sum of the array
	for(double f : fa) 
	{
		sum = sum + f;
	} 
	return sum;
} 

// Driver code 
int main() 
{ 
	vector<double> no(10);
	for(int i = 0; i < 10; i++)
	{
		no[i] = 0.1;
	}

	// Comparing the results
	cout << setprecision(16);
	cout << "Normal sum: " << sum(no) << " \n";
	cout << "Kahan sum: " << kahanSum(no);	 
} 

// This code is contributed by ajaykr00kj

```

## Reference
1.<https://en.wikipedia.org/wiki/Kahan_summation_algorithm>