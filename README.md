# CS-61-A-structure-and-Interpretation-of-Computer-Programs-Homework-1

This is the first homework of CS 61a taught by UC Berkeley for the Fall 2020 Semester

Q2: A Plus Abs B
Fill in the blanks in the following function for adding a to the absolute value of b, without calling abs. You may not modify any of the provided code other than the two blanks.

from operator import add, sub

def a_plus_abs_b(a, b):
    """Return a+abs(b), but without calling abs.

    >>> a_plus_abs_b(2, 3)
    5
    >>> a_plus_abs_b(2, -3)
    5
    >>> # a check that you didn't change the return statement!
    >>> import inspect, re
    >>> re.findall(r'^\s*(return .*)', inspect.getsource(a_plus_abs_b), re.M)
    ['return f(a, b)']
    """
    if b < 0:
        f = sub
    else:
        f = add
    return f(a, b)
Use Ok to test your code:

python3 ok -q a_plus_abs_b
If b is positive, we add the numbers together. If b is negative, we subtract the numbers. Therefore, we choose the operator add or sub based on the sign of b.

Video walkthrough: https://youtu.be/o9eUNrWTr3I

Q3: Two of Three
Write a function that takes three positive numbers as arguments and returns the sum of the squares of the two smallest numbers. Use only a single line for the body of the function.

def two_of_three(x, y, z):
    """Return a*a + b*b, where a and b are the two smallest members of the
    positive numbers x, y, and z.

    >>> two_of_three(1, 2, 3)
    5
    >>> two_of_three(5, 3, 1)
    10
    >>> two_of_three(10, 2, 8)
    68
    >>> two_of_three(5, 5, 5)
    50
    >>> # check that your code consists of nothing but an expression (this docstring)
    >>> # a return statement
    >>> import inspect, ast
    >>> [type(x).__name__ for x in ast.parse(inspect.getsource(two_of_three)).body[0].body]
    ['Expr', 'Return']
    """
    return min(x*x+y*y, x*x+z*z, y*y+z*z)
    # Alternate solution
def two_of_three_alternate(x, y, z):
    return x**2 + y**2 + z**2 - max(x, y, z)**2
Hint: Consider using the max or min function:

>>> max(1, 2, 3)
3
>>> min(-1, -2, -3)
-3
Use Ok to test your code:

python3 ok -q two_of_three
We use the fact that if x>y and y>0, then square(x)>square(y). So, we can take the min of the sum of squares of all pairs. The min function can take an arbitrary number of arguments.

Alternatively, we can do the sum of squares of all the numbers. Then we pick the largest value, and subtract the square of that.

Video walkthrough: https://youtu.be/oPN3OCGGb4M

Q4: Largest Factor
Write a function that takes an integer n that is greater than 1 and returns the largest integer that is smaller than n and evenly divides n.

def largest_factor(n):
    """Return the largest factor of n that is smaller than n.

    >>> largest_factor(15) # factors are 1, 3, 5
    5
    >>> largest_factor(80) # factors are 1, 2, 4, 5, 8, 10, 16, 20, 40
    40
    >>> largest_factor(13) # factor is 1 since 13 is prime
    1
    """
    factor = n - 1
    while factor > 0:
        if n % factor == 0:
            return factor
        factor -= 1
Hint: To check if b evenly divides a, you can use the expression a % b == 0, which can be read as, "the remainder of dividing a by b is 0."

Use Ok to test your code:

python3 ok -q largest_factor
Iterating from n-1 to 1, we return the first integer that evenly divides n. This is guaranteed to be the largest factor of n.

Video walkthrough: https://youtu.be/pVgxbeL4DHQ

Q5: If Function vs Statement
Let's try to write a function that does the same thing as an if statement.

def if_function(condition, true_result, false_result):
    """Return true_result if condition is a true value, and
    false_result otherwise.

    >>> if_function(True, 2, 3)
    2
    >>> if_function(False, 2, 3)
    3
    >>> if_function(3==2, 3+2, 3-2)
    1
    >>> if_function(3>2, 3+2, 3-2)
    5
    """
    if condition:
        return true_result
    else:
        return false_result
Despite the doctests above, this function actually does not do the same thing as an if statement in all cases. To prove this fact, write functions cond, true_func, and false_func such that with_if_statement prints the number 47, but with_if_function prints both 42 and 47.

def with_if_statement():
    """
    >>> result = with_if_statement()
    47
    >>> print(result)
    None
    """
    if cond():
        return true_func()
    else:
        return false_func()

def with_if_function():
    """
    >>> result = with_if_function()
    42
    47
    >>> print(result)
    None
    """
    return if_function(cond(), true_func(), false_func())

def cond():
    return False

def true_func():
    print(42)

def false_func():
    print(47)
Hint: If you are having a hard time identifying how an if statement and if_function differ, consider the rules of evaluation for if statements and call expressions.

Use Ok to test your code:

python3 ok -q with_if_statement
python3 ok -q with_if_function
The function with_if_function uses a call expression, which guarantees that all of its operand subexpressions will be evaluated before if_function is applied to the resulting arguments.

Therefore, even if cond returns False, the function true_func will be called. When we call true_func, we print out 42. Then, when we call false_func, we will also print 47.

By contrast, with_if_statement will never call true_func if cond returns False. Thus, we will only call false_func, printing 47.

Q6: Hailstone
Douglas Hofstadter's Pulitzer-prize-winning book, Gödel, Escher, Bach, poses the following mathematical puzzle.

Pick a positive integer n as the start.
If n is even, divide it by 2.
If n is odd, multiply it by 3 and add 1.
Continue this process until n is 1.
The number n will travel up and down but eventually end at 1 (at least for all numbers that have ever been tried -- nobody has ever proved that the sequence will terminate). Analogously, a hailstone travels up and down in the atmosphere before eventually landing on earth.

Breaking News (or at least the closest thing to that in math). There was a recent development in the hailstone conjecture last year that shows that almost all numbers will eventually get to 1 if you repeat this process. This isn't a complete proof but a major breakthrough.

This sequence of values of n is often called a Hailstone sequence. Write a function that takes a single argument with formal parameter name n, prints out the hailstone sequence starting at n, and returns the number of steps in the sequence:

def hailstone(n):
    """Print the hailstone sequence starting at n and return its
    length.

    >>> a = hailstone(10)
    10
    5
    16
    8
    4
    2
    1
    >>> a
    7
    """
    length = 1
    while n != 1:
        print(n)
        if n % 2 == 0:
            n = n // 2      # Integer division prevents "1.0" output
        else:
            n = 3 * n + 1
        length = length + 1
    print(n)                # n is now 1
    return length
Hailstone sequences can get quite long! Try 27. What's the longest you can find?

Use Ok to test your code:

python3 ok -q hailstone
We keep track of the current length of the hailstone sequence and the current value of the hailstone sequence. From there, we loop until we hit the end of the sequence, updating the length in each step.

Note: we need to do floor division // to remove decimals.

