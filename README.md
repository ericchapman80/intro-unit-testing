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

Start by cloning this project from GitHub:

```bash
git clone git@github.com:ericchapman80/intro-unit-testing.git
cd intro-unit-testing
```

If you already cloned the project and are already inside the `intro-unit-testing` folder, you can skip those two commands.

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
python -m pip install -r requirements.txt
```

This project already includes two starter files:

```text
interest.py
test_interest.py
```

Your folder should look like this:

```text
intro-unit-testing/
  .gitignore
  interest.py
  requirements.txt
  test_interest.py
```

The starter files are intentionally almost empty. That is part of the lesson. We will use TDD to decide what code belongs in them.

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
    principal = float(input("Principal: "))
    rate = float(input("Rate as a decimal, like 0.05 for 5%: "))
    time = float(input("Time in years: "))

    interest = calculate_interest(principal, rate, time)

    print(f"The interest is ${interest:.2f}")
```

The `input` function asks the user a question and waits for them to type an answer. The `float` function converts that answer from text into a number with decimals. For this lesson, we will keep the input code simple and focus our tests on the calculation function.

Run the program:

```bash
python interest.py
```

Type these values when the program asks:

```text
Principal: 100
Rate as a decimal, like 0.05 for 5%: 0.05
Time in years: 2
```

You should then see:

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
    principal = float(input("Principal: "))
    rate = float(input("Rate as a decimal, like 0.05 for 5%: "))
    time = float(input("Time in years: "))

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


## Adding Unit Tests to an Existing Python Assignment

You can also add unit tests to a Python program you already wrote for class. This is a common next step: first the program works, then you start moving important logic into functions so you can test it.

### 1. Open the Assignment Folder

In Terminal, go to the folder that contains the assignment:

```bash
cd path/to/your/assignment-folder
```

For example:

```bash
cd ~/projects/my-python-assignment
```

### 2. Install Pytest

If the assignment does not use a `requirements.txt` file, install `pytest` directly:

```bash
python3 -m pip install pytest
```

If that does not work, try upgrading `pip` first:

```bash
python3 -m ensurepip --upgrade
python3 -m pip install --upgrade pip
python3 -m pip install pytest
```

Use `python3 -m pip` instead of just `pip`. That helps make sure `pytest` is installed for the same Python version you are using to run the program.

### 3. Create a Test File

If your assignment file is named `assignment.py`, create a test file named:

```text
test_assignment.py
```

Pytest looks for files that start with `test_`.

### 4. Move Important Logic Into a Function

Many beginner programs start as a script, with all the code running from top to bottom. That is normal.

For example:

```python
principal = float(input("Principal: "))
rate = float(input("Rate: "))
time = float(input("Time: "))

interest = principal * rate * time
print(interest)
```

This works as a program, but it is hard to unit test because the calculation is mixed together with `input()` and `print()`.

A better testing shape is:

```python
def calculate_interest(principal, rate, time):
    return principal * rate * time


if __name__ == "__main__":
    principal = float(input("Principal: "))
    rate = float(input("Rate: "))
    time = float(input("Time: "))

    interest = calculate_interest(principal, rate, time)
    print(interest)
```

The function contains the logic we want to test. The `if __name__ == "__main__":` block contains the code that should only run when a person runs the program directly.

Indentation matters. After this line:

```python
if __name__ == "__main__":
```

The interactive code beneath it must be indented:

```python
if __name__ == "__main__":
    principal = float(input("Principal: "))
    rate = float(input("Rate: "))
    time = float(input("Time: "))
```

### 5. Write a Small Test

In `test_assignment.py`, import the function and test one expected result:

```python
from assignment import calculate_interest


def test_calculates_interest():
    result = calculate_interest(100, 0.05, 2)

    assert result == 10
```

Run the test:

```bash
python3 -m pytest
```

### Troubleshooting

If you see an error like this:

```text
OSError: pytest: reading from stdin while output is captured
```

That usually means `input()` ran while pytest was trying to import or test the file.

First, make sure your `input()` code is inside a main block:

```python
if __name__ == "__main__":
    name = input("Name: ")
```

If you are intentionally testing a program that asks for input, you can run pytest with:

```bash
python3 -m pytest -s
```

The `-s` option lets input and output happen directly in the terminal. For beginner unit testing, though, it is usually better to test functions first and keep `input()` outside the tests.

If Python says it cannot find `pytest`, try:

```bash
python3 -m pip install pytest
python3 -m pytest
```

If Python says it cannot import your function, check these things:

- The function name in the test matches the function name in the program.
- The file name in the import matches the program file name.
- The test file is in the same folder as the program file.
- The program file does not have spaces or dashes in its name.

For example, this works well:

```text
assignment.py
test_assignment.py
```

This can cause problems:

```text
my assignment.py
my-assignment.py
```

### A Good First Goal

When adding tests to an existing assignment, start small:

1. Pick one calculation or decision in the program.
2. Move it into a function.
3. Write one test for that function.
4. Run `python3 -m pytest`.
5. Add one more test when the first one passes.

You do not need to test the whole program at once. One small tested function is a great start.
