# FTEC5660 Homework 1

Chen Youxin / 陈由欣 · 1155274414

## Homework 1 solution

![Receipt extraction and checking workflow](chain_design.svg)

I read each receipt independently with a LangChain prompt, the required
`deepseek-v4-flash-vision-exp` model and a text output parser. The prompt asks
for item line totals, discounts, subtotal, rounding and final payment as JSON.
It also explains why cash tendered is different from the amount spent and why
rounding is not a discount. Python uses `Decimal` to check two equations:
items minus discounts equals subtotal, and subtotal plus rounding equals
payment. Even a one-cent difference triggers a second reading, with feedback
and overlapping enlarged views of the receipt. Up to two receipts run in
parallel, and each gets at most one additional extraction attempt. Python
then adds the payments for question 1 and the original item line totals for
question 2, as defined in the assignment. Discounts provide an arithmetic
cross-check rather than determining the second answer. Only
`build_chain()` and `answer_queries()` were changed in the starter code.

## Run the public test

Use Python 3.10 or later. Create a virtual environment:

```bash
python -m venv .venv
```

Activate it with `.venv\Scripts\Activate.ps1` in Windows PowerShell, or
`source .venv/bin/activate` on macOS/Linux, then install the dependencies:

```bash
python -m pip install -r requirements.txt
```

Create a local `.env` file containing `DEEPSEEK_API_KEY=your_key_here`, or set
that environment variable. Keep the real key out of GitHub. From the repository
root, run:

```bash
python hw1.py --image-folder public_test
```

The supplied runner writes `results.csv`. Check **both** rows: each should
contain one HKD amount and have `correct` in the `correctness` column. The chain
does not read the ground-truth file; the supplied runner uses it for scoring.
See [VALIDATION.md](VALIDATION.md) for the test results.

## What still can go wrong

In testing, the model sometimes read a six-dollar discount as five dollars,
even after a second reading with enlarged views. Summing original item prices
avoids passing that particular discount error into the second answer.
Arithmetic checks cannot catch every misreading, especially if several wrong
numbers happen to balance.

If both readings have issues, the code keeps the one with fewer failed checks
and prints a warning. If no usable reading is available, it returns a failure
message rather than a partial folder total. A CSV containing that message is
not a successful test. Passing the public receipts does not guarantee the
same result on unseen images.
