# DATA 266 Lab 2 — Pair_Programming_Team_## 

> Replace `##` with your two-digit team number everywhere (repo name, Kaggle team, Canvas zip).

Members: Manjot Kaur, _partner name_

| Part | Notebook | Tag | Status |
|---|---|---|---|
| 1. Multimodal RAG (Kaggle) | `notebooks/p1_rag.ipynb` | `p1-v1.0` | in progress |
| 2. Diffusion LoRA fine-tuning | `notebooks/p2_finetune.ipynb` | `p2-v1.0` | in progress |
| 3. Multimodal multi-agent system | `notebooks/p3_agents.ipynb` | `p3-v1.0` | in progress |

All required work runs on a single 16 GB GPU (Kaggle T4/P100 or Colab T4).

## Setup

```bash
git clone https://github.com/<org-or-user>/data266-lab2-team##.git
cd data266-lab2-team##
pip install -r requirements.txt
```

Secrets (never commit them): set `HF_TOKEN` as an environment variable, or as a Kaggle secret / Colab secret with the same name.

## Part 1 — how to run (Kaggle)

1. Create a Kaggle notebook from `notebooks/p1_rag.ipynb`, attach the competition data, GPU T4 x2, internet ON (downloads models).
2. Write the 25-question golden set in `prompts/p1_golden_set.csv` (template created on first run) and **commit it before tuning**.
3. Run all: it builds the index, answers `QUESTIONS_PATH` (default `Questions.csv`), writes `submission.csv` and runs `validate_submission.py`.
4. Submit `submission.csv` from the Output tab. Graders re-run the tagged notebook with the hidden set via `QUESTIONS_PATH`.

## Part 2 — how to run

**RunPod (RTX 4090/5090):** clone the repo into `/workspace`, run `huggingface-cli login`, then execute
`jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=-1 notebooks/p2_finetune.ipynb`
inside `tmux` (or open it in JupyterLab and Restart & Run All). Peak GPU memory is ~3 GB, well under the 16 GB limit.


1. **Colab T4:** open `notebooks/p2_finetune.ipynb` in Colab (Runtime > Change runtime type > T4 GPU). Add Colab secrets `HF_TOKEN` and `GITHUB_TOKEN` (a GitHub personal access token with repo read/write) and set `GITHUB_REPO` in Cell 1. Cell 1 mounts Google Drive and clones the repo to `MyDrive/data266-lab2-team48`, so checkpoints and images survive a disconnect; just re-run all cells and training resumes. **Kaggle:** clone the repo and set `REPO_DIR`.
2. Set `HF_USER` in the config cell.
3. Run the notebook to the end of section 2.3 ("Write the 20 prompts"), then **commit `prompts/p2_prompts.json` and push before training finishes** (the Git timestamp is checked).
4. Run all (Restart & Run All) to produce the final executed notebook. Training takes about 1 GPU-hour per run on a T4 with the default settings.
5. After both raters + one outside rater fill in `human_eval/p2_ratings_<name>.csv`, re-run the human-evaluation section.

Models and images are pushed to Hugging Face, not GitHub:
- `HF_USER/data266-lab2-team##-p2-run1-lora-r4`
- `HF_USER/data266-lab2-team##-p2-run2-lora-r32`
- dataset `HF_USER/data266-lab2-team##-p2-images`

## Part 3 — how to run

1. Open `notebooks/p3_agents.ipynb` on Colab/Kaggle with a T4 (Part 3 installs `requirements-p3.txt` pins in its install cell).
2. It loads the final Part 2 LoRA from `HF_USER/data266-lab2-team##-p2-run1-lora-r4` (or a local `outputs/run1/final`).
3. Run the notebook up to the scenarios cell, **commit `prompts/p3_scenarios.json`**, then Restart & Run All.
4. Both members fill `human_eval/p3_judgements_<name>.csv` (copy of the template), agree on `human_eval/p3_judgements_final.csv`, and re-run the scoring cell.
5. The last cell launches the Gradio demo (`share=True` prints a public link).

## Models and APIs used (with versions)

| Part | Model | Where it runs |
|---|---|---|
| 1 | `Qwen/Qwen2.5-VL-7B-Instruct` (4-bit; fallback `Qwen2.5-VL-3B-Instruct` fp16) — figure descriptions + answers | Kaggle GPU |
| 1 | `BAAI/bge-small-en-v1.5` embeddings, ChromaDB, BM25 (`rank_bm25`); optional `BAAI/bge-reranker-base` | Kaggle GPU |
| 2 | `stable-diffusion-v1-5/stable-diffusion-v1-5` | local GPU |
| 2 | `openai/clip-vit-base-patch16` (CLIP score), Inception-v3 (KID via torchmetrics), LPIPS AlexNet | local GPU |
| 2 | `openai/clip-vit-large-patch14` (memorization check) | local GPU |

| 3 | `Qwen/Qwen2.5-VL-3B-Instruct` (4-bit NF4, fp16 fallback) — planner checks, vision, prompt writing, critique | local GPU |
| 3 | `stable-diffusion-v1-5/stable-diffusion-v1-5` img2img + Part 2 LoRA; `timbrooks/instruct-pix2pix` | local GPU |
| 3 | `openai/clip-vit-base-patch16` (critique score); SD safety checker (NSFW output filter) | local GPU |

No hosted API is used in any part. (Add any API you use in Parts 1/3 here with its exact model version.)

## Repository layout

```
notebooks/        executed notebooks, one or more per part
src/              shared helper code
prompts/          test prompts / questions, committed BEFORE tuning
eval/             metric outputs (json/csv) written by notebooks
human_eval/       blinded rating sheets and filled ratings
recordings/       links/notes for meeting recordings (large files stay outside Git)
CONTRIBUTIONS.md  who did what + list of meeting recordings
AI_USAGE.md       AI coding assistants used and for what
```
