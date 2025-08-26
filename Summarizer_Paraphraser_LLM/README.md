# 📊 LangChain: Sequential Text Summarization and Paraphrasing Chain

## 🔑 Keywords
- **LangChain**
- **SequentialChain**
- **ConversationBufferMemory**
- **ChatOpenAI**
- **Language Detection**
- **Translation**
- **Summarization**
- **Paraphrasing**
- **Text Processing**

---

## 📖 Overview
This project demonstrates a **sequential text processing pipeline** using LangChain to:
1. Detect the language of input text.
2. Translate the text into English.
3. Summarize or paraphrase the translated text based on user choice.

The pipeline leverages `SequentialChain` with `ConversationBufferMemory` to maintain chat history, ensuring context retention across multiple text inputs. The system processes two texts, concatenates the results, and supports both summarization and paraphrasing modes.

---

## ⚙️ Workflow

1. **Setup LLM**
   - Initialize `ChatOpenAI` with temperature=0.7 and model="gpt-3.5-turbo".

2. **Memory**
   - Use `ConversationBufferMemory` to store chat history with keys for input and output.

3. **Language Detection**
   - Detect the language of the input text using a dedicated `LLMChain`.

4. **Translation**
   - Translate the input text into English using another `LLMChain`.

5. **Summarization/Paraphrasing**
   - Process the translated text (summarize or paraphrase) into a concise output (2-3 sentences).

6. **Sequential Chain**
   - Combine the three chains into a `SequentialChain` with verbose logging.
   - Input variables: `input_text` (the text to process), `mode` (summarize or paraphrase).
   - Output variables: `language`, `english_text`, `processed_text`.

7. **Processing Two Texts**
   - Process two user-provided texts.
   - Concatenate the processed outputs (summarized or paraphrased).
   - Display intermediate results (translated text) and final concatenated output.

---

## 🚀 Example Usage

### Import Required Modules
```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain.chains import LLMChain, SequentialChain
from langchain.memory import ConversationBufferMemory
```

### Initialize LLM and Memory
```python
llm = ChatOpenAI(temperature=0.7, model="gpt-3.5-turbo")

memory = ConversationBufferMemory(
    memory_key="chat_history",
    input_key="input_text",
    output_key="processed_text"
)
```

### Define Chains
#### 1. Language Detection
```python
detect_prompt = ChatPromptTemplate.from_template(
    "Detect the language of the following text:\n\n{input_text}"
)
detect_chain = LLMChain(llm=llm, prompt=detect_prompt, output_key="language")
```

#### 2. Translation to English
```python
translate_prompt = ChatPromptTemplate.from_template(
    "Translate the following text into English:\n\n{input_text}"
)
translate_chain = LLMChain(llm=llm, prompt=translate_prompt, output_key="english_text")
```

#### 3. Summarization/Paraphrasing
```python
process_prompt = ChatPromptTemplate.from_template(
    "You need to {mode} the following English text into a concise summary (max 2-3 sentences):\n\n{english_text}"
)
process_chain = LLMChain(llm=llm, prompt=process_prompt, output_key="processed_text")
```

#### 4. Sequential Chain
```python
overall_chain = SequentialChain(
    memory=memory,
    chains=[detect_chain, translate_chain, process_chain],
    input_variables=["input_text", "mode"],
    output_variables=["language", "english_text", "processed_text"],
    verbose=True
)
```

### Process Single Text (Summarization Example)
```python
text1 = """Le produit est arrivé très rapidement, ce qui était une bonne surprise. 
Cependant, l’emballage était endommagé et cela a légèrement abîmé l’article. 
Dans l’ensemble, je suis satisfait de la qualité du produit, mais je pense que 
l’entreprise devrait faire plus attention à la protection des colis."""

result1 = overall_chain({
    "input_text": text1,
    "mode": "summarize"
})

print("\n".join(f"{key}: {value}" for key, value in result1.items()))
```

### Process Second Text and Concatenate
```python
text2 = """Heureusement, le service client a été très réactif et m’a proposé un remboursement 
partiel rapidement. Leur professionnalisme m’a convaincu de continuer à acheter chez eux."""

result2 = overall_chain({
    "input_text": text2,
    "mode": "summarize"
})

print("\n".join(f"{key}: {value}" for key, value in result2.items()))

final_text1 = result1["processed_text"]
final_text2 = result2["processed_text"]
concatenated_text = final_text1 + " " + final_text2
print(concatenated_text)
```

### Display Dialogue History
```python
print("\n=== Dialogue History ===")
for msg in memory.chat_memory.messages:
    print(msg.content)
```

### Process Two Texts Interactively
```python
def process_two_texts():
    mode = input("Choose mode (summarize/paraphrase): ").strip().lower()
    
    text1 = input("Enter the first text:\n").strip()
    text2 = input("Enter the second text:\n").strip()
    
    result1 = overall_chain({"input_text": text1, "mode": mode})
    print("\n=== Text 1 translated into English ===")
    print(result1["english_text"])
    
    result2 = overall_chain({"input_text": text2, "mode": mode})
    print("\n=== Text 2 translated into English ===")
    print(result2["english_text"])
    
    # Final result
    final_result = result1["processed_text"] + " " + result2["processed_text"]
    
    # Automate final title based on chosen mode
    mode_title = "summarized" if mode == "summarize" else "paraphrased"
    print(f"\n=== Concatenated final text ({mode_title}) ===")
    print(final_result)
```

### Run for Summarization and Paraphrasing
```python
process_two_texts()  # For summarization
process_two_texts()  # For paraphrasing
```
