# Company Names Generalisation

This project focuses on **generalising variations of company names**, a step that follows cleaning raw or “dummy” company names using a **LLM**.
The goal is to map messy, inconsistent company name formats into their unified, standardized forms.

## Project Overview

Two main approaches are implemented to achieve this:

### **1. API-based LLM Generalisation**

A straightforward pipeline using **OpenAI’s API**:

1. **Cleaning:** Raw dummy company names are cleaned (e.g., removing suffixes like *GmbH*, *Ltd*, *S.A.*, etc.).
2. **Generalisation:** The cleaned names are then generalised using the LLM to produce consistent, canonical forms.

### **2. Fine-tuned LLM Approach (Unsloth + LoRA)**

A more advanced, custom solution that fine-tunes an existing instruction-tuned LLM to specialise in company name generalisation.

#### **Steps:**

1. **Data Generation:**
   Synthetic company names and their target generalised forms were generated programmatically with ChatGPT.

2. **Fine-tuning:**
   The model was fine-tuned on **Google Colab** using **[Unsloth](https://github.com/unslothai/unsloth)** and **Low-Rank Adaptation (LoRA)** to efficiently inject task-specific knowledge into an existing LLM.

3. **Inference and Evaluation:**
   The fine-tuned model was evaluated on a test dataset to measure its accuracy and generalisation performance. The accuracy is 97.6% for our generated inference dataset.

4. **Model Deployment:**

   * The trained model was **saved locally**.
   * A **modelfile** was created for deployment.
   * The model was loaded and served **locally via [Ollama](https://ollama.ai)** for fas

## Example Workflow

1. Generate dataset:

   ```python
   {"input": "Bremen Logistik GmbH", "label": "bremen"}
   {"input": "A-B-C Transport Schweiz", "label": "abc"}
   ```

2. Fine-tune model:

   ```python
   model = FastLanguageModel.get_peft_model(...)
   model.train(...)
   ```

3. Run inference locally with Ollama:

   ```bash
   ollama run company-generaliser "Generalize the company name: 'Continental Reifen Deutschland GmbH'"
   # Output: continental
   ```

---

## Project Structure

```
company-names-generalisation/
│
├── data/
│   ├── company_cleaning_minroot_1000.json (train set)
│   └── company_cleaning_minroot_val_disjoint1000.json (inference set)
│
├── Fine_Tuning_LLM.ipynb
│
├── gguf_model/
│   └── (fine-tuned model files)
│
├── Modelfile
├── requirements.txt
└── README.md
```

---

## 🧪 Results

* The fine-tuned model achieved **high consistency** on unseen company names.
* Inference via Ollama was **faster** and **more reliable** compared to API calls for small inputs.
* LoRA adaptation allowed training with **low GPU memory usage** without sacrificing performance.

---

## 📖 Future Work

* Experiment with **different LoRA ranks and target modules** for optimization.
* Extend dataset with **multilingual company names** (e.g., French, Spanish).
* Integrate into a **RAG-based system** for enterprise name standardisation.

---

## 🧑‍💻 Author

Developed by **Vy Tran**,
as part of ongoing experiments in efficient fine-tuning and deployment of lightweight LLMs.

---

Would you like me to make this version also include **badges** (e.g., Python, LoRA, Ollama, License, etc.) and **a short quickstart section** with installation and run commands? It would make it look even more professional for GitHub.
