# 🎬 Neo4j Netflix Recommendation System

A **graph-based recommendation system** built using **Neo4j Graph Data Science (GDS)** on Netflix-like user behavior data.  
The project demonstrates how to apply **PageRank, Node Similarity, and Link Prediction** to generate personalized movie recommendations.

---

## 📂 Dataset
- Source: [Netflix User Behavior Dataset (Kaggle)](https://www.kaggle.com/datasets/sayeeduddin/netflix-2025user-behavior-dataset-210k-records)  
- Files used:
  - `users.csv` – user profiles  
  - `watch_history.csv` – viewing interactions  
  - `movies.csv` – movie metadata  
  - `reviews.csv` – user ratings  

---

## ⚙️ Workflow
1. Import datasets into Neo4j  
2. Create constraints and indexes  
3. Project graph into GDS  
4. Run algorithms:
   - **PageRank** → find influential movies  
   - **Node Similarity (Jaccard)** → find similar users/movies  
   - **Link Prediction** → forecast likely user–movie interactions  
5. Generate personalized recommendations  
6. Visualize results  

---

## 🛠️ Requirements
- **Neo4j Desktop** or **Neo4j Sandbox**  
- Python 3.x with packages:
  - `py2neo`  
  - `pandas`  
  - `networkx`  
  - `matplotlib`  

---

## 🚀 How to Run
1. Clone this repository  
2. Ensure Neo4j is running and update connection settings in the notebook  
3. Run the Jupyter Notebook step by step  
4. Explore recommendations and visualizations  

---

## 📖 Conclusion
Graph-based methods allow richer and more explainable recommendations compared to traditional collaborative filtering.  
This project highlights how **Neo4j GDS** can be effectively applied to build a scalable and insightful recommendation engine.  

---

## 🔮 Future Work
- Scale to larger datasets  
- Integrate **Graph Neural Networks (GNNs)**  
- Add advanced evaluation metrics (NDCG, MAP)  
- Build interactive dashboards with Neo4j Bloom or Dash  

---

## 🏁 Getting Started Example

Here’s a minimal Cypher example to get you started with loading movies and running PageRank:

```cypher
// Load movies (replace URL with your direct CSV link)
LOAD CSV WITH HEADERS FROM 'https://www.dropbox.com/s/yourfile/movies.csv?dl=1' AS row
MERGE (m:Movie {ID: row.movie_id})
SET m.title = row.title,
    m.genre = row.genre_primary,
    m.release_year = toInteger(row.release_year);

// Run PageRank to find influential movies
CALL gds.graph.project(
  'MovieGraph',
  'Movie',
  '*'
);

CALL gds.pageRank.stream('MovieGraph')
YIELD nodeId, score
RETURN gds.util.asNode(nodeId).title AS movie, score
ORDER BY score DESC
LIMIT 10;
