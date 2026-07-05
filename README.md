#  Movie Recommender

An end-to-end movie recommendation web app combining a **FastAPI** backend with a **Streamlit** frontend. It suggests movies using **TF-IDF content-based similarity** on a local dataset, and enriches results with **live posters, genres, and metadata from TMDB**.

**Live Demo:** _add your Streamlit Cloud link here_
**API:** https://movie-rec-466x.onrender.com

---

## Features

-  **Keyword search** with autocomplete suggestions (powered by TMDB)
-  **Home feed** — Trending, Popular, Top Rated, Now Playing, Upcoming
-  **Movie details page** — overview, genres, release date, poster, backdrop
-  **TF-IDF based recommendations** — content-based similarity from a locally trained model
-  **Genre-based recommendations** — fetched live from TMDB
-  **FastAPI backend** with typed, validated responses (Pydantic models)
-  Responsive poster grid UI built in Streamlit

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend API | FastAPI, Uvicorn |
| Frontend | Streamlit |
| ML / Recommendation | Scikit-learn (TF-IDF), NumPy, SciPy (cosine similarity) |
| Data | Pandas, Pickle (`df.pkl`, `tfidf.pkl`, `tfidf_matrix.pkl`, `indices.pkl`) |
| External API | [TMDB (The Movie Database)](https://www.themoviedb.org/documentation/api) |
| HTTP Client | httpx, requests |

---

## Project Structure

```
movie_recommender/
├── main.py              # FastAPI backend — routes, TF-IDF logic, TMDB integration
├── app.py               # Streamlit frontend — UI, search, details, recommendations
├── requirement.txt      # Python dependencies
├── df.pkl               # Preprocessed movie dataset
├── indices.pkl          # Title → index mapping for TF-IDF lookup
├── tfidf.pkl            # Fitted TF-IDF vectorizer
├── tfidf_matrix.pkl     # Precomputed TF-IDF matrix (sparse)
└── .gitignore
```

---

## How It Works

1. **Search** — User types a movie name; the app queries TMDB (`/tmdb/search`) for live suggestions.
2. **Select** — Selecting a movie loads its full details via `/movie/id/{tmdb_id}`.
3. **Recommend** —
   - **TF-IDF recommendations**: cosine similarity is computed between the selected movie's TF-IDF vector and all others in the local dataset (`tfidf_matrix.pkl`), returning the most similar titles.
   - **Genre recommendations**: TMDB's `/discover/movie` endpoint is queried using the selected movie's primary genre for real-time, popularity-ranked suggestions.
4. **Display** — Both recommendation sets are rendered as poster grids in the Streamlit UI.

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/health` | Health check |
| `GET` | `/home` | Home feed (trending/popular/top_rated/upcoming/now_playing) |
| `GET` | `/tmdb/search` | Keyword search via TMDB (raw results) |
| `GET` | `/movie/id/{tmdb_id}` | Full movie details |
| `GET` | `/recommend/genre` | Genre-based recommendations (TMDB) |
| `GET` | `/recommend/tfidf` | TF-IDF only recommendations (debug) |
| `GET` | `/movie/search` | Bundle: details + TF-IDF recs + genre recs |

---

## Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/Jyotikasingh04/IPL_Analysis_Web_app.git
cd movie_recommender
```

### 2. Install dependencies
```bash
pip install -r requirement.txt
```

### 3. Configure environment variables
Create a `.env` file in the root directory:
```
TMDB_API_KEY=your_tmdb_api_key_here
```
> Get a free API key from [TMDB](https://www.themoviedb.org/settings/api).

### 4. Run the FastAPI backend
```bash
uvicorn main:app --reload
```
API will be live at `http://127.0.0.1:8000`

### 5. Run the Streamlit frontend
In a separate terminal:
```bash
streamlit run app.py
```
> Update `API_BASE` in `app.py` to `http://127.0.0.1:8000` for local development.

---

## Future Enhancements

- Deep learning-based recommendation model (e.g., embeddings via neural networks)
- User accounts with personalized watchlists and rating history
- Hybrid recommender combining collaborative + content-based filtering
- Caching layer to reduce redundant TMDB API calls

---

## Author

**Jyotika Singh**
GitHub: [@Jyotikasingh04](https://github.com/Jyotikasingh04)
