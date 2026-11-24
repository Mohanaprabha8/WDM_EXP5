### EX5 Information Retrieval Using Boolean Model in Python
### DATE: 24.11.2026
### NAME: MOHANAPRABHA D
### REG NO: 212225040254
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:
```
   import numpy as np
import pandas as pd

class BooleanRetrieval:
    def __init__(self):
        self.index = {}
        self.documents_matrix = None

    def index_document(self, doc_id, text):
        terms = text.lower().split()
        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()
            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())
        num_docs = len(documents)
        num_terms = len(terms)

        self.documents_matrix = np.zeros(
            (num_docs, num_terms), dtype=int
        )

        for i, (doc_id, text) in enumerate(documents.items()):
            for term in text.lower().split():
                term_id = terms.index(term)
                self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(
            self.documents_matrix,
            columns=self.index.keys()
        )
        print(df)

    def print_all_terms(self):
        print("All terms in the documents:")
        print(list(self.index.keys()))

    def boolean_search(self, query):
        import re

        tokens = re.findall(r'\w+', query.lower())

        all_docs = set()
        for docs in self.index.values():
            all_docs.update(docs)

        result = set()
        operator = "AND"
        negate = False
        first = True

        for token in tokens:
            if token in ("and", "or"):
                operator = token.upper()

            elif token == "not":
                negate = True

            else:
                docs = self.index.get(token, set())

                if negate:
                    docs = all_docs - docs
                    negate = False

                if first:
                    result = docs.copy()
                    first = False
                elif operator == "AND":
                    result = result.intersection(docs)
                elif operator == "OR":
                    result = result.union(docs)

        return sorted(result)


if __name__ == "__main__":

    indexer = BooleanRetrieval()

    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }

    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)

    indexer.create_documents_matrix(documents)
    indexer.print_documents_matrix_table()
    indexer.print_all_terms()

    query = input("Enter your boolean query: ")
    results = indexer.boolean_search(query)

    if results:
        print(f"Results for '{query}': {results}")
    else:
        print("No results found for the query.")
```      
### Output:
<img width="793" height="287" alt="Screenshot 2026-11-24 231102" src="https://github.com/user-attachments/assets/0c6310c6-ccb6-4ca8-90e9-cc472b1c551f" />

<img width="825" height="280" alt="Screenshot 2026-11-24 231137" src="https://github.com/user-attachments/assets/de260c01-b3e0-4458-873e-ddef5bb9eecb" />

<img width="823" height="276" alt="Screenshot 2026-11-24 231207" src="https://github.com/user-attachments/assets/bcfd253b-89e0-424b-a01b-cd4ce22f871d" />

### Result:
Thus, the Information Retrieval using Boolean Model was successfully implemented in Python. The documents were indexed, and Boolean search operations (AND, OR, and NOT) were performed successfully to retrieve the relevant documents based on the given queries.
