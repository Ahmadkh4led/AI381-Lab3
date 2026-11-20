# Model selection

## The question

The model predicts whether a customer is likely to leave the company (churn).
A wrong prediction can cause the company to lose customers or waste money on unnecessary retention offers.

## Candidates

| Candidate | Why I tried it | AUC | F1 | F1 per slice (lowest) | p95 latency (ms) | Size (KB) | MLflow run ID |
|---|---|---:|---:|---:|---:|---:|---|
| Logistic Regression | Simple, fast baseline model | 0.798 | 0.495 | 0.250 (one-year, two-year) | 9.163 | 3.548 | 75b3be3f93c04a6e9976ad71530ff510 |
| Random Forest | To capture more complex patterns | 0.759 | 0.454 | 0.222 (one-year) | 21.560 | 3160.1 | 4d0241e5dfff431288b8269618a9d489 |
| Gradient Boosting | To test a boosting-based approach | 0.748 | 0.417 | 0.222 (two-year) | 8.264 | 141.7 | 0914c75b3d384c89b8d9e29fcaea51ef |

## The champion

I selected Logistic Regression because it achieved the highest overall F1 (0.495) and AUC (0.798). It also had the smallest model size (3.548 KB) and acceptable prediction latency (9.163 ms).

I accepted that Gradient Boosting was slightly faster and performed better on the one-year contract slice, while Random Forest performed better on the two-year contract slice.

## My promotion rules

- Latency budget (p95): 25 ms, because all three candidates meet this limit and it keeps prediction time low.
- Minimum gain to replace the champion (F1 points): 0.005, because a new model should show a measurable improvement before replacing the current champion.

## What I would check next

I would investigate the F1 scores for each contract type, especially the one-year and two-year groups, and check whether the model performs consistently on new customer data.
