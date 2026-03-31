# Model Card: Mood Machine — ML Model (`ml_experiments.py`)

## Model Overview

| Field | Details |
|---|---|
| **Model name** | Mood Machine ML Classifier |
| **Model type** | Logistic Regression on bag-of-words features |
| **Task** | Sentiment classification of short social media-style text |
| **Output labels** | `positive`, `negative`, `neutral`, `mixed` |
| **Framework** | scikit-learn (`CountVectorizer` + `LogisticRegression`) |
| **Intended use** | Educational / lab demo only |

---

## How It Works

1. **Vectorization** — Each input sentence is converted into a word-count vector using `CountVectorizer`. Every unique word in the training data becomes a feature; its value is how many times it appears in the sentence.

2. **Training** — A `LogisticRegression` model is fit on those vectors and their human-assigned labels from `dataset.py`. The model learns which words tend to appear in positive vs. negative vs. neutral posts.

3. **Prediction** — A new sentence is transformed the same way, then the model outputs whichever label its math says is most likely.

This is different from the rule-based model (`mood_analyzer.py`), which uses hand-crafted word lists. This model infers its own word associations from the examples.

---

## Training Data

- **Source:** `SAMPLE_POSTS` and `TRUE_LABELS` in `dataset.py`
- **Size:** ~13 examples (small by design — this is a learning lab)
- **Labels:** Assigned by hand by the student author
- **Label distribution:** Varies depending on what examples were added

### Known data limitations

- The dataset is tiny. A real sentiment classifier uses thousands or millions of examples.
- Labels are subjective. Sarcasm, slang, and mixed-feeling posts are hard to label consistently.
- The model is evaluated on the **same data it was trained on** (no held-out test set), so reported accuracy is optimistic and does not reflect real-world performance.

---

## Performance

Accuracy is measured on the training set itself (not a separate test set). Expect near-100% accuracy on seen examples, but unknown performance on new sentences.

To see current accuracy, run:
```
python ml_experiments.py
```

---

## Limitations

| Limitation | Example |
|---|---|
| **Sarcasm** | "I absolutely love getting stuck in traffic" — likely predicted positive |
| **Negation** | "not happy" — `not` and `happy` are treated as separate words |
| **Slang** | "lowkey stressed" — may not appear in training data |
| **Emojis** | "😂💀" — not tokenized; ignored by `CountVectorizer` by default |
| **Tiny training set** | Rare words or new phrasing will not be recognized |
| **No generalization metric** | Accuracy reported on training data only |

---

## Intended Use

- **Appropriate:** Learning how bag-of-words + logistic regression works; comparing ML vs. rule-based approaches in a classroom setting.
- **Not appropriate:** Any real-world application, content moderation, mental health assessment, or deployment of any kind.

---

## Comparison with Rule-Based Model

| | Rule-Based (`mood_analyzer.py`) | ML Model (`ml_experiments.py`) |
|---|---|---|
| How rules are defined | Hand-written word lists | Learned from labeled examples |
| Interpretability | High — you can read the rules | Lower — weights are implicit |
| Improves with more data | No | Yes |
| Handles unseen slang | Only if you add it manually | Only if it appeared in training |
| Accuracy on training data | Depends on implementation | Typically high (overfits small data) |
