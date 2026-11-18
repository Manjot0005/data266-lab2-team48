# Contributions - DATA 266 Lab 2, Part 1 (Team 48)

Team members: Manjot Kaur, Kavya Ayyappan

Manjot Kaur led Part 1. Kavya Ayyappan contributed to every task, as listed below.

## Who did what

| Task | Work items | Led by | Kavya Ayyappan's contribution |
|---|---|---|---|
| 1.1 Ingestion and indexing | PDF text extraction and cleaning; figure and table cropping with captions; trust levels from `kb_manifest.json`; figure descriptions with the vision model; ChromaDB and BM25 indexes | Manjot Kaur | Ran the full ingestion and indexing pipeline end to end on a GPU pod, set up the data folder, and checked the extraction and figure-description outputs |
| 1.2 Retrieval and grounded answers | Hybrid retrieval (dense + BM25) and exact figure lookup; fact store built from table cells; precedence rules (outturn over forecast, stated revision, conflict); citation and figure-reference checks; injection filtering for Appendix C | Manjot Kaur | Ran and tested the retrieval and answering pipeline, compared submission files across versions, and worked on the figure-reference and citation rules |
| 1.3 Conversation handling | Per-conversation memory; follow-up question rewriting; session-only user values; refusal of permanent changes to the documents | Manjot Kaur | Ran the multi-turn questions and reviewed the session-override and refusal answers in the outputs |
| 1.4 Evaluation, safety and documentation | 25-question golden set; baseline versus final comparison (answer correctness, Recall@5, citation accuracy, seconds per question); groundedness ratings by both members; safety analysis; README | Manjot Kaur | Ran the golden-set evaluation, rated the 25 answers for groundedness as rater_a, and updated the README and repository layout |
| 1.5 Kaggle submission | Kaggle notebook run at tag `p1-v1.0`; `submission.csv` validation; leaderboard submissions and final selection | Manjot Kaur | Produced and validated submission files, made leaderboard submissions, and pushed the Part 1 folder to GitHub |
