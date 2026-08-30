# Intro to Unit Testing with Python

This lesson is for a beginner who is learning Python, is new to programming, and is seeing test-driven development, or TDD, for the first time. We will use a small program that calculates simple interest so the programming idea stays focused and the math stays manageable.

We are going to build the program in a slightly different order than many beginners expect. Instead of writing the program first and checking it by hand afterward, we will write a test first. The test will describe the answer we expect. Then we will write the Python code that makes the test pass.

The simple interest formula is:

```text
I = PRT
```

Where:

- `I` is the interest
- `P` is the principal, or starting amount of money
- `R` is the interest rate
- `T` is the time

Simple interest means the interest is calculated only from the original principal. It does not add interest on top of interest. That makes it a good first example because the formula has only multiplication, and we can easily check the answer ourselves before asking Python to check it.

In the real world, someone might use `I = PRT` to estimate how much interest a savings account or loan could earn or cost over a period of time. For example, if you put `$100` into an account that earns `5%` simple interest each year for `2` years, you could use the formula to estimate how much interest you would earn. A lender could also use the same idea to estimate the interest owed on a short-term loan.

For example, if you invest `$100` at a `5%` yearly interest rate for `2` years:

```text
I = 100 * 0.05 * 2
I = 10
```

So the interest is `$10`.

In Python, percentages are usually written as decimals when we do math. So `5%` becomes `0.05`, `4%` becomes `0.04`, and `3%` becomes `0.03`.

## Goal

By the end of this lesson, a learner will be able to:

- Explain what a unit test is
- Write a test before writing the program code
- Run tests with `pytest`
- Use the red, green, refactor cycle
- Build a small Python function using TDD

## What Is a Unit Test?

A unit test checks one small piece of a program.

In this lesson, the small piece is a function that calculates interest. Instead of running the whole program and checking the answer by hand, we will write a test that says what answer we expect.

That way, Python can check our work for us.

## What Is TDD?

TDD stands for test-driven development.

The idea is:

1. Write a test for what the program should do.
2. Run the test and watch it fail.
3. Write the smallest amount of code to make the test pass.
4. Clean up the code if needed.

This is often called:

```text
Red, Green, Refactor
```

- Red means the test fails.
- Green means the test passes.
- Refactor means improve the code without changing what the code does.

The failing test is not a bad thing. It shows that the test is actually checking something.

## Project Setup

If you are starting from scratch, create a new folder for the project:

```bash
mkdir intro-unit-testing
cd intro-unit-testing
```

If this folder already exists and you are already inside it, you can skip those two commands.

Create a virtual environment:

```bash
python3 -m venv .venv
```

Turn it on:

```bash
source .venv/bin/activate
```

Install `pytest`:

```bash
python -m pip install pytest
```

Create two files:

```text
interest.py
test_interest.py
```

Your folder should look like this:

```text
intro-unit-testing/
  interest.py
  test_interest.py
```

## Step 1: Write the Test First

Open `test_interest.py`.

Before writing the program, write a test that describes what the program should do:

```python
from interest import calculate_interest


def test_calculates_simple_interest():
    principal = 100
    rate = 0.05
    time = 2

    result = calculate_interest(principal, rate, time)

    assert result == 10
```

The last line is called an assertion:

```python
assert result == 10
```

An assertion is a statement that must be true for the test to pass. This line means, "I expect `result` to be equal to `10`." If `result` is anything else, the test fails.

This test says:

- If the principal is `100`
- And the rate is `0.05`
- And the time is `2`
- Then the interest should be `10`

## Step 2: Run the Test

Run:

```bash
pytest
```

The test should fail.

That is the red step.

It probably fails because `interest.py` does not have a `calculate_interest` function yet. That is expected.

## Step 3: Write the Smallest Program Code

Open `interest.py`.

Add this code:

```python
def calculate_interest(principal, rate, time):
    return principal * rate * time
```

This function takes in the three parts of the formula:

- `principal`
- `rate`
- `time`

Then it returns the interest.

## Step 4: Run the Test Again

Run:

```bash
pytest
```

This time the test should pass.

That is the green step.

## Step 5: Add Another Test

One passing test is helpful, but a second example gives us more confidence.

Add this test to `test_interest.py`:

```python
def test_calculates_interest_for_a_different_amount():
    principal = 250
    rate = 0.04
    time = 3

    result = calculate_interest(principal, rate, time)

    assert result == 30
```

Check the math:

```text
250 * 0.04 * 3 = 30
```

Run:

```bash
pytest
```

Both tests should pass.

## Step 6: Refactor

Refactoring means improving the code without changing what it does.

You might refactor code to make it easier to read, easier to understand, or easier to change later. When refactoring, the tests should still pass because the behavior should stay the same.

Our function is already simple, but we can make it clearer by adding a docstring:

```python
def calculate_interest(principal, rate, time):
    """Calculate simple interest using I = PRT."""
    return principal * rate * time
```

Run the tests again:

```bash
pytest
```

If the tests still pass, the refactor worked.

## Step 7: Try a Tiny Program

After the function is tested, we can use it in a small program.

Update `interest.py`:

```python
def calculate_interest(principal, rate, time):
    """Calculate simple interest using I = PRT."""
    return principal * rate * time


if __name__ == "__main__":
    principal = 100
    rate = 0.05
    time = 2

    interest = calculate_interest(principal, rate, time)

    print(f"The interest is ${interest:.2f}")
```

Run the program:

```bash
python interest.py
```

You should see:

```text
The interest is $10.00
```

Then run the tests one more time:

```bash
pytest
```

The program can now be run by a person, and the important calculation can still be checked by tests.

## Full Final Code

`interest.py`:

```python
def calculate_interest(principal, rate, time):
    """Calculate simple interest using I = PRT."""
    return principal * rate * time


if __name__ == "__main__":
    principal = 100
    rate = 0.05
    time = 2

    interest = calculate_interest(principal, rate, time)

    print(f"The interest is ${interest:.2f}")
```

`test_interest.py`:

```python
from interest import calculate_interest


def test_calculates_simple_interest():
    principal = 100
    rate = 0.05
    time = 2

    result = calculate_interest(principal, rate, time)

    assert result == 10


def test_calculates_interest_for_a_different_amount():
    principal = 250
    rate = 0.04
    time = 3

    result = calculate_interest(principal, rate, time)

    assert result == 30
```

## Practice Challenges

Try these after the main lesson:

1. Add a test where the principal is `1000`, the rate is `0.03`, and the time is `5`.
2. Add a test where the time is `0`. What should the interest be?
3. Change the program values and run `python interest.py` again.
4. Add comments explaining what each test is checking.
5. Try making one test fail on purpose, then fix it.

## Discussion Questions

- Why do we write the test before the program code in TDD?
- Why is a failing test useful?
- What does the test protect us from?
- What is the difference between testing the function and running the program?
- How does `assert` help Python check our expected answer?

## Lesson Wrap-Up

Unit testing is a way to check that one small part of a program works correctly.

TDD gives us a rhythm:

```text
Red: write a test and see it fail
Green: write code and see it pass
Refactor: improve the code and keep it passing
```

For this project, the important idea is not just the formula. The important idea is learning how to prove that the formula works in code.
