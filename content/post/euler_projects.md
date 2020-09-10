---
title: "Python Solution to 3 Euler projects"
date: 2020-09-10T06:12:14+08:00
draft: false
---

[Euler Projects](https://projecteuler.net/archives) are a series of computational problems intended to be solved with computer programs. Today I will show the process of solving Problem 8, 41 and 82 using python.

## Problem 82. Path sum: three ways

The minimal path sum in the 5 by 5 matrix below, by starting in any cell in the left column and finishing in any cell in the right column, and only moving up, down, and right, is indicated in red and bold; the sum is equal to 994.

Find the minimal path sum from the left column to the right column in [matrix.txt](https://projecteuler.net/project/resources/p082_matrix.txt) (right click and "Save Link/Target As..."), a 31K text file containing an 80 by 80 matrix.

### Analysis

Starting from any position in the first column, we can only move up, down, and right to the last column, so that the sum of the passing numbers is the smallest. Since we cannot go left, we can slice the matrix into columns and solve them one by one. The best way is to look from the last column to each previous ones.

Define an array called ans to store the smallest path sum at each column. ans[i] represents the smallest path when going to line i and we are going to update it column by column. Starting from the last column, "walk" to the previous column. The initial values of ans is the the last column of the matrix. Suppose we are at the next to last column. What we want is the minimum value of the path taken. Let's first consider only going 'down' or 'left'. Then for entries in the first line, the only option is to go from left. i.e. ans[0] = ans[0] + entry value. To arrive at the rest of the entries, there are two options: by going left directly from the entry to their right or by going through the entry above. We choose the minimum to store in ans[j]. Then let's consider going 'up' from the entry below it. Similarly we compare the value of ans[j] from the provious step with ans[j + 1] + entry value (going from the entry below it).

### Python Solution

```{python}
def min_path_sum(lst):

    """
    Find the minimum path sum of only moving up, down, and right to travel from the first column to the last column in a matrix.

    Parameters
    ----------
    lst : list
        list of list that stores a square matrix

    Returns
    -------
    int
        the minimum path sum
    """
    import numpy as np
    size, size = np.shape(lst)
    ans = [lst[i][size - 1] for i in range(size)]
    for i in range(size - 2, -1, -1):
        ans[0] = ans[0] + lst[0][i]
        for j in range(1, size):
            ans[j] = min(ans[j] + lst[j][i],ans[j-1] + lst[j][i])
        for j in range(size - 2, -1, -1):
            ans[j] = min(ans[j],ans[j + 1] + lst[j][i])
    return min(ans)


from urllib import request  # the lib that handles the url stuff
txt = request.urlopen('https://projecteuler.net/project/resources/p082_matrix.txt').read()
lines = [line.decode("utf-8") for line in txt.split()]
lst = [list(map(int, line.split(','))) for line in lines]
path_sum(lst)

# returns 260324
```


## Problem 41. Pandigital prime

We shall say that an n-digit number is pandigital if it makes use of all the digits 1 to n exactly once. For example, 2143 is a 4-digit pandigital and is also prime.  

What is the largest n-digit pandigital prime that exists?  

### Analysis
By definition we know that pandigital number is at most 9-digit. The 9-digit pandigital number is a permutation of '987654321'. Since if the digit sum can be divided by 3, the whole number can be divided by 3 (which is not prime anymore), any permutation of '987654321' should not be prime since its digit sum equals 45. In a similar manner, the 8-digit pandigital numbers cannot be prime either. So the largest pandigital number exsited should be at most 7-digit. It can be shown that for n = 2, 3, 5, 6, all pandigital numbers are not prime so they should be ruled out as well. Then we are only left with 4-digit and 7-digit.  

I solved this problem in 3 steps. First write a function to check if the number is prime. Then use the function **permutations()** from itertools package to generate all possible combinations of n digits (to make the number pandigital). Finally write a function to enumerate the number list backwards and check if each one is prime.

### Python Solution

```{python}
# define function is_prime

def is_prime(x):
    """
    Check if the given number is prime.

    Parameters
    ----------
    x : digits
        a numerical number

    Returns
    -------
    boolean
        True/False
    """
    prime = True
    for i in range(2, round(x/2) + 1):
        if x % i == 0:
            prime = False
            break
    return prime


# define function largest_pandigital_prime

def largest_pandigital_prime(n):
    """
    Find the largest n-digit pandigital prime number.

    Parameters
    ----------
    n : digit
        a numerical number between 4 and 7.

    Returns
    -------
    int
        the largest n-digit pandigital prime number.
    """

    from itertools import permutations
    assert n in [4, 7], 'Invalid input n. '

    per = permutations(range(n, 0, -1))

    for p in per:
        if p[-1] % 2 != 0:
            number = int(''.join(map(str, p)))
            if is_prime(number):
                print(number)
                break

largest_pandigital_prime(7)

# returns 7652413
```

## Problem 8. Largest product in a series

The four adjacent digits in the 1000-digit number that have the greatest product are 9 × 9 × 8 × 9 = 5832.

73167176531330624919225119674426574742355349194934
96983520312774506326239578318016984801869478851843
85861560789112949495459501737958331952853208805511
12540698747158523863050715693290963295227443043557
66896648950445244523161731856403098711121722383113
62229893423380308135336276614282806444486645238749
30358907296290491560440772390713810515859307960866
70172427121883998797908792274921901699720888093776
65727333001053367881220235421809751254540594752243
52584907711670556013604839586446706324415722155397
53697817977846174064955149290862569321978468622482
83972241375657056057490261407972968652414535100474
82166370484403199890008895243450658541227588666881
16427171479924442928230863465674813919123162824586
17866458359124566529476545682848912883142607690042
24219022671055626321111109370544217506941658960408
07198403850962455444362981230987879927244284909188
84580156166097919133875499200524063689912560717606
05886116467109405077541002256983155200055935729725
71636269561882670428252483600823257530420752963450

Find the thirteen adjacent digits in the 1000-digit number that have the greatest product. What is the value of this product?

### Analysis
The first step to solve this problem is to generate a list of all possible 13-adjacent digits. One way is by using **zip()**. Then simply write a for loop to calculate and store the product of each number in the list and find the minimal one.

### Python Solution

```{python}
def greatest_prod(x):
    """
    Find the greatest 13-adjacent product in given string.

    Parameters
    ----------
    x : string
        a string that contains only digits, white spaces and line breaks.

    Returns
    -------
    tuple
        The 13-adjacent digits that generates greatest product, greatest product
    """
    import numpy as np

    string = ''.join(x.split())
    zipped = list(zip(string[:988], string[1:989], string[2:990], string[3:991], string[4:992], string[5:993], string[6:994],
                      string[7:995], string[8:996], string[9:997], string[10:998], string[11:999], string[12:]))
    digits = [list(map(int, s)) for s in zipped]

    prod_lst = []
    for lst in digits:
        prod = np.prod(lst)
        prod_lst.append(prod)
    idx = np.argmax(prod_lst)
    return (digits[idx], np.max(prod_lst))


x = '''
    73167176531330624919225119674426574742355349194934
    96983520312774506326239578318016984801869478851843
    85861560789112949495459501737958331952853208805511
    12540698747158523863050715693290963295227443043557
    66896648950445244523161731856403098711121722383113
    62229893423380308135336276614282806444486645238749
    30358907296290491560440772390713810515859307960866
    70172427121883998797908792274921901699720888093776
    65727333001053367881220235421809751254540594752243
    52584907711670556013604839586446706324415722155397
    53697817977846174064955149290862569321978468622482
    83972241375657056057490261407972968652414535100474
    82166370484403199890008895243450658541227588666881
    16427171479924442928230863465674813919123162824586
    17866458359124566529476545682848912883142607690042
    24219022671055626321111109370544217506941658960408
    07198403850962455444362981230987879927244284909188
    84580156166097919133875499200524063689912560717606
    05886116467109405077541002256983155200055935729725
    71636269561882670428252483600823257530420752963450
    '''
greatest_prod(x)

# returns ([5, 5, 7, 6, 6, 8, 9, 6, 6, 4, 8, 9, 5], 23514624000)
```


The complete solution is written in the Jupyter notebook [here](https://github.com/cassie1102/mini-project/blob/master/Euler%20Problem.ipynb).
