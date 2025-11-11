# Movie_Recommendation_System
The sources provide extensive, detailed information about the design, implementation, and deployment of your project, the **Movie Recommendation System**.

Below is a comprehensive structure and content for your GitHub `README.md` file, drawing on all the technical and conceptual details available in the excerpts.

***

# Movie Recommendation System

Repository: `kallemahendhar/Movie_Recommendation_System`

## 📝 Project Overview

This project delivers an **end-to-end machine learning solution** for recommending movies. We build a machine learning model, convert it into an interactive web application, and deploy it for public use. The primary goal of the system is to offer users highly relevant recommendations based on the content they enjoy.

Recommendation systems are critical in the modern online economy, helping users quickly find products or content (like movies, songs, or products) from vast databases. This concept is widely applied across platforms such as Netflix (for movies), Spotify (for songs), YouTube (for videos), and major e-commerce sites like Amazon and Flipkart.

## ⚙️ Methodology: Content-Based Filtering

This project utilizes the **Content-Based Recommendation System** approach.

### How Content-Based Filtering Works:
A Content-Based system recommends new items based on the similarity of the content itself.

1.  **Tag Creation:** The system creates "tags" for each movie based on its attributes (like genre, keywords, cast, and overview).
2.  **Similarity Check:** If a user likes a particular movie (e.g., a horror film), the system looks for other movies whose tags are highly similar to the liked movie's tags.
3.  **Recommendation:** Videos or movies similar in content are recommended.

This differs from Collaborative Filtering, which recommends content based on the similarity of user interests or behavior.

## 📊 Data Sources and Technology

### Data Set
The project uses the **TMDB 5000 Movie Dataset**.

The raw data consists of two files:
*   `tmdb_5000_movies.csv`: Contains metadata such as budget, genres, keywords, original language/title, popularity, revenue, and overview.
*   `tmdb_5000_credits.csv`: Contains details about the cast and crew, including actors, directors, editors, etc..

### Technologies Used

The core of the project is built using Python, leveraging the following key components:

| Category | Technology/Library | Purpose |
| :--- | :--- | :--- |
| **Development** | Python, Jupyter Notebook | Primary languages for ML scripting. |
| **Data Handling** | Pandas, NumPy | Data manipulation and array operations. |
| **Model Building** | Scikit-learn (`CountVectorizer`, `cosine_similarity`) | Vectorizing text data and calculating movie similarity. |
| **Text Processing** | NLTK (`PorterStemmer`) | Applying stemming to standardize words (e.g., 'actions' to 'action'). |
| **Web Interface** | **Streamlit** | Converting the model into an interactive website. |
| **Deployment** | Heroku, Git | Application deployment platform. |
| **API Integration** | Requests | Fetching external data, specifically **movie posters from TMDB API**. |

## 🚀 End-to-End Project Flow

The project execution is divided into four main stages:

### Stage 1: Data Preprocessing

This phase prepares the two raw datasets for model building:

1.  **Data Merging:** The `movies` and `credits` datasets are merged based on the movie `Title`.
2.  **Column Selection:** Only relevant columns that help in creating informative content tags are retained: `genres`, `id`, `keywords`, `overview`, `cast`, and `crew`. Irrelevant numerical columns (like budget, revenue, popularity) were discarded.
3.  **Data Cleaning:** Missing data (like the 3 rows with missing overview) and duplicate entries were addressed.
4.  **Data Transformation (JSON/String Conversion):** Columns like `genres`, `keywords`, `cast`, and `crew` were initially in a stringified list-of-dictionaries format. These were converted into proper Python lists using the `ast.literal_eval` function.
5.  **Feature Extraction:**
    *   **Cast:** Only the names of the **top three lead actors** were extracted.
    *   **Crew:** Only the **Director's name** was extracted from the crew column.
6.  **Space Removal and Standardization:** Spaces were removed between names and multi-word keywords (e.g., "Sam Worthington" became "SamWorthington"). This is crucial to prevent the vectorizer from treating "Sam" and "Worthington" as separate entities, which could lead to confusion with other names containing "Sam".

### Stage 2: Model Building and Similarity Calculation

This stage converts the textual features into a mathematical format (vectors) to calculate similarity.

1.  **Tag Creation:** A new `tags` column was created by **concatenating** the processed `overview`, `genres`, `keywords`, `cast`, and `crew` lists for each movie. The entire text was converted to lowercase for consistency.
2.  **Text Vectorization:**
    *   The **Bag of Words** technique was employed using Scikit-learn's `CountVectorizer`.
    *   The text corpus was limited to the **top 5,000 most frequently occurring words** (`max_features=5000`).
    *   Common English **stop words** (like 'a', 'the', 'and') were ignored.
3.  **Stemming:** The `PorterStemmer` from NLTK was applied to the tags to reduce words to their root form (e.g., converting 'actions', 'acting', and 'actor' to 'act'). This enhances feature accuracy by grouping variations of the same word.
4.  **Similarity Matrix Generation:**
    *   **Cosine Similarity** was calculated between all 4806 movie vectors. Cosine distance is used because it is a reliable measure for similarity in high-dimensional spaces, unlike Euclidean distance.
    *   This resulted in a 4806x4806 similarity matrix, where each cell represents the similarity score (ranging from 0 to 1) between two movies.

### Stage 3 & 4: Web Application and Deployment

The final stage involves presenting the model to the user and deploying the app:

1.  **Recommendation Function:** A function was created to:
    *   Accept a movie title from the user.
    *   Find the index of that movie in the dataset.
    *   Retrieve its corresponding row from the similarity matrix.
    *   **Sort** this row to identify the five most similar movies (highest similarity scores).
    *   Uses **API Integration** (via the `requests` library and the TMDB API) to fetch and display the associated **movie posters** along with the titles.
2.  **Web Interface:** The application interface was developed using the **Streamlit** library.
3.  **Deployment:** The application was deployed on **Heroku**. Deployment required specific configuration files (`Procfile`, `setup.sh`, `.gitignore`) and a refined `requirements.txt` listing only the primary dependencies (`streamlit`, `requests`, etc.).

***

## 🌐 Features and Usage

The final application allows users to:
1.  Select a movie from the complete list of 4806 movies.
2.  Click the "Recommend" button.
3.  Receive a list of **five highly similar movies** based on content tags, displayed with their respective titles and posters.

The result is a fully functional web app running on the server.