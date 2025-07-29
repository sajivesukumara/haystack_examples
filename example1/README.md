
## haystack_demo 1

The following example consists of the user asking a question that is then used
by a retriever to filter appropriate documents likely to contain an answer 
using appropriate metrics (BM25 in this case). 
Next, the question and the relevant context outputted by the retriever are
fed into a prompt builder to generate an appropriate prompt.

## Requirements

```bash
pip install -r requirements.txt
```
## Run

```bash
python python haystack_ex1.py
```

# Program structure

## Prepare input data
Query the poetryDB API to obtain poems by Shakespeare and store them as a JSON file:
Or create a json file with array of story(ies)

```json
[
  {
    "title": "short story",
    "lines": [ "line1", "line2", "line3"],
    "linecount": "3"
  }
]
```

## Store the data
Store the data as instances of Haystack’s Document class in a DocumentStore

## Indexing Pipeline
Define an indexing pipeline, that splits the documents and stores in DocumentStore

## Retrieve - Initialize the retriever 
With access to the document in the DocumentStore, the retrievel find the most relevant 
document to the qiven question. Here we use **BM25** retriever.

## Prompt creation
Create a custom prompt for generative question-answering task using the RAG approach.
The prompt take two parameters
- Documents, which are retrieved from the DocumentStore
- A question from the user

For this initialize a PromptBuilder instance with you prompt template.
```python
template = """
Given the following information, answer the question.
Context:
{% for document in documents %}
 {{ document.content }}
{% endfor %}
Question: {{question}}
Answer:
"""

prompt_builder = PromptBuilder(
    template=template, 
    required_variables="*"
)
```
**How It Works**
The template uses Jinja2 syntax:
- {% for %} loops through context documents
- {{ }} inserts variable values
When the pipeline runs, it:
- Receives documents from the retriever
- Inserts the question from user input
- Combines everything into a formatted prompt

## RAG Pipeline
make a RAG pipeline that encapsulates the workflow, including retrieving documents 
based on an input query and generating the output based on the retrieved context and query.


**Note**: Visualize pipeline using the following methods.

rag_pipeline.show() | rag_pipeline.draw("d:\\haystack_rag_pipeline.png")

# Run the RAG Pipeline
The RAG pipeline is invoked to answer a particular question, including
- retrieving the relevant context corresponding to that question,
- it appropriately builds the prompt based on the query and context:

