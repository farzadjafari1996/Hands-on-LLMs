# paper-labs

Small, runnable reproductions of papers I read for my M.Sc. in Computer Science at HPI Potsdam. My focus is LLMs and NLP, interpretability and explainable AI, comparing models, and the social impact of ML.

Each lab is **one Jupyter notebook** that:

1. states the paper's core claim in plain words,
2. downloads public data and sets everything up,
3. reproduces the paper's key result at laptop scale,
4. runs **one "twist" experiment** the paper did not do,
5. ends with my own findings and ideas for further work.

Every notebook has a `QUICK_MODE` flag at the top. With `True` it runs fast, which is useful for checking the setup. With `False` it runs the full lab, which takes longer.

## Labs

| ID | Paper | Year | Topic | Twist | Status |
|---|---|---|---|---|---|
| [F001](F001-word2vec/) | Mikolov et al., *Efficient Estimation of Word Representations in Vector Space* ([1301.3781](https://arxiv.org/abs/1301.3781)) | 2013 | CBOW vs Skip-gram, analogy test set | Gender–occupation bias probe: CBOW vs Skip-gram, the analogy "exclusion trick", seed stability | ✅ ready |
| [I002](I002-saliency-maps/) | Simonyan, Vedaldi & Zisserman, *Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps* ([1312.6034](https://arxiv.org/abs/1312.6034)) | 2013 | Class model visualisation, gradient saliency maps | GPT-2 small text saliency for gender bias + static embedding bias check | ✅ ready |

Paper IDs follow my reading list's strands: **F** fundamentals, **I** interpretability/xAI, **A** agents, **E** evaluation, **V** vision/multimodal, **S** society.

## Repository layout

```
paper-labs/
├── README.md            # this file
├── requirements.txt     # pinned Python packages
├── .gitignore           # data/, models/, outputs/, checkpoints are never committed
├── F001-word2vec/
│   ├── README.md        # claim, what the lab does, how to run, results
│   └── lab.ipynb        # the notebook (committed with outputs cleared)
└── I002-saliency-maps/
    ├── README.md        # claim, what the lab does, how to run, results
    └── lab.ipynb        # the notebook (committed with outputs cleared)
```

Each notebook writes into its own folder:

- `data/` holds downloads;
- `models/` holds trained models;
- `outputs/` holds plots and CSV files.

All three are git-ignored and are re-created when you run the notebook.

## Setup

`requirements.txt` lists the required Python packages.

## Reproducibility

- Package versions are pinned in `requirements.txt`.
- Every notebook fixes its random seeds at the top.
- Notebooks are committed **without outputs**. Each lab's README records the results and runtimes of my full run.
