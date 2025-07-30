# haystack 

In Haystack, pipelines refer to the structured workflows that connect the various components
necessary for an LLM-powered application to function seamlessly. A pipeline defines the sequence
of operations involved in tasks like retrieving relevant information, processing data, 
and generating responses

For example, in a RAG setup, a pipeline might include components for
- retrieving documents from a knowledge base,
- preprocessing the retrieved content, and
- feeding it into a language model for response generation. 

# Example1 - Simple program that reads the json and responds to the question asked.



## Haystack PromptBuilder Templates
The PromptBuilder component in Haystack supports several template formats and styles. Here are the main types:

- 1. Basic Template
The simplest form, as shown in your code:

```bash
template = """
Question: {{question}}
Answer:
"""
```

- 2. Context-Aware Template
For RAG applications with document context:

```python
template = """
Context:
{% for document in documents %}
{{ document.content }}
{% endfor %}
Question: {{question}}
Answer:
"""
```

- 3. Structured Output Template
For getting formatted responses:

```
template = """
Based on the context, provide a response in the following format:
Context: 
{% for document in documents %}
{{ document.content }}
{% endfor %}

Question: {{question}}

Format your response as:
- Main Point:
- Supporting Evidence:
- Conclusion:
"""
```

- 4. Few-Shot Template
Including examples to guide the model:

```
template = """
Here are some examples:
Q: What is the capital of France?
A: Paris is the capital of France.

Q: What is the capital of Italy?
A: Rome is the capital of Italy.

Now answer this question:
Context:
{% for document in documents %}
{{ document.content }}
{% endfor %}
Question: {{question}}
Answer:
"""
```

- 5. Role-Based Template
Specifying a particular role or persona:

```
template = """
You are an expert literature professor.
Based on the following texts:
{% for document in documents %}
{{ document.content }}
{% endfor %}

Please analyze the following question:
{{question}}

Provide your expert analysis:
"""
```

**Key Features:**
- Uses Jinja2 templating syntax
- Supports conditional statements ({% if %})
- Allows loops ({% for %})
- Can include variable interpolation ({{ }})
- Supports custom metadata fields
- Can handle multiple document formats

To use any of these templates, initialize the PromptBuilder with your chosen template:
```
prompt_builder = PromptBuilder(
    template=your_template,
    required_variables=["question", "documents"]  # Specify required variables
)
```




