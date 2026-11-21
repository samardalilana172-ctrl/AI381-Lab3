# Model selection

## The question
The model predicts whether a customer is likely to leave the service (churn). A wrong prediction can hurt the company by missing customers who may leave, or by spending retention offers on customers who would stay anyway.

## Candidates

| Candidate | Why I tried it | AUC | F1 | F1 per slice (lowest) | p95 latency (ms) | Size (KB) | MLflow run ID |

| Logistic Regression | Simple and lightweight baseline | 0.7982 | 0.4948 | 0.2500 | 8.4395 | 3.5479 | f3e52c06717c40ffb05ec198d1f84fcf |

| Random Forest | To test a model that can learn nonlinear patterns | 0.7589 | 0.4536 | 0.2222 | 26.2571 | 3160.0957 | 26c5a9afa3134ecbbdce9ad27bcb5765 |

| Gradient Boosting | To compare another tree-based model | 0.7476 | 0.4167 | 0.2222 | 3.9223 | 141.6445 | c31c29bdee3c4948a30681387802e272 |

## The champion
I picked Logistic Regression because it achieved the highest AUC (0.7982) and F1 (0.4948). It also has the smallest model size (3.5479 KB) and reasonable latency (8.4395 ms). I accepted that Gradient Boosting was faster (3.9223 ms), and Random Forest could learn more complex patterns, but neither performed better on our evaluation metrics.

## My promotion rules
- Latency budget (p95): **30 ms, because predictions should be fast enough for the service.**
- Minimum gain to replace the champion (F1 points): **0.02, because a small improvement may not be enough to justify replacing the current model.**

## What I would check next
I would check the model performance on different customer groups, especially groups with few customers, to make sure the model does not perform poorly for some types of customers.
