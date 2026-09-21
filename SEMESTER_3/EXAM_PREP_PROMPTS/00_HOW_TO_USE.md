# Semester 3 — Exam-Prep DOCX Master Prompts (B.S. CSDA, IIT Patna)

These four prompt files each produce **one complete, self-sufficient .docx exam-preparation guide** for one Semester-3 course.
The exam is a **CBT (computer-based test) with MCQs only**, so every guide is built around fast recall, calculation speed,
trap-awareness and large volumes of MCQ practice, while still teaching every concept from zero.

| # | File | Course | Output .docx |
|---|------|--------|--------------|
| 1 | `01_CDA201_Statistics_Master_Prompt.md` | BO CDA 201 — Statistics for Data Science (3-1-2-5) | `CDA201_Statistics_Exam_Guide.docx` |
| 2 | `02_CDA203_Algorithms_Master_Prompt.md` | BO CDA 203 — Design of Algorithms (3-1-2-5) | `CDA203_Algorithms_Exam_Guide.docx` |
| 3 | `03_CDA205_Machine_Learning_Master_Prompt.md` | BO CDA 205 — Machine Learning Techniques (3-1-2-5) | `CDA205_ML_Exam_Guide.docx` |
| 4 | `04_CDA207_Financial_Economics_Master_Prompt.md` | BO CDA 207 — Financial Economics (3-1-0-4) | `CDA207_FinEcon_Exam_Guide.docx` |

## How to use (one subject at a time)

1. Open a **fresh** Claude Code session with the working directory `/Users/aditya/STUDY/IIT_Patna_NOTES/SEMESTER_3`
   (so the AI can open the lecture PDFs and books itself).
2. Paste the **entire** content of one prompt file (for example, `01_...md`) as your message.
3. The AI builds the guide **unit by unit, in syllabus order**, and checks in after each unit. Reply `continue` to move on.
   (Each guide runs to hundreds of pages, so building it in stages keeps quality high and stops content being cut off.)
4. At the end, the AI stitches everything into one final `.docx`, adds the TOC, the formula sheet and mock tests, and runs the QA checklist.
5. Repeat with the next prompt file.

## What each prompt already contains (so the AI does not need to guess)

- The **official syllabus**, in its exact order. Guides must follow that order and must not skip or reorder topics.
- A **lecture-by-lecture digest** of what your professor actually taught (dates, examples, notation, solved questions),
  written after reading every page of every lecture PDF, including the handwritten 201 and 203 notes.
- An **errata list**: mistakes found in the lecture materials that the guide must correct, not copy.
- A **textbook map** from each syllabus topic to chapters and sections in the PDFs in your folder, plus warnings about mislabeled PDFs.
- A **"not yet taught"** list, so topics the professor hasn't reached yet are still covered in full from the textbooks.
- A strict **unit template**: concepts, examples, exercises, practice MCQs, flowcharts, diagrams, tables, traps, formula boxes and answer keys.
- **DOCX build specs**: styles, equations, images, TOC, page setup, and a QA checklist that includes checking every numeric answer with code.

## Where the source material lives

```
SEMESTER_3/
├── s-main.png, S-1.png, s-2.png          ← official curriculum and syllabus pages
├── 201/  (Statistics)  Lecture Notes-20260921/{Lectures/Lecture-1..7.pdf, Lab Tutorial/, Books/Book-1..3.pdf, T/F/Chi-square tables}
│         + Ramachandran–Tsokos, Hines–Montgomery, Hogg–McKean–Craig PDFs
├── 203/  (Algorithms)  Lecture Notes-20260921 (1)/Lec 1-2 … Lec 15 Tute 5.pdf  + Weiss, Kleinberg–Tardos, CLRS-IM, "Aho" PDFs
├── 205/  (ML)          Lecture Notes-20260921 (2)/{1 Intro kNN- Naive Bayes.pdf, StatQuest guide, Lab-Assignment-1/2.docx, DecisionTree assignment}
│         + Bishop, ESL, Mitchell PDFs
└── 207/  (FinEcon)     Lecture Notes-20260921 (3)/{Evolution, Indian Financial System, Forms of Business, Formation of Company, Information Asymmetry}
          + Hull F&O, Luenberger, LeRoy–Werner, Merton RFBR PDFs
```

Snapshot date of the lecture material: **21 Sep 2026**. When new lectures are added, re-run the relevant prompt and tell the AI to
"also read the new lecture files and update the professor digest".
