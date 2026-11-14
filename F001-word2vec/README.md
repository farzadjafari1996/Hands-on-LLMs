# F001 — word2vec: CBOW vs Skip-gram, and what the vectors learn about gender

**Paper:** Tomas Mikolov, Kai Chen, Greg Corrado and Jeffrey Dean (2013). *Efficient Estimation of Word Representations in Vector Space.* [arXiv:1301.3781](https://arxiv.org/abs/1301.3781)

## The claim

Two very shallow models learn high-quality word vectors from huge corpora in hours:

- **CBOW** predicts a word from its context;
- **Skip-gram** predicts the context from a word.

In the resulting vectors, relations become roughly constant offsets (*king − man + woman ≈ queen*). On the paper's 19,544-question analogy test set:

- Skip-gram is much better on **semantic** questions;
- CBOW is about as good on **syntactic** questions, and faster to train.

Accuracy keeps improving only if vector size and training data grow together.

## What this lab does

1. **Data.** Downloads `text8`, the first 100 MB of cleaned English Wikipedia (17M words). The 6B-word Google News corpus from the paper was never released. A larger corpus, `fil9` (~124M words), is a one-line option.
2. **Reproduction.** Trains four models with gensim and times each one:
   - CBOW and Skip-gram with the paper's settings: hierarchical softmax, a fixed 4+4 window for CBOW and a random window of up to 10 for Skip-gram;
   - the same two models with the later negative sampling and subsampling (Mikolov et al. 2013b).

     It then evaluates them on the paper's analogy set, split into semantic and syntactic questions (Table 3 style), with a per-category breakdown. It also runs a vector-size × data-size grid (Table 2 style) and some *king − man + woman* examples.
3. **Twist: a social-impact probe.** Builds a Bolukbasi-style (2016) *he–she* direction with PCA and projects ~55 occupation words onto it. It then asks four questions:
   - Do CBOW and Skip-gram rank jobs the same way? (Spearman ρ)
   - Which model is more biased? (*DirectBias*)
   - Do the stereotyped analogies (*man : programmer :: woman : ?*) survive when the analogy code is **allowed to return the input word**? This is the point raised by Nissim et al. (2020).
   - Are the bias rankings stable across random seeds?
4. **Your findings** prompts and **further ideas**, including the released Google News vectors, hard debiasing versus "Lipstick on a Pig", WEAT, and the link to linear directions in LLMs.

gensim's word2vec runs on the CPU and uses all threads. The GPU is not used.

## How to run

Install the packages from [`requirements.txt`](../requirements.txt).

```bash
cd F001-word2vec
jupyter lab lab.ipynb        # set QUICK_MODE at the top, then Run All
```

The notebook creates `data/` (about 130 MB), `models/` and `outputs/`, where the plots and CSVs go. All three are git-ignored.

## Runtime

With `QUICK_MODE = True` the notebook runs fast. With `QUICK_MODE = False` it runs the full lab, which takes longer. Using `CORPUS = "fil9"` takes longer still.

## Results

### Reference run

This was a full-mode run on text8 with 300-dim vectors and 3 epochs. Analogy questions were restricted to the 30k most frequent words, which covers 63% of the questions.

| Model | Semantic | Syntactic | Total |
|---|---|---|---|
| CBOW-HS (paper setup) | 16.4% | 26.8% | 23.4% |
| Skip-gram-HS (paper setup) | 47.0% | 30.7% | 36.1% |
| CBOW-NS (+ subsampling) | 18.3% | 35.5% | 29.8% |
| Skip-gram-NS (+ subsampling) | 30.1% | 34.4% | 32.9% |

For comparison, the paper's Table 3 (640-dim) reports CBOW at 24% semantic / 64% syntactic and Skip-gram at 55% / 59%.

Twist (reference run):

- DirectBias was between 0.055 and 0.076 for all four models.
- CBOW and Skip-gram ranked jobs similarly (Spearman ρ = 0.63–0.77 across models).
- Retraining CBOW with different seeds gave ρ = 0.83–0.89.
- When the analogy code was allowed to return the input word, it returned **the same job in 24 out of 24** probes for both negative-sampling models. For example, *man : programmer :: woman : ?* gives *programmer*.

### My results

*To fill in after running on my laptop.*

| Model        | Semantic | Syntactic | Total |
| ------------ | -------- | --------- | ----- |
| CBOW-HS      | 17.13    | 27.54     | 24.09 |
| Skip-gram-HS | 44.02    | 29.83     | 34.55 |
| CBOW-NS      | 19.47    | 35.59     | 30.24 |
| Skip-gram-NS | 27.57    | 32.31     | 30.74 |

**Twist findings:**

**Takeaway:**

## References

- Mikolov, Chen, Corrado & Dean (2013). *Efficient Estimation of Word Representations in Vector Space.* arXiv:1301.3781
- Mikolov, Sutskever, Chen, Corrado & Dean (2013b). *Distributed Representations of Words and Phrases and their Compositionality.* arXiv:1310.4546
- Bolukbasi, Chang, Zou, Saligrama & Kalai (2016). *Man is to Computer Programmer as Woman is to Homemaker? Debiasing Word Embeddings.* arXiv:1607.06520
- Nissim, van Noord & van der Goot (2020). *Fair is Better than Sensational: Man is to Doctor as Woman is to Doctor.* Computational Linguistics. arXiv:1905.09866
- Gonen & Goldberg (2019). *Lipstick on a Pig.* arXiv:1903.03862
