# 🎥 MovieGraphQL: LLM Based Movie Search with Neo4j & Groq LLaMA3
#### Graph-Based Movie Intelligence Using Neo4j + Groq LLaMA3
This project integrates a graph database (Neo4j) with Groq’s LLaMA3.3-70B model to create a natural-language-powered movie knowledge engine. Using LangChain's GraphCypherQAChain, it allows users to query complex movie relationships — such as directors, genres, and IMDB ratings — using plain English.

## 📌 Key Highlights
* 📊 Graph Database Architecture: All movie data, actors, directors, and genres are structured using Neo4j’s highly connected graph model.
* 🤖 Natural Language Queries: LLaMA3.3-70B interprets user questions and auto-generates Cypher queries using LangChain’s GraphCypherQAChain.
* ⚡ High Performance with Groq API: LLM-powered reasoning runs with ultra-fast inference through Groq’s backend, ensuring sub-second response times.
* 🧠 Contextual AI Insight: Combines structured graph data with AI to provide intelligent, conversational answers.

## 🧠 Technologies Used
* Neo4j – Graph database to store and manage movie knowledge
* LangChain – GraphCypherQAChain for LLM-driven query handling
* Groq API – Used to run the llama-3.3-70b-versatile model
* Cypher – Query language for interacting with Neo4j
* Python (Jupyter Notebook) – Used for orchestration and demo

## 📊 Graph Schema
* 🔹 Node Types:
  - Movie {id, title, released, imdbRating}
  - Person {name}
  - Genre {name}

* 🔹 Relationships:
  - (:Movie)-[:IN_GENRE]->(:Genre)
  - (:Person)-[:DIRECTED]->(:Movie)
  - (:Person)-[:ACTED_IN]->(:Movie)

## 🖼 Visualization
Includes full graph visualizations showing thousands of interconnected nodes.
###### Output
![visualisation](https://github.com/user-attachments/assets/cc002a53-f562-42f7-94af-ab3a26efb2f1)
###### Zoomed in for clear understanding
![visualisation Zoomed In](https://github.com/user-attachments/assets/ea0f07e6-a68c-4795-9067-278c01cb0327)

## 📁 Project Structure
```
├── Movie_Graph_DB_With_Neo4j_and_Groq_LLM.ipynb   # Jupyter Notebook (main logic)
├── Movie_Graph_DB_With_Neo4j_and_Groq_LLM with Visualization.pdf  # Project doc
├── visualisation.png                              # Full graph snapshot
├── visualisation Zoomed In.png                    # Zoomed node connections
└── requirements.txt                               # Dependencies
```

## ⚙️ Setup Instructions
1. Install Dependencies
```
pip install --upgrade langchain langchain-community langchain-groq neo4j langchain_experimental
```
2. Set Your Environment Variables
```python
os.environ["NEO4J_URI"] = "bolt+s://<your-neo4j-uri>"
os.environ["NEO4J_USERNAME"] = "neo4j"
os.environ["NEO4J_PASSWORD"] = "<your-password>"
groq_api_key = "<your-groq-api-key>"
```
3. Load and Populate Graph with CSV
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/tomasonjo/blog-datasets/main/movies/movies_small.csv' AS row

MERGE (m:Movie {id: row.movieId})
SET m.title = row.title,
    m.released = date(row.released),
    m.imdbRating = toFloat(row.imdbRating)

FOREACH (director IN split(row.director, '|') |
  MERGE (p:Person {name: trim(director)})
  MERGE (p)-[:DIRECTED]->(m))

FOREACH (actor IN split(row.actors, '|') |
  MERGE (p:Person {name: trim(actor)})
  MERGE (p)-[:ACTED_IN]->(m))

FOREACH (genre IN split(row.genres, '|') |
  MERGE (g:Genre {name: trim(genre)})
  MERGE (m)-[:IN_GENRE]->(g))
```

## 💬 Example Natural Language Queries

| 🧠 User Query                                  | 🧾 LLM Response                                               |
|-----------------------------------------------|---------------------------------------------------------------|
| “Who directed the movie *GoldenEye*?”         | Martin Campbell                                               |
| “Tell me the genres of *GoldenEye*.”          | Adventure, Action, Thriller                                   |
| “List movies with IMDB rating above 8.”       | Seven, Usual Suspects, Toy Story, Taxi Driver...              |
| “Which movies were released in 2008?”         | *(Returns empty if no matches)*                               |

All queries auto-converted to Cypher and executed via LangChain.

## 📝 Notes
This project was fully developed by me. The README was written based on my knowledge and experience, with support from generative AI tools to refine its structure and presentation.

## 📄 License
This project is open source and available under the MIT License.

## 🙋‍♂️ Author
Made with ❤️ by Bhaskar Shivaji Kumbhar
