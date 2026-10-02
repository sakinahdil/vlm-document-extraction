

# Document Information Extraction using Vision-Language Models

A proof-of-concept system that automatically extracts key information from document images (receipts, invoices, and bills) using a Vision-Language Model (VLM), and returns the results as structured, machine-readable JSON.

This project was developed as part of a summer internship with the School of Computer & Mathematical Sciences, University of Nottingham Malaysia.

---

## Overview

Organisations often need to extract information from documents such as invoices, receipts, and forms. Doing this manually is slow, expensive, error-prone, and does not scale with document volume. This project automates that task: a user uploads a document image, a Vision-Language Model reads it, and the key fields are returned as structured JSON that can be saved, searched, or integrated into other systems.

The project also includes an experimental evaluation comparing two VLMs — **Qwen-VL** (the selected model) against **LLaVA-1.5** (a baseline) — to justify the model choice with evidence.

---

## Features

- Upload a document as an image (JPG, PNG) or PDF (PDFs are converted to images automatically)
- Automatic extraction of key fields: **vendor name, date, invoice/receipt number, total amount, and currency**
- Results displayed in a clean field-by-field table alongside a preview of the uploaded document
- Download the extracted data as a structured **JSON** file
- A simple web interface built with Gradio, shareable via a public link

---

## Technology Stack

- **Programming Language:** Python
- **Development Environment:** Google Colab (cloud-based Jupyter Notebook, used to access GPU resources beyond local hardware limits)
- **Machine Learning Frameworks:** PyTorch, Hugging Face Transformers
- **Primary Model:** Qwen2-VL-7B-Instruct
- **Baseline Model (evaluation only):** LLaVA-1.5-7B
- **Optimisation:** 4-bit quantisation (via bitsandbytes) to run the models within available GPU memory
- **Supporting Libraries:** Pillow, pdf2image, Pandas
- **Interface:** Gradio
- **Version Control:** Git and GitHub

---

## How to Run

The project runs in Google Colab with a GPU runtime.

1. Open the notebook (`vlm_document_extraction.ipynb`) in Google Colab.
2. Set the runtime to a GPU: **Runtime → Change runtime type → GPU**.
3. Run the cells in order. This will:
   - Install the required libraries
   - Load the Qwen-VL model (with 4-bit quantisation)
   - Define the extraction functions
   - Launch the Gradio web interface
4. When the interface launches, a public link (ending in `.gradio.live`) is generated. Open it in a browser.
5. Upload a document, click **Extract Information**, and view / download the extracted fields.

> Note: The Gradio public link is temporary and remains active only while the Colab session is running.

---

## Evaluation Summary

Both models were evaluated on an 18-document test set: 14 receipts from the standard **SROIE** dataset plus 4 varied real-world documents (a utility bill, a formal payment receipt, and restaurant receipts). Both models received the same images and prompts, and were scored on field accuracy (value correctness) and valid-JSON rate.

| Metric | Qwen-VL | LLaVA-1.5 |
|---|---|---|
| Valid JSON rate | 18/18 (100%) | 0/18 (0%) |
| Field accuracy | 89/90 (98.9%) | ~25% |

Qwen-VL substantially outperformed LLaVA-1.5. Notably, LLaVA scored 0% on fields requiring precise text reading (vendor name, invoice number) but ~77% on currency (a more guessable field), indicating it tends to generate plausible answers rather than accurately read the document. Qwen-VL's single error was a minor single-character OCR misread, not a hallucination. These findings confirmed the prediction from the literature review and justified the selection of Qwen-VL for the prototype.

---

## Limitations and Future Work

- The evaluation used a relatively small test set (18 documents), predominantly Malaysian retail receipts; results may not fully generalise to other document types or regions.
- Accuracy scoring was performed manually.
- Only a single baseline (LLaVA-1.5) was tested, without extensive prompt-tuning.
- The system currently processes the first page of multi-page PDFs only.
- **Future work:** line-item extraction (individual products, quantities, prices); support for multi-page documents; a larger and more varied evaluation set; permanent hosting for the web interface.

---

## Project Deliverables

This repository forms part of a larger internship project that also includes a literature review, a use-case analysis, and an experimental evaluation report.

---

## Author

Fathima Sakinah Dil Fairaz — University of Nottingham Malaysia
Supervisor: Dr. Tissa Chandesa
