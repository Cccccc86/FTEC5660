# Validation notes

Validated on 29 September 2026 using the required model identifier
`deepseek-v4-flash-vision-exp` and the teacher's seven public receipts.

| Run | Environment | Paid total | Total without discounts | Result |
| --- | --- | --- | --- | --- |
| Final run 1 | Development virtual environment | HK$1974.30 | HK$2348.20 | Both correct |
| Final run 2 | New independent calls, same environment | HK$1974.30 | HK$2348.20 | Both correct |
| Final run 3 | Fresh local Git clone and newly installed virtual environment | HK$1974.30 | HK$2348.20 | Both correct |

The fresh-clone test ran the unmodified command-line entry point:

```bash
python hw1.py --image-folder public_test
```

It exited successfully and wrote the required `results.csv`. The fresh clone
was made from a local validation repository containing the submission files;
this does not claim that the GitHub repository was already updated.

The first two final runs each used eight logical model calls for seven receipts
(one receipt required a second reading). Their elapsed times were 9.52 and
8.61 seconds in the local environment. The clean-clone run took 12.88 seconds.
These times are observations, not performance guarantees.

Before adding enlarged views, one development run returned HK$1974.30 and
HK$2347.20. The second result was wrong: a six-dollar discount was read as
five dollars. Feedback alone repeated the misreading; a second pass with
overlapping enlarged views resolved it. No filenames or public answers were
added to the implementation.

Fifteen offline tests also checked output formatting, Decimal aggregation,
positive and negative rounding, multiple discounts, cash tender versus net
payment, malformed JSON, bounded retry, missing key, failure-to-CSV handling,
and the actual LangChain request format through a mock HTTP transport. One
test covered all 127 non-empty combinations of the seven public receipts using
mock extraction records. This checks aggregation only; it is not an OCR
accuracy result.

The final implementation only changes `build_chain()` and `answer_queries()`
in the original `hw1.py`; everything outside those functions was compared
against the teacher's template. Pillow is the only added direct dependency.

Three successful public runs do not predict a private-test score. The same
model can still misread an image consistently, including in a way that passes
the arithmetic checks. No private test data were available.
