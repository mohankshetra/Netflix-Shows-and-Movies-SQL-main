# Netflix Shows and Movies Analysis

## 📌 Project Overview

This project analyzes the **Netflix Shows and Movies dataset** to uncover meaningful insights about Netflix's content library.

The analysis focuses on:

* IMDb ratings
* Content released across different decades
* Age certifications
* Movie and TV show genres
* Top and bottom-rated content
* Content distribution and audience preferences

The project uses **Excel, MySQL, and Tableau** to clean, analyze, and visualize the dataset.

## 🛠️ Tools Used

* **Excel**
* **MySQL**
* **Tableau**

## 📊 Dataset

The dataset used for this project is the Netflix TV Shows and Movies dataset from Kaggle.

**Dataset:**
https://www.kaggle.com/datasets/victorsoeiro/netflix-tv-shows-and-movies?select=titles.csv

## 🔗 Project Resources

### SQL Analysis

The SQL queries used for the analysis are available here:

https://github.com/MohankShetra/Netflix-Shows-and-Movies-SQL/blob/main/Netflix_SQL_Analysis.sql

### Tableau Dashboard

The interactive Netflix dashboard is available here:

https://public.tableau.com/app/profile/mohankshetra/viz/NetflixShowsMoviesDashboard/Dashboard1

---

# 💼 Business Problem

Netflix wants to gather useful insights from its shows and movies dataset for its subscribers.

The dataset contains approximately **82,000 rows of combined data**, making it difficult to manually analyze and extract meaningful insights.

Therefore, a robust data analytics approach is required to:

* Analyze large amounts of Netflix content data
* Identify important patterns and trends
* Understand audience preferences
* Identify highly rated and poorly rated content
* Analyze genre and age-certification distributions
* Present insights through interactive visualizations

# 💡 Proposed Solution

To solve this problem, **MySQL** is used to analyze and extract relevant information from the Netflix dataset, while **Tableau** is used to visualize the results.

SQL analysis helps identify important metrics and patterns such as:

* IMDb ratings
* Popularity trends
* Genre preferences
* Content distribution
* Release-year trends
* Age-certification patterns

The extracted results are then presented using Tableau dashboards, allowing users to interactively explore the Netflix content library.

---

# 🔎 Questions Answered Through the Analysis

## 1. What are the Top 10 and Bottom 10 Movies and Shows Based on IMDb Scores?

### Top 10 Movies

```mysql
SELECT title, 
       type, 
       imdb_score
FROM shows_movies.titles
WHERE imdb_score >= 8.0
  AND type = 'MOVIE'
ORDER BY imdb_score DESC
LIMIT 10;
```

### Result

![Top 10 Movies](https://i.ibb.co/6mQWCw9/Screen-Shot-2023-07-09-at-9-38-11-PM.png)

### Top 10 Shows

```mysql
SELECT title, 
       type, 
       imdb_score
FROM shows_movies.titles
WHERE imdb_score >= 8.0
  AND type = 'SHOW'
ORDER BY imdb_score DESC
LIMIT 10;
```

### Result

![Top 10 Shows](https://i.ibb.co/QppHsN2/Screen-Shot-2023-07-09-at-9-45-58-PM.png)

### Bottom 10 Movies

```mysql
SELECT title, 
       type, 
       imdb_score
FROM shows_movies.titles
WHERE type = 'MOVIE'
ORDER BY imdb_score ASC
LIMIT 10;
```

### Result

![Bottom 10 Movies](https://i.ibb.co/tMXV1yp/Screen-Shot-2023-07-09-at-9-47-24-PM.png)

### Bottom 10 Shows

```mysql
SELECT title, 
       type, 
       imdb_score
FROM shows_movies.titles
WHERE type = 'SHOW'
ORDER BY imdb_score ASC
LIMIT 10;
```

### Result

![Bottom 10 Shows](https://i.ibb.co/Y7Qjvg5/Screen-Shot-2023-07-09-at-9-49-36-PM.png)

### Insight

The IMDb score provides an indication of the overall quality and audience reception of a movie or show.

The top-rated titles demonstrate strong audience reception and can be useful for identifying highly regarded content in Netflix's library.

The bottom-rated titles received comparatively lower IMDb scores. These results can help identify content that may have weaker audience reception and provide opportunities for further analysis.

---

# 2. How Many Movies and Shows Fall Into Each Decade?

```mysql
SELECT CONCAT(FLOOR(release_year / 10) * 10, 's') AS decade,
       COUNT(*) AS movies_shows_count
FROM shows_movies.titles
WHERE release_year >= 1940
GROUP BY CONCAT(FLOOR(release_year / 10) * 10, 's')
ORDER BY decade;
```

### Result

![Content by Decade](https://i.ibb.co/8dTzVZ3/Screen-Shot-2023-07-09-at-10-02-18-PM.png)

### Insight

The analysis shows a significant change in Netflix's content distribution across different decades.

The 1940s through the 1980s contain relatively few titles, while the number of titles increases significantly from the 1990s onward.

The 1990s contain **121 titles**, while the 2010s contain approximately **3,304 titles**.

The 2020s already contain approximately **1,972 titles** in the dataset, demonstrating the strong presence of recent content.

---

# 3. How Did Age Certifications Impact the Dataset?

### Average IMDb and TMDB Scores by Age Certification

```mysql
SELECT DISTINCT age_certification,
       ROUND(AVG(imdb_score),2) AS avg_imdb_score,
       ROUND(AVG(tmdb_score),2) AS avg_tmdb_score
FROM shows_movies.titles
GROUP BY age_certification
ORDER BY avg_imdb_score DESC;
```

### Result

![Average Ratings by Certification](https://i.ibb.co/SvJyjgF/Screen-Shot-2023-07-09-at-10-16-52-PM.png)

### Most Common Age Certifications

```mysql
SELECT age_certification,
       COUNT(*) AS certification_count
FROM shows_movies.titles
WHERE type = 'Movie'
  AND age_certification != 'N/A'
GROUP BY age_certification
ORDER BY certification_count DESC
LIMIT 5;
```

### Result

![Age Certification Distribution](https://i.ibb.co/T0f5cNq/Screen-Shot-2023-07-09-at-10-20-23-PM.png)

### Insight

The analysis of average IMDb scores shows differences between age-certification categories.

**TV-14** has the highest average IMDb score at approximately **6.71**, followed by other certification categories.

The distribution analysis also shows that **R** is the most prevalent certification in the analyzed movie dataset, with **556 titles**, followed by **PG-13 with 451 titles** and **PG with 233 titles**.

The dataset therefore demonstrates a diverse range of content across different age-certification categories.

---

# 4. Which Genres Are the Most Common?

## Top 10 Movie Genres

```mysql
SELECT genres,
       COUNT(*) AS title_count
FROM shows_movies.titles
WHERE type = 'Movie'
GROUP BY genres
ORDER BY title_count DESC
LIMIT 10;
```

### Result

![Top Movie Genres](https://i.ibb.co/VWrgd8m/Screen-Shot-2023-07-10-at-12-25-40-PM.png)

## Top 10 Show Genres

```mysql
SELECT genres,
       COUNT(*) AS title_count
FROM shows_movies.titles
WHERE type = 'Show'
GROUP BY genres
ORDER BY title_count DESC
LIMIT 10;
```

### Result

![Top Show Genres](https://i.ibb.co/P59s4X7/Screen-Shot-2023-07-10-at-12-27-41-PM.png)

## Top 3 Genres Overall

```mysql
SELECT t.genres,
       COUNT(*) AS genre_count
FROM shows_movies.titles AS t
WHERE t.type = 'Movie' OR t.type = 'Show'
GROUP BY t.genres
ORDER BY genre_count DESC
LIMIT 3;
```

### Result

![Top Overall Genres](https://i.ibb.co/qMvMBGf/Screen-Shot-2023-07-10-at-12-30-04-PM.png)

### Insight

The genre analysis provides an overview of the types of content that dominate Netflix's library.

For movies, **Comedy** is the most common genre with approximately **384 movies**, followed by **Documentation with 230 movies** and **Drama with 224 movies**.

For shows, **Reality** is the leading genre with approximately **113 shows**, followed by **Drama with 104 shows**, while Comedy and Documentation each account for approximately **100 shows**.

When movies and shows are considered together, the three most common genres are:

1. **Comedy — 484 titles**
2. **Documentation — 329 titles**
3. **Drama — 328 titles**

This demonstrates the variety of content available on Netflix and highlights the prominence of comedy, documentation, and drama in the dataset.

---

# 📈 Key Findings

| Analysis Area     | Key Finding                                           |
| ----------------- | ----------------------------------------------------- |
| IMDb Ratings      | Identified top and bottom 10 movies and shows         |
| Release Decades   | Significant increase in content from the 2000s onward |
| Age Certification | TV-14 recorded the highest average IMDb score         |
| Movie Genres      | Comedy was the most common movie genre                |
| Show Genres       | Reality was the most common show genre                |
| Overall Genres    | Comedy, Documentation, and Drama were the top three   |

---

# 🎯 Conclusion

This project provides a comprehensive analysis of Netflix's movies and shows dataset.

The analysis identified the **top and bottom-rated movies and shows based on IMDb scores**, providing insights into audience reception.

The examination of content across different decades showed a significant increase in Netflix's library from the **2000s onward**, with particularly strong representation from the 2010s and 2020s.

The analysis of **age certifications** revealed differences in average IMDb scores and demonstrated the diverse range of content available on Netflix.

Finally, the genre analysis showed that **Comedy, Documentation, and Drama** are among the most common genres across Netflix's movies and shows.

Overall, the project demonstrates how **SQL and Tableau can be combined to transform a large dataset into meaningful business insights and interactive visualizations.**

---

## 👤 Project Ownership

**MohankShetra**

This project and its analysis are owned and maintained by **MohankShetra**.

## 📂 Project Structure

```text
Netflix-Shows-and-Movies/
│
├── README.md
├── Netflix_SQL_Analysis.sql
├── Dataset/
│   └── titles.csv
└── Dashboard/
    └── Netflix Shows & Movies Dashboard
```



