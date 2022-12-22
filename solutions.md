## ALTERNATE SOLUTIONS

### Solution 1
```js
function sumPrimes(num) {
  // Helper function to check primality
  function isPrime(num) {
    for (let i = 2; i <= Math.sqrt(num); i++) {
      if (num % i == 0)
        return false;
    }
    return true;
  }

  // Check all numbers for primality
  let sum = 0;
  for (let i = 2; i <= num; i++) {
    if (isPrime(i))
      sum += i;
  }
  return sum;
}
```

### Code Explanation
- We loop over all values in our range, adding them to the sum if they are prime.
- Our primality checking function returns `false` if the target number is divisible by any number in between 2 and the square root of the target number.
- We only need to check up to the square root because the square root of a number is the largest possible unique divisor.

### Solution 2
```js
function sumPrimes(num) {
  // Check all numbers for primality
  let primes = [];
  for (let i = 2; i <= num; i++) {
    if (primes.every((prime) => i % prime !== 0))
      primes.push(i);
  }
  return primes.reduce((sum, prime) => sum + prime, 0);
}
```

### Code Explanation
- This solution is very similar to Solution 1.
- In this solution we retain a list of all primes found so far and check if any of these numbers divide into each number in our range.
- Note that this solution is actually less efficient than Solution 1 for very large values of `n`.
- Frequently growing the size of an array in JavaScript can be inefficient and slow.

### Solution 3
```js
function sumPrimes(num) {
  // Prime number sieve
  let isPrime = Array(num + 1).fill(true);
  // 0 and 1 are not prime
  isPrime[0] = false;
  isPrime[1] = false;
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (isPrime[i]) {
      // i has not been marked false -- it is prime
      for (let j = i * i; j <= num; j += i)
        isPrime[j] = false;
    }
  }

  // Sum all values still marked prime
  return isPrime.reduce(
    (sum, prime, index) => prime ? sum + index : sum, 0
  );
}
```

### Code Explanation
- This solution is based on the [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes).
- We create a boolean array for the primality of each number in our range.  All numbers except 0 and 1 are assumed to be prime.
- Then, we start with 2 and proceed to `num`, marking every multiple of a prime number as `false`, or 'not prime'.
- Finally, we reduce our sieve array, returning the sum of all indices still marked as `true` or 'prime'.

Note: many optimizations can be made to improve the efficiency of this sieve, but they have been omitted for the sake of simplicity and readability.

