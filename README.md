# Probing Large Language Models (LLMs) and Evaluating Hallucinations

## Overview

This repository focuses on evaluating and probing Large Language Models (LLMs) to analyze their performance, identify hallucinations, and assess the effectiveness of various layers in the model. The analysis is based on the OpenHathi and LLAMA 8B models, and includes experiments with the DBpedia 14 dataset for probing LLMs.

## Hallucination Analysis

### OpenHathi

**Hallucinations (Fact-Check):**
1. [Example 1]
2. [Example 2]
3. [Example 3]

**Hallucinations (Self Consistency):**
1. [Example 1]
2. [Example 2]
3. [Example 3]

**RAG Replies:**
1. Query: "how many cm in inch?"
2. Query: "kya aam neela hota hai??"
3. Query: "which one is the most reactive element?"
4. Query: "what is the second element of periodic table"

### LLAMA 8B

**Hallucinations:**
1. Question: "What are the unique customs of the inhabitants of the planet Xylon in the Andromeda galaxy?"
   - Issue: Creating a fake planet.
2. Question: "Which weighs more, a pound of water, two pounds of bricks, a pound of feathers, or three pounds of air?"
   - Issue: Confuses weights with densities.
3. Question: "Write me a sentence without any words that appear in The Bible."
   - Issue: Unable to avoid providing false information.

**Hallucinations (Self Inconsistency):**
1. Issue: Counts different numbers of "r" in two ways.

**RAG Fixing Hallucinations:**
1. Query: "What are the unique customs of the inhabitants of the planet Xylon in the Andromeda galaxy?"
2. Query: "How does a teleportation device developed by Einstein work?"
3. Query: "Count the number of R in the word 'r a s s e r b e r y'"

### Explanation

- **OpenHathi** is a small LLM with language inconsistencies and frequent hallucinations. Responses to the same questions in different languages can yield different outputs. It also lacks knowledge of basic facts such as the most reactive element or the 2nd element in the periodic table.

- **LLAMA 8B** is a more advanced model that rarely hallucinates, except in some cases. For example, it may give different results for the same word written in different formats and has difficulty acknowledging non-existent entities.

## Report on Probing Large Language Models (LLMs)

### 1. Dataset Selection

- **Chosen Dataset:** DBpedia 14
- **Description:** DBpedia 14 contains structured information about various entities, including geographical locations, historical figures, and scientific concepts. Each entry includes fields such as population, notable achievements, and other numeric or categorical attributes.
- **Dataset Characteristics:**
  - **Fields Available:** Numeric fields (e.g., population), categorical fields (e.g., profession, gender)
  - **Sample Size:** 560 samples (1% of the full dataset)

### 2. Prompt Design

**Example Prompts:**
- For extracting information about a geographical location: "Tell me about Paris?"
- For extracting numeric data: "What is the population of Paris?"
- For extracting categorical data: "What is the predominant language spoken in Paris?"

### 3. Extraction of Embeddings

- **Model Used:** LLAMA 3 (Meta-Llama-3-8B-Instruct)
- **Process:**
  - Sent prompts to the LLAMA 3 model for a forward pass.
  - Extracted embeddings from the final token of the model’s output.
- **Embedding Details:**
  - **First Layer Embeddings:** Representations from the initial layers of the model.
  - **Middle Layer Embeddings:** Representations from intermediate layers of the model.
  - **Final Layer Embeddings:** Representations from the last layer of the model.

### 4. Linear Regression and Classification

- **Models Used:**
  - **Linear Regression:** To predict numeric fields (e.g., population).
  - **Logistic Regression:** To predict categorical fields (e.g., profession).
- **Process:**
  - Split the data into training and testing sets.
  - Trained and evaluated both regression and classification models using embeddings from different layers.
- **Evaluation Metrics:**
  - **Regression Model:** Mean Squared Error (MSE)
  - **Classification Model:** Accuracy and Classification Report

### 5. Evaluation of Probing Results

**Findings:**
- **First Layer Embeddings:** Showed initial performance; may not capture deep contextual information.
- **Middle Layer Embeddings:** Provided improved performance over the first layer, capturing more nuanced information.
- **Final Layer Embeddings:** Generally offered the best performance for both regression and classification tasks, as these embeddings represent the most refined model output.

**Performance Analysis:**
- **Regression Accuracy:** Evaluated using MSE; final layer embeddings yielded the lowest error.
- **Classification Accuracy:** Evaluated using accuracy score and classification report; final layer embeddings provided the highest accuracy and detailed classification metrics.

### 6. Discussion

**Results Reflection:**
- The final layer embeddings of LLAMA 3 proved to be the most effective for predicting numeric values and classifying categorical data, indicating that the model encodes detailed and relevant information at the final layers.

**Patterns and Anomalies:**
- **Performance Consistency:** Performance improvements from the first to final layer embeddings suggest that LLMs progressively refine their representations, making final layer embeddings more robust for prediction tasks.
- **Layer Impact:** The discrepancy in performance across different layers highlights the importance of using embeddings from deeper layers for complex tasks, as they capture more abstract and high-level features.

**Conclusion:**
- The probing analysis demonstrates that LLMs like LLAMA 3 effectively encode information about entities and topics across different layers, with final layer embeddings being the most informative for both regression and classification tasks. This reflects the model's ability to capture and retain detailed and structured information at deeper layers of its architecture.
