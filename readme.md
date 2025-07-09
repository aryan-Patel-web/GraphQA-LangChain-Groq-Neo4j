
# GraphQA-LangChain-Groq-Neo4j

**Graph-powered Q&A system using LangChain, Groq LLM, and Neo4j graph database for natural-language querying of movie data.**

This repo demonstrates how to:
- Load a movie dataset into Neo4j
- Connect LangChain’s GraphCypherQAChain to Groq’s Gemma LLM
- Automatically translate user questions into Cypher queries and retrieve answers

---

## 🚀 How to Run

### 1. Set your environment variables

Create a `.env` file with:

```env

NEO4J_URI=bolt://localhost:7687
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your_password
GROK_API_KEY=your_groq_api_key


 Install dependencies

pip install langchain langchain_groq neo4j python-dotenv

 Load movie dataset into Neo4j


 LOAD CSV WITH HEADERS FROM

'https://raw.githubusercontent.com/tomasonjo/blog-datasets/main/movies/movies_small.csv' AS row

MERGE (m:Movie {id: row.movieId})
SET m.released = date(row.released),
    m.title = row.title,
    m.imdbRating = toFloat(row.imdbRating)
FOREACH (director IN split(row.director, '|') |
    MERGE (p:Person {name: trim(director)})
    MERGE (p)-[:DIRECTED]->(m))
FOREACH (actor IN split(row.actors, '|') |
    MERGE (p:Person {name: trim(actor)})
    MERGE (p)-[:ACTED_IN]->(m))
FOREACH (genre IN split(row.genres, '|') |
    MERGE (g:Genre {name: trim(genre)})
    MERGE (m)-[:IN_GENRE]->(g));


📝 Example Queries
“Which movies did Leonardo DiCaprio act in?”

“Show all genres with more than 10 movies.”

“Create a new movie node called ‘My Movie 2025’.”



