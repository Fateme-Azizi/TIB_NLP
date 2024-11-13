# TIB_NLP


### Semantic NLP Filtering for Deep Learning Papers in Virology/Epidemiology

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
- Approach: Five distinct prompts were created to account for the different ways papers might be described and classified.
- Output: The LLM returns one of the four categories or NaN if it is unable to confidently categorize the paper.


**Task 3:** Extracting Model Names from Paper
- Input: For this task, the LLM is provided with the title, abstract, and a prompt asking it to extract the names of deep learning models used in the paper.
- Challenge: The LLM’s response can be a string containing model names or additional explanatory text. This creates ambiguity in processing, as sometimes the LLM returns more information than required.
- Approach: The responses are post-processed and compared to response from different promts to isolate model names. 
- Output: The LLM provides a string of text that may contain the names of one or more models used in the paper.

#### **Note:**
##### Script filter_papers_task1.ipynb contains the whole code for task 1, Semantic NLP Filtering.
##### Script classification_task2.ipynb contains the whole code for task 2, Classification of Methods.
##### Script extract_model_name_task3.ipynb contains the whole code for task 3, Extraction of Methods.
---

### Prompt Templates 
Here you can find prompt templates used for each task.

#### **Prompts Used for Task 1**:
- **Prompt 1**: The basic structure of the prompt asked the model to classify the relevance of the paper based on the provided title and abstract.
    - **Example**: "Given the title and abstract of a research paper, classify whether it is relevant to deep learning applications."

- **Prompt 2**: This prompt emphasized the expertise of deep learning in determining relevance. It ensured that the model recognized deep learning-specific terminology and context.
    - **Example**: "You are an expert in deep learning. Based on the following title and abstract, determine if the paper contributes to the field of deep learning."

- **Prompt 3**: Here, a simple yes/no decision was requested based on whether deep learning was mentioned or relevant.
    - **Example**: "Analyze the following title and abstract to decide if it addresses deep learning methodologies."

- **Prompt 4**: Provided examples to clarify how to make a judgment about deep learning relevance.
    - **Example**: "Read the title and abstract below and determine if they pertain to deep learning. Use 'yes' if they do and 'no' if they do not."

- **Prompt 5**: This prompt encouraged summarizing the focus before making a judgment, making it more comprehensive.
    - **Example**: "You are tasked with reviewing academic papers for relevance to deep learning. For the following title and abstract, first summarize the main focus, then decide if it is related to deep learning."

#### **Explanation**:
- The use of multiple similar prompts allowed for a robust evaluation by testing different approaches to detecting deep learning relevance.
- The AI was guided by explicit instructions, examples, and summarization techniques, ensuring accuracy and consistency in filtering out irrelevant papers.


#### **Prompts Used for Task 2**:
- **Prompt 1**: This prompt was designed for direct classification based on the research paper’s title and abstract.
    - **Example**: "Classify the following research paper based on the type of method used."

- **Prompt 2**: This was similar to Prompt 1 but emphasized the categorization of the method.
    - **Example**: "Based on the title and abstract, determine the primary method used in this research paper."

- **Prompt 3**: Another prompt offering clear instructions to classify papers based on the research method, ensuring precision.
    - **Example**: "Read the title and abstract and classify it according to the method used."

- **Prompt 4**: This prompt helped contextualize the classification by explaining the possible methods and examples.
    - **Example**: "Analyze the title and abstract below and classify them based on the type of method employed."

- **Prompt 5**: It offered a direct classification request, focusing on identifying the most relevant category.
    - **Example**: "Classify the following title and abstract into one of these categories."

#### **Explanation**:
- The variety in prompt wording provided flexibility in handling a broad spectrum of academic papers. It ensured that the model could correctly identify the method applied, whether it be related to "text mining," "computer vision," or a combination of both.
- The distinction between "other" and the other categories ensured that the model could identify research that didn't neatly fit into the other predefined categories.



#### **Prompts Used for Task 3**:
- **Prompt 1**: Direct extraction of the model or method name was required from the provided title and abstract.
    - **Example**: "Your task is to extract the name of the model or method used in the following research paper."

- **Prompt 2**: Focused on identifying the model or method and returning "NA" if no model was mentioned.
    - **Example**: "Extract the name of the model or method. If no model is mentioned, return 'NA'."

- **Prompt 3**: A prompt that catered to extracting multiple model names if applicable.
    - **Example**: "Analyze the following title and abstract to identify and extract the specific model or method being discussed."

- **Prompt 4**: Provided clear instructions on identifying models such as BERT, ResNet, etc., helping the AI model target specific well-known names.
    - **Example**: "Your task is to extract the name of the model or method from research papers."

- **Prompt 5**: A simple approach where if no model was found, the response was clearly marked as "No model found."
    - **Example**: "Please read the following title and abstract carefully and extract any names of models or methods mentioned within them."

- **Prompt 6 & 7**: These prompts required extracting methods in a clean bullet-point format and were designed to handle a variety of possible method names, from hybrid approaches to specific deep learning models.
    - **Example**: "Now, please analyze the following article and return the name of the methods."

#### **Explanation**:
- These extraction prompts focused on ensuring that the model could identify specific methods, even those that might not be directly labeled as models but as methodologies used in research.
- By focusing on the extraction of multiple methods and using a clear, formatted list, these prompts ensured that all relevant methods were captured, improving the completeness of the extraction.


-----

### **Prompt Engineering Techniques Employed**

Prompt engineering is central to guiding the LLM’s responses, as it clarifies task objectives, reduces ambiguity, and shapes the output format. Here are the specific techniques and considerations used:

#### **1. Task-Specific Prompt Design**

For each task— filtering, classification, and extraction—separate prompts were crafted with explicit instructions. This modular design helped compartmentalize complex tasks, improving clarity and the relevance of LLM responses. Below, you can find more details on how I designed the prompts.

- **Semantic Filtering (Task 1)**: Here, prompts focus on binary classification ("yes" or "no") to decide whether a paper is relevant. By keeping the prompt output simple, I tried to minimize the risk of unnecessary information while making it easier for the model to focus on keywords like "neural networks," "deep learning," and domain-specific terms.
  
- **Method Classification (Task 2)**: For this classification task, I used straightforward prompts to assign a category (like "text mining" or "computer vision") based on context. The design of these prompts ensured the model would scan for specific methodological cues, improving classification accuracy.

- **Method Extraction (Task 3)**: Prompts in this task required Llama 3.2 to identify and list specific model or method names. By presenting examples in some prompts, I guided the model to format responses in bullet points or to return "NA" for cases where no model was identified. This technique focused the model’s attention on detecting unique terminology (like BERT or CNN) and returning concise results.

#### **2. Iterative Prompt Variants**

Using multiple prompt variations for each task is a strategy to reduce the impact of outliers and encourage more robust model responses. Each prompt variant approaches the task from a slightly different angle, such as adding context, altering phrasing, or including examples. Here’s how the iterative approach improves performance:

- **Enhances Consistency**: By offering different versions, I ensure the model learns to identify relevant cues across a broader range of expressions, improving its robustness in classification and extraction tasks.
- **Minimizes Misclassification**: Prompts like “you are an expert in deep learning” encourage the model to prioritize relevant terminology in scientific language, minimizing potential misclassification or over-generalization.

#### **3. Use of Examples within Prompts**

For Tasks 1 and 3, some prompts include examples to demonstrate the expected structure and content of responses. I clarified the instructions by showing concrete examples and helped the model distinguish between relevant and irrelevant information, especially for binary classification tasks.

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

I guided Llama 3.2 to filter, classify, and extract relevant information from a specialized dataset through structured prompt engineering, multiple prompt variants, and examples. These prompt techniques leveraged the LLM’s strengths in understanding context, handling structured tasks, and managing large datasets, making it an ideal model choice for this comprehensive NLP task.
