# Diagrams — Admissions & Student Services Assistant

These are simple diagrams to help a beginner see how the product works. They match the spec in `docs/03-spec.md`. No image files are used — the diagrams below are written in Mermaid text, which GitHub renders as a picture.

## 1. How it works

```mermaid
flowchart TD
    A["Student asks a question"] --> B["Find approved sources"]
    B --> C["Draft an answer from the sources only"]
    C --> D{"Is the question covered by a source?"}
    D -- Yes --> E["Show the answer with source and date"]
    D -- No --> F["Say I do not know"]
    F --> G["Offer a human handoff"]
    E --> H["Staff review in Stage 1"]
```

What this shows: the assistant never answers from memory alone. It first finds an approved document, then answers from it. If there is no source, it refuses safely and offers a human.

## 2. User journey

```mermaid
flowchart TD
    A["Prospective student has a question"] --> B["Student asks the assistant"]
    B --> C["Assistant answers with source and date"]
    C --> D["Student checks the date and source"]
    D --> E["Student follows the correct deadline"]
    E --> F["Student applies on time"]
    F --> G["Staff spend time on hard cases only"]
```

What this shows: before the product, the student got different answers from different people. After it, the student gets one clear answer and can check where it came from.

## 3. Eval and release flow

```mermaid
flowchart TD
    A["Build the 60-question test set"] --> B["Run the eval"]
    B --> C{"Pass bar met?"}
    C -- No --> D["Fix the sources or the rules"]
    D --> B
    C -- Yes --> E["Stage 1: staff only, 100 percent review"]
    E --> F["Stage 2: students, 20 percent sampled daily"]
    F --> G["Stage 3: full admissions season if eval stays above the bar for 7 days"]
```

What this shows: the product is tested before real students use it, and it is released in stages. It only reaches full release if the live eval stays above the pass bar.
