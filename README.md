# Semantic NLP Filtering for Deep Learning Papers in Virology/Epidemiology


This project leverages large language models (LLMs) to automate the processing of academic papers, specifically focusing on three key tasks:

1. **Semantic NLP Filtering**
2. **Classification of Method Type**
3. **Extraction of Methods**
   
The goal is to streamline the process of academic paper analysis, saving time and resources while ensuring high-quality insights.

---

### Project Overview

The project consists of three main tasks:

1. **Semantic NLP Filtering**: Identifying papers that are relevant to deep learning applications in virology and epidemiology.
2. **Classification of Method Type**: Categorizing relevant papers into one of four categories: ["text mining", "computer vision", "both", "other"] based on the methodology used.
3. **Extraction of Methods**: Extracting and reporting the names of the specific methods or models used in the relevant papers.

---

### Methodology

**Task 1:**  Filtering Papers Based on Deep Learning Usage
- Input: For each paper, the title and abstract are provided to the LLM, along with a prompt asking whether the paper uses deep learning models.
- Approach: Five variations of the prompt were designed to ensure robustness and capture diverse interpretations of deep learning terminology by the LLM.
- Output: The LLM returns the response: either "Yes," indicating that the paper uses deep learning, or "No," indicating that it does not.


**Task 2:** Categorizing Papers into Predefined Groups
- Input: The LLM receives the title, abstract, and a prompt requesting the categorization of the paper into one of the following groups:
  - text_mining
  - computer_vision
  - both (indicating that the paper involves both text mining and computer vision)
  - other (for papers that do not fit into any of the previous categories)
- Approach: Five distinct prompts were created to describe and classify papers.
- Output: The LLM returns one of the four categories or NaN if it is unable to confidently categorize the paper.


**Task 3:** Extracting Model Names from Paper
- Input: For this task, the LLM is provided with the title, abstract, and a prompt asking it to extract the names of deep learning models used in the paper.
- Challenge: The LLM’s response can be a string containing model names or additional explanatory text. This creates ambiguity in processing, as sometimes the LLM returns more information than required.
- Approach: The responses are post-processed and compared to the responses from different prompts to isolate model names. 
- Output: The LLM provides a string of text that may contain the names of one or more models used in the paper.

#### **Note:**
###### Script ***_filter_papers_task1.ipynb_*** contains the whole code for **task 1**, Semantic NLP Filtering.
###### Script ***_classification_task2.ipynb_*** contains the whole code for **task 2**, Classification of Methods.
###### Script ***_extract_model_name_task3.ipynb_*** contains the whole code for **task 3**, Extraction of Methods.
---

### Prompt Templates 
Prompt engineering is central to guiding the LLM’s responses, as it clarifies task objectives, reduces ambiguity, and shapes the output format. Here are the specific techniques and considerations used.
For each task— filtering, classification, and extraction—separate prompts were crafted with explicit instructions. This modular design helped compartmentalize complex tasks, improving clarity and the relevance of LLM responses. Below, you can find more details on designed prompts.<be>

<br>

**Prompts Used for Task 1**

| **Prompt Number** | **Prompt Template** | **Description** | **Example** | **Performance** |
|-------------------|---------------------|-----------------|-------------|-----------------|
| **Prompt 1**      | Basic relevance classification | Asks the model to classify paper relevance based on title and abstract | "Given the title and abstract of a research paper, classify whether it is relevant to deep learning applications. <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | Normal |
| **Prompt 2**      | Expert-based relevance | Emphasizes expertise in deep learning for classification | "You are an expert in deep learning. Based on the following title and abstract, determine if the paper contributes to the field of deep learning.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | **[High]** |
| **Prompt 3**      | Simple yes/no decision | Requests a binary decision on relevance based on deep learning mention | "Analyze the following title and abstract to decide if it addresses deep learning methodologies.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | [Moderate] |
| **Prompt 4**      | Example-based guidance | Provides examples to guide judgment on relevance | "Read the title and abstract below and determine if they pertain to deep learning. Use 'yes' if they do and 'no' if they do not.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | [Normal] |
| **Prompt 5**      | Summarization-based judgment | Encourages summarization before making a relevant decision | "First summarize the main focus, then decide if it is related to deep learning.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | [Moderate] |

#### **Explanation:**
Using multiple similar prompts allowed for a robust evaluation by testing different approaches to detecting deep learning relevance. The AI was guided by explicit instructions, examples, and summarization techniques, ensuring accuracy and consistency in filtering out irrelevant papers. Here, prompts focus on binary classification ("yes" or "no") to decide whether a paper is relevant. By keeping the prompt output simple, we tried to minimize the risk of unnecessary information while making it easier for the model to focus on keywords like "neural networks," "deep learning," and domain-specific terms.

<br>

 **Prompts Used for Task 2**

| **Prompt Number** | **Prompt Template** | **Description** | **Example** | **Performance** |
|-------------------|---------------------|-----------------|-------------|-----------------|
| **Prompt 1**      | Direct classification | Designed for straightforward classification based on title and abstract | "Classify the following research paper based on the type of method used.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | Normal |
| **Prompt 2**      | Emphasized categorization | Similar to Prompt 1 but focusing on categorizing the method | "Based on the title and abstract, determine the primary method used in this research paper.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | Normal |
| **Prompt 3**      | Precision-focused classification | Offers instructions to classify based on research method with a focus on precision | "Read the title and abstract and classify it according to the method used.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | Normal |
| **Prompt 4**      | Contextualized classification | Explains classification context with examples of possible methods | "Analyze the title and abstract below and classify them based on the type of method employed.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | Normal |
| **Prompt 5**      | Direct classification request | Directly requests classification, focusing on identifying the most relevant category | "Classify the following title and abstract into one of these categories.  <br>Title: `{title}` <br>Abstract:`{abstract}` <br>Decision:" | Normal |


#### **Explanation**:
The variety in prompt wording provided flexibility in handling a broad spectrum of academic papers. It ensured that the model could correctly identify the method applied, whether related to "text mining," "computer vision," or a combination of both. Furthermore, the distinction between "other" and the other categories ensured that the model could identify research that didn't neatly fit into the other predefined categories.

<br>

#### **Prompts Used for Task 3**:

| **Prompt Number** | **Prompt Template** | **Description** | **Example** | **Performance** |
|-------------------|---------------------|-----------------|-------------|-----------------|
| **Prompt 1**      | Direct extraction | Requires extraction of the model or method name from the title and abstract | "Your task is to extract the name of the model or method used in the following research paper.<br>Title: `{title}` <br>Abstract: `{abstract}` <br>Decision:" | Normal |
| **Prompt 2**      | Conditional extraction | Focused on identifying the model or method, returning "NA" if no model is mentioned | "Extract the name of the model or method. If no model is mentioned, return 'NA'.<br>Title: `{title}` <br>Abstract: `{abstract}` <br>Decision:" | High |
| **Prompt 3**      | Multiple model extraction | Allows for extraction of multiple model names, if applicable | "Analyze the following title and abstract to identify and extract the specific model or method being discussed.<br>Title: `{title}` <br>Abstract: `{abstract}` <br>Decision:" | Moderate |
| **Prompt 4**      | Specific model targeting | Targets well-known models like BERT, ResNet, etc., with clear extraction instructions | "Your task is to extract the name of the model or method from research papers.<br>Title: `{title}` <br>Abstract: `{abstract}` <br>Decision:" | High |
| **Prompt 5**      | Simple conditional extraction | Marks the response as "No model found" if no model is mentioned | "Please read the following title and abstract carefully and extract any names of models or methods mentioned within them.<br>Title: `{title}` <br>Abstract: `{abstract}` <br>Decision:" | Normal |
| **Prompt 6 & 7**  | Bullet-point extraction | Extracts methods in a clean bullet-point format, handling hybrid and specific models | "Now, please analyze the following article and return the name of the methods.<br>Title: `{title}` <br>Abstract: `{abstract}` <br>Decision:" | Moderate |

#### **Explanation**:
These extraction prompts focused on ensuring that the model could identify specific methods, even those that might not be directly labeled as models but as methodologies used in research. By focusing on the extraction of multiple methods and using a clear, formatted list, these prompts ensured that all relevant methods were captured, improving the completeness of the extraction. By presenting examples in some prompts, we guided the model to format responses in bullet points or to return "NA" for cases where no model was identified. This technique focused the model’s attention on detecting unique terminology (like BERT or CNN) and returning concise results.

<br>

Using multiple prompt variations for each task is a strategy to reduce the impact of outliers and encourage more robust model responses. Each prompt variant approaches the task from a slightly different angle, such as adding context, altering phrasing, or including examples. Here’s how the iterative approach improves performance:

- **Enhances Consistency**: By offering different versions, I ensure the model learns to identify relevant cues across a broader range of expressions, improving its robustness in classification and extraction tasks.
- **Minimizes Misclassification**: Prompts like “you are an expert in deep learning” encourage the model to prioritize relevant terminology in scientific language, minimizing potential misclassification or over-generalization.

Also, For Tasks 1 and 3, some prompts include examples to demonstrate the expected structure and content of responses. I clarified the instructions by showing concrete examples and helped the model distinguish between relevant and irrelevant information, especially for binary classification tasks.

- **Structured Examples**: The examples in Task 1 help the model differentiate between papers focused on deep learning and unrelated fields. This clarity aids the model in generating "yes" or "no" responses with greater precision.
- **Guided Extraction**: For method extraction in Task 3, listing examples of recognized models encourages the model to seek similar terminology in titles and abstracts, leading to the accurate extraction of model names.

---

### **Challenges and Solutions**


**Ambiguity in Model Extraction:**

- Problem: The model’s response sometimes includes explanations or references to concepts rather than direct model names.
- Solution: We are developing post-processing techniques, including pattern matching (using regex), to extract model names more reliably. A second layer of prompts could also be used to clarify ambiguous outputs.
Lack of Evaluation Metrics:

- Problem: Due to the absence of predefined labels or ground truth data, validating the model’s accuracy is challenging.
- Solution: A manual evaluation of a subset of papers is planned. A group of experts will assess the LLM’s responses to provide a qualitative assessment of performance, which will help estimate accuracy.
Prompt Sensitivity:

- Problem: The quality of responses is highly dependent on the structure of the prompts used.
- Solution: By generating multiple variations of each prompt, the robustness of the system is improved, and diverse interpretations of the academic papers are captured.

---

### **Results** 

**Task 1 (Filtering Papers):** The LLM was able to successfully identify papers that use deep learning models. A subset of example responses can demonstrate its effectiveness.
**Task 2 (Categorizing Papers):** The LLM demonstrated strong categorization abilities, though there were cases where the response was NaN due to vague or unclear abstracts.
**Task 3 (Extracting Model Names):** While the LLM occasionally provided explanatory responses, it also correctly identified popular model names like BERT, ResNet, and GPT. Post-processing is being optimized to handle these cases more effectively.

### Future Improvements
- Evaluation Metrics:
The implementation of external evaluation techniques (e.g., domain experts or a comparison to human annotations) will help refine the accuracy assessment of the model’s outputs.
- Model Name Extraction:
We plan to enhance the post-processing pipeline by introducing more advanced pattern matching techniques and potentially incorporating a list of known deep learning models for comparison.
- Fine-tuning the LLM:
Fine-tuning the LLM on a domain-specific dataset could improve performance for tasks like categorization and model name extraction, ensuring that the system is more aligned with the academic context.
Conclusion

---
### **Conclusion and Summary**

This project showcases how large language models can be utilized to automate the analysis of academic papers, providing valuable insights into whether deep learning is used, how papers are categorized, and which models are employed. While the current version of the system demonstrates promising results, further refinement in evaluation metrics, prompt optimization, and post-processing techniques will enhance its reliability and applicability in real-world academic research scenarios.

By employing structured and varied prompts, we were able to:
1. **Filter out irrelevant papers** that did not discuss deep learning techniques.
2. **Classify papers** according to their methodology.
3. **Extract specific model or method names** from relevant papers.

These steps were critical in refining the dataset and ensuring that only the most pertinent research was included in the analysis, enhancing the quality and accuracy of the final output. The methodology employed, especially the prompt-based approach, ensured a high degree of precision and consistency in tackling the task.

We guided Llama 3.2 to filter, classify, and extract relevant information from a specialized dataset through structured prompt engineering, multiple prompt variants, and examples. These prompt techniques leveraged the LLM’s strengths in understanding context, handling structured tasks, and managing large datasets, making it an ideal model choice for this comprehensive NLP task.
