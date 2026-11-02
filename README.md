# Multimodal RAG classroom project

This beginner project answers questions using three forms of evidence:

- text extracted from a PDF;
- factual captions that represent charts and diagrams;
- timestamped text from a video transcript.

It matches the practical path in `Multimodal_RAG.pptx`: create searchable representations, attach source metadata, store them in one Chroma collection, retrieve relevant evidence, optionally inspect the best retrieved image with vision, and produce a cited answer.

## Setup on Windows PowerShell

The project already has its own `.venv` and `.env`. From this folder:

```powershell
.venv\Scripts\Activate.ps1
python test_code.py
```

To recreate the environment:

```powershell
python -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Never display or commit `.env`. `.env.sample` contains safe placeholders.

## Classroom run order

```powershell
.venv\Scripts\python.exe test_code.py
.venv\Scripts\python.exe reset_local_db.py
.venv\Scripts\python.exe build_index.py
.venv\Scripts\python.exe ask.py
.venv\Scripts\python.exe evaluate.py
```

Press Enter in `ask.py` to use the cross-modal demo question. It requires evidence from both the chart and the transcript.

## What was intentionally left out

There is no notebook, web UI, live YouTube download, keyword router, fake embedding fallback, LangSmith setup, or live image-caption generation. These additions hide the core RAG flow or create unreliable classroom dependencies. Prepared captions and a prepared transcript keep the practical repeatable. The answer step still demonstrates late vision by sending at most one retrieved image to the model.

Read `STUDENT_GUIDE.md` for the design and every function. Read `TRAINER_README.md` for the teaching script and rehearsal checklist.



## Paul's contribution: cross-modal retrieval experiment

This project extension focuses on testing whether one custom question can retrieve
evidence from more than one modality.

- `ask.py`: the default demo question now asks for the leading support-resolution
  channel in 2025 and the video's reason for retrieving candidate images before
  invoking a vision model.
- `data/evaluation_questions.json`: includes the same custom cross-modal question
  with `image` and `video` as the expected source types.
- `evaluate.py`: checks whether all expected source types appear among the top
  retrieved candidates. This is a retrieval/modality check, not a full factual
  answer-quality evaluation.

The question is designed to require both:
1. the chart caption / structured facts (image evidence); and
2. the timestamped transcript (video evidence).

### Rebuild and test

After setting up `.env`, run:

```powershell
python test_code.py
python reset_local_db.py
python build_index.py
python ask.py
python evaluate.py
```

The custom question should retrieve both `image` and `video` evidence. The exact
retrieval outcome can vary with the embedding model and corpus, so use the
evaluation output to verify it rather than assuming it passed.
