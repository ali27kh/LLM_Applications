# 📊 LangChain: Multi-Prompt Router Chain

## 🔑 Keywords
- **LangChain**
- **MultiPromptChain**
- **LLMRouterChain**
- **Expert Routing**
- **ChatOpenAI**
- **Prompt Templates**
- **Medical Expert**
- **Psychology Expert**
- **Math Expert**
- **History of Tunisia Expert**

---

## 📖 Overview
This project demonstrates how to set up a **multi-prompt router chain** in LangChain to intelligently route user questions to specialized expert chains. The router selects the most appropriate expert based on the question's topic, falling back to a default chain if no specific expert matches.

Experts include:
- Medicine: For medical questions.
- Psychology: For psychological inquiries.
- Math: For solving mathematical problems.
- History of Tunisia: For questions on Tunisian history.

The setup uses `MultiPromptChain` with an LLM router to direct inputs, showcasing dynamic chaining for domain-specific responses.

---

## ⚙️ Workflow

1. **Setup LLM**
   - Initialize `ChatOpenAI` with specified parameters (temperature=0, model="gpt-3.5-turbo", max_tokens=500).

2. **Define Expert Templates**
   - Create prompt templates for each expert domain.

3. **Create Destination Chains**
   - Build an `LLMChain` for each expert using the corresponding template.

4. **Create Router**
   - Define a router prompt to select the best expert.
   - Use `LLMRouterChain` with a custom output parser.

5. **Assemble MultiPromptChain**
   - Combine the router, destination chains, and a default chain.
   - Enable verbose mode for debugging.

6. **Testing**
   - Run the chain on a list of sample questions and print responses.

---

## 🚀 Example Usage

### Import Required Modules
```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate, PromptTemplate
from langchain.chains import LLMChain
from langchain.chains.router.llm_router import LLMRouterChain, RouterOutputParser
from langchain.chains.router.multi_prompt import MultiPromptChain
```

### Initialize LLM
```python
llm = ChatOpenAI(temperature=0, model="gpt-3.5-turbo", max_tokens=500)
```

### Define Expert Templates
```python
medicine_template = """You are a highly skilled doctor. 
You answer medical questions clearly and accurately. Here is the question: {input}"""

psychology_template = """You are an expert psychologist. 
You answer psychological questions with care and clarity. Here is the question: {input}"""

math_template = """You are an expert mathematician. 
You solve math problems step by step. Here is the question: {input}"""

history_tunisia_template = """You are an expert in the history of Tunisia. 
You answer questions about historical events in Tunisia precisely. Here is the question: {input}"""

prompt_infos = [
    {"name": "medicine", "description": "Medical questions", "prompt_template": medicine_template},
    {"name": "psychology", "description": "Psychological questions", "prompt_template": psychology_template},
    {"name": "math", "description": "Math questions", "prompt_template": math_template},
    {"name": "history_tunisia", "description": "History of Tunisia", "prompt_template": history_tunisia_template}
]
```

### Create Destination Chains
```python
destination_chains = {}
for p_info in prompt_infos:
    name = p_info["name"]
    prompt = ChatPromptTemplate.from_template(template=p_info["prompt_template"])
    chain = LLMChain(llm=llm, prompt=prompt)
    destination_chains[name] = chain
```

### Create Router
```python
destinations_str = "\n".join([f"{p['name']}: {p['description']}" for p in prompt_infos])

MULTI_PROMPT_ROUTER_TEMPLATE = f"""
Given a question, select the most appropriate expert for answering.
Available experts:
{destinations_str}

Input: {{input}}

Output a JSON in a markdown code block like this:
```json
{{{{"destination": "string", "next_inputs": "string"}}}}

"""

router_prompt = PromptTemplate(
    template=MULTI_PROMPT_ROUTER_TEMPLATE,
    input_variables=["input"],
    output_parser=RouterOutputParser()
)

router_chain = LLMRouterChain.from_llm(llm, router_prompt)
```

### Assemble MultiPromptChain
```python
default_chain = LLMChain(llm=llm, prompt=ChatPromptTemplate.from_template("{input}"))

multi_prompt_chain = MultiPromptChain(
    router_chain=router_chain,
    destination_chains=destination_chains,
    default_chain=default_chain,
    verbose=True
)
```

### Test with Questions
```python
questions = [
    "Quels sont les symptômes courants de la grippe ?",
    "Résoudre l'équation x^2 - 5x + 6 = 0",
    "Quelles sont les causes de stress les plus fréquentes selon la psychologie moderne ?",
    "Quand a eu lieu l'indépendance de la Tunisie ?",
    "Calculate 7*8+65"
]

for q in questions:
    print(f"\nQuestion: {q}")
    response = multi_prompt_chain.run(q)
    print(f"Answer: {response}")
```

---

## 📊 Sample Results
Here are simulated sample outputs based on expected routing and responses (actual results may vary slightly due to LLM variability):

- **Question**: Quels sont les symptômes courants de la grippe ?  
  **Routed to**: medicine  
  **Answer**: Les symptômes courants de la grippe incluent la fièvre, la toux, les maux de gorge, les courbatures, la fatigue, les frissons et parfois des nausées ou des vomissements.

- **Question**: Résoudre l'équation x^2 - 5x + 6 = 0  
  **Routed to**: math  
  **Answer**: Pour résoudre l'équation x² - 5x + 6 = 0, factorisons : (x - 2)(x - 3) = 0. Les solutions sont x = 2 et x = 3.

- **Question**: Quelles sont les causes de stress les plus fréquentes selon la psychologie moderne ?  
  **Routed to**: psychology  
  **Answer**: Selon la psychologie moderne, les causes de stress les plus fréquentes incluent le travail, les relations interpersonnelles, les problèmes financiers, les changements de vie et les pressions sociétales.

- **Question**: Quand a eu lieu l'indépendance de la Tunisie ?  
  **Routed to**: history_tunisia  
  **Answer**: L'indépendance de la Tunisie a eu lieu le 20 mars 1956.

- **Question**: Calculate 7*8+65  
  **Routed to**: default (or math if matched)  
  **Answer**: 7 * 8 + 65 = 56 + 65 = 121.

#### 👉 Interpretation:
- The router effectively directs questions to the appropriate expert.
- For unmatched questions, the default chain handles them directly.
- Verbose mode provides insights into the routing decisions.

---

## ✅ Features
- 🔄 Dynamic routing to domain-specific experts
- 📝 Customizable prompt templates for each expert
- 🔍 Verbose logging for chain execution
- 🔄 Extendable to additional experts and domains
- 📊 Handles fallback with a default chain