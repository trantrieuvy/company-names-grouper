# Company Names Generalisation

This project focuses on **generalising variations of company names**, a step that follows cleaning raw or “dummy” company names using a **LLM**.
The goal is to map messy, inconsistent company name formats into their unified, standardized forms.

## Project Overview

Two main approaches are implemented to achieve this:

### **1. API-based LLM Generalisation** 
[names_grouping.ipynb](https://github.com/trantrieuvy/names-generaliser/blob/main/names_grouping.ipynb)


A straightforward pipeline using **OpenAI’s API**:

1. **Cleaning:** Raw dummy company names are cleaned (e.g., removing suffixes like *GmbH*, *Ltd*, *S.A.*, etc.).
2. **Generalisation:** The cleaned names are then generalised using the LLM to produce consistent, canonical forms.

### **2. Fine-tuned LLM Approach (Unsloth + LoRA)**

A more advanced, custom solution that fine-tunes an existing instruction-tuned LLM to specialise in company name generalisation.

#### **Steps:**

1. **Data Generation:**
   Synthetic company names and their target generalised forms were generated programmatically with ChatGPT.
   Examples:
      ```python
   {"input": "Bremen Logistik GmbH", "label": "bremen"}
   {"input": "A-B-C Transport Schweiz", "label": "abc"}
   ```

3. **Fine-tuning:**
   The model was fine-tuned on **Google Colab** using **[Unsloth](https://github.com/unslothai/unsloth)** and **Low-Rank Adaptation (LoRA)** to efficiently inject task-specific knowledge into an existing LLM. The fine-tuning process and downloading the fine-tuned model can be found in this notebook: [Fine_Tuning_LLM.ipynb](https://github.com/trantrieuvy/names-generaliser/blob/main/Fine_Tuning_LLM.ipynb)

4. **Inference and Evaluation:**
   The fine-tuned model was evaluated on a test dataset to measure its accuracy and generalisation performance. The accuracy is 97.6% for our generated inference dataset. The predicted names for the inference dataset can be found at [results.csv](https://github.com/trantrieuvy/names-generaliser/blob/main/results.csv)

5. **Model Deployment:**

   * The trained model **phi-3-mini-4k-instruct.Q4_K_M.gguf** was saved locally.
   * A [Modelfile](https://github.com/trantrieuvy/names-generaliser/blob/main/Modelfile) was created for deployment.
     The Modelfile is used to create a model with Ollama:
      ```bash
      ollama create names_generaliser -f Modelfile
      ```
   * The model was loaded and served **locally via [Ollama](https://ollama.ai)**.
      ```bash
      ollama run company-generaliser
      # Input: Dopper B.V. 
      # Output: "dopper"
      ```
