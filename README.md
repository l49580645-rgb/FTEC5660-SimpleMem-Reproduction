# SimpleMem

This project reproduces the memory retrieval, answer generation and evaluation stages of SimpleMem on a subset of the LoCoMo long-conversation question-answering dataset. Given a question about a past conversation, SimpleMem retrieves relevant memories and uses them to generate an answer. The evaluation compares that answer with the reference and records retrieval and answer-generation times separately.

## Research question

For the same 20 questions about one conversation, how does increasing the retrieval setting from `top10` to `top25` affect answer quality and retrieval time?

The two result files contain the same questions in the same order. The run labels identify retrieval configurations. The actual number of memories returned is recorded per question in `num_retrieved`. 

## Trial 3 results

| Retrieval configuration | Questions | Mean token F1 | Exact match | Mean retrieval time per question |
| ----------------------- | ---------:| -------------:| -----------:| --------------------------------:|
| `top10`                 | 20        | 0.4425        | 20% (4/20)  | 22.59 s                          |
| `top25`                 | 20        | 0.5072        | 25% (5/20)  | 20.11 s                          |

Mean token F1 measures overlap between generated and reference answers. Exact match counts answers that match their references under the evaluation script's comparison rules. On this sample, mean F1 increased by about 0.065, and one additional answer matched its reference exactly. Mean retrieval time was lower in the top25 run. Establishing a consistent speed difference would require repeated timing measurements.

## Sources and code version

- **Original paper:** [SimpleMem: Efficient Lifelong Memory for LLM Agents](https://arxiv.org/abs/2601.02553).
- **Official implementation:** [aiming-lab/SimpleMem](https://github.com/aiming-lab/SimpleMem).
- **Local run identifier:** `trial3_conv26_seed42`.
- **Base commit:** Unverified. The saved results contain no Git commit hash. If the original Colab checkout is still available and has not changed since the experiment, run the following in a Colab cell to recover the base commit and identify local modifications.

```python
!git -C /content/SimpleMem rev-parse HEAD
!git -C /content/SimpleMem status --short
```

## Running the experiment in Google Colab

The Trial 3 archive contains the two result JSON files. An exact rerun also requires the original experiment notebook, any locally modified scripts it invokes, and the model and dependency versions used in that notebook.

1. **Set up the repository.** Open the experiment notebook in Colab. Clone the official repository into `/content/SimpleMem`. If the original commit has been recovered, check it out before applying the local experiment changes. Run the notebook's dependency installation cells.
2. **Configure and test the models.** Load the DeepSeek API key from Colab Secrets and apply the generation and embedding model settings from the experiment notebook. Run the smoke test to verify that the system can store dialogue, retrieve memories, and answer a question.
3. **Prepare the evaluation sample.** Load the LoCoMo conversation and question selection associated with `trial3_conv26_seed42`. Check the 20 questions and their order against the `question` fields in the saved `detailed_results` array.
4. **Run both configurations.** Use the same conversation, questions, generation model, embedding model, and scoring procedure for both runs. Run the notebook's `top10` and `top25` configurations and save their outputs as `top10_trial3.json` and `top25_trial3.json`.
5. **Verify and summarize.** Confirm that both files contain 20 questions in identical order. Read mean F1 from `aggregated_metrics.overall.f1.mean`, exact match from `aggregated_metrics.overall.exact_match.mean`, and mean retrieval time in seconds from `summary.avg_retrieval_time`.
