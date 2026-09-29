# Validation notes

Tested on 29 September 2026 with `deepseek-v4-flash-vision-exp` and the seven
receipts supplied with the homework.

| Final version | Paid total | Total without discounts | Result |
| --- | --- | --- | --- |
| Run 1 | HK$1974.30 | HK$2348.20 | Both correct |
| Run 2 | HK$1974.30 | HK$2348.20 | Both correct |
| Run 3, fresh local Git clone | HK$1974.30 | HK$2348.20 | Both correct |

[public_test_results.csv](public_test_results.csv) is the unedited CSV from
run 3. That run used the existing Python environment and the original command:

```bash
python hw1.py --image-folder public_test
```

It exited successfully. A separate attempt to install dependencies into a new
virtual environment timed out, so this is a fresh-clone check, not a successful
new-environment installation check. The clone was local, not downloaded from
the updated GitHub repository.

A test before the final change returned HK$2347.20 for question 2. The model
read a six-dollar discount as five dollars, and rereading with enlarged views
still sometimes repeated it. Trying six smaller crops did not fix that run.
The final version sums the original item prices, as the assignment defines
the undiscounted amount, and uses discounts to cross-check the reading. It
does not change a transcribed number to make the arithmetic balance. Run 1
still printed an inconsistency warning even though both final answers were
correct; this remains a limitation.

All 18 offline checks passed, including one-cent discrepancies, rounding,
malformed JSON, a repeated discount misreading, failure handling and the
LangChain request format. Mock receipt records also checked aggregation over
all 127 non-empty public-receipt subsets; this is not an OCR accuracy test.
A comparison with the starter confirmed that only `build_chain()` and
`answer_queries()` changed. Three correct public runs do not establish
accuracy on unseen receipts.
