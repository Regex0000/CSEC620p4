# Submission Manifest: Project 4

Fill this in before submitting. The grading agent reads it first.

## Student
- Name (RIT ID): Zachary Shrader (zjs5329)

## Course level
- [ ] 520 (undergraduate)
- [X] 620 (graduate)

## Dataset
- Name / source / version / retrieval date:
- How to obtain it (command or steps):
- Duplicate messages: removed? how many? what effect on your results?

## How to reproduce
```bash
make setup
make reproduce
```
Anything non-default the grader must know (data download, runtime): N/A, Runs as default

## Text processing
- Features used in the reported run (`config.yaml -> features`): kind, n-grams, min_df, sublinear_tf:
- Preprocessing choices and why:
- Confirm the vectorizer is fitted on training text only: [X]

## Your Naive Bayes
- Confirm `config.yaml` has `naive_bayes.implementation: scratch`: [X]
- Smoothing alpha used, and why: 0.5, small numbers reported in the alpha sweep blocked far too much ham (an 18% increase from .5 to .1), and larger numbers were far too innacurate to be useful (Less than 50% spam caught)
(`reference_check` in metrics.json):
  - `prediction_agreement`: 1.0
  - `max_abs_diff_log_prob`: 2.84e-14,

## Other classifiers
- Linear SVM: how C was chosen (training data only):C was chosen by seeking a balance between f1 improvement while not blocking too much ham. This was done by doubling until an increase in ham blocked was seen, then exploring in the range.
- KNN: k values and metric: 1,3 and 5 were used, with a cosine metric. This is as it was used in the paper.
- k-means exploration: what it showed: k-means was weak when looking for ham/spam diffrences. spam fractions for cluster 0 is still at around 8%, which is a very poor result when compraed to the Naive Bayes.

## Claimed results (must match `results/metrics.json`, protocol: ______)
| Classifier | Precision | Recall (SC%) | F1 | MCC | Blocked ham % |
|---|---|---|---|---|---|
| Naive Bayes (yours, α=0.5) | 99.66 | 68.36 | 81.10 | 80.80 | 0.03 |
| Linear SVM (C=11.25) | 98.14 | 85.45 | 91.36 | 90.55 | 0.22 |
| KNN (best k = 1) | 94.85 | 80.83 | 87.28 | 86.06 | 0.60 |
| Paper's best (SVM) | Not listed | 83.10 | Not listed | Not listed | 0.18 |

- Operating point / threshold and the cost argument behind it: Anything seen above 5% Ham blocked is seen as a detriment when looking at these figures. However, knn was taken from the paper, and linear SVM did not show a valid  decrease past a certian C value.

## Comparison to paper
- Naive Bayes: Strongest in my work, weak when compared to best results in the paper. However, the results of my NB implementation are stronger than the paper's results (SC% is higher, BH% is around the same for lower preforming NB implementations)
- Linear SVM: The implementation i this report is much weaker than those reproted in the paper. 41% blocked Ham is a very waek finding.
- KNN: The paper's findings are also much sronger, even when using the same K values.
## Exploratory task (Enron-Spam), if done (bonus: up to 10 extra points)
- Which Enron set(s) and what you did (pipeline on Enron / cross-corpus): Enron 1 was used, the test was done to compare the training of both sms to enrorn and enron to sms
- Main finding (transfer SMS <-> Enron, differences from the SMS results): The cross-corpus training was weak. enron to sms was stronger, but both were rather weak. the sms to enron was worse than a random guess. (AUC of 0.43, mcc of -.12)

## AI-use acknowledgment
Claude was used for assistance in debuggung and writing code, as well as interpreting results. The code was hard to understand without the use of claude due to a lack of full documentation for a learner of these techniques.
