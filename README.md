# <p align="center">Netflix Shows and Movies Project</p>
# <p align="center">![Pic](https://i.ibb.co/Q81WwRN/92399716.jpg)</p>

**Tools Used:** Excel, MySQL, Tableau

[Datasets Used](https://www.kaggle.com/datasets/victorsoeiro/netflix-tv-shows-and-movies?select=titles.csv)

[SQL Analysis (Code)](https://github.com/SharifAthar/Netflix-Shows-and-Movies-SQL/blob/main/Netflix_SQL_Analysis.sql)

[Netflix Dashboard - Tableau](https://public.tableau.com/app/profile/sharif.athar/viz/NetflixShowsMoviesDashboard/Dashboard1)

- **Business Problem:** Netflix wants to gather useful insights on their shows and movies for their subscribers through their datasets. The issue is, they are working with too much data (approximately 82k rows of data combined) and are unsure how to effectively analyze and extract meaningful insights from it. They need a robust and scalable data analytics solution to handle the vast amount of data and uncover valuable patterns and trends.

- **How I Plan On Solving the Problem:** In helping Netflix gather valuable insights from their extensive movies and shows dataset, I will be utilizing SQL and a data visualization tool like Tableau to extract relevant information, and conduct insightful analyses. By leveraging SQL's functions, I can uncover key metrics such as viewer ratings, popularity trends, genre preferences, and viewership patterns. Once the data has been extracted and prepared, I will leverage Tableau to present the findings. This will allow for interactive exploration of the data, enabling stakeholders at Netflix to gain actionable insights through visually appealing charts, graphs, and interactive visualizations. I plan on creating a dynamic dashboard in Tableau that enables users to delve into specific movie genres, viewer demographics, or geographical regions.

## Questions I Wanted To Answer From the Dataset:

## 1. Which movies and shows on Netflix ranked in the top 10 and bottom 10 based on their IMDB scores?
- Top 10 Movies
```mysql
SELECT title, 
type, 
imdb_score
FROM shows_movies.titles
WHERE imdb_score >= 8.0
AND type = 'MOVIE'
ORDER BY imdb_score DESC
LIMIT 10
```
Result: 

![Q1](https://i.ibb.co/6mQWCw9/Screen-Shot-2023-07-09-at-9-38-11-PM.png)

- Top 10 Shows
```mysql
SELECT title, 
type, 
imdb_score
FROM shows_movies.titles
WHERE imdb_score >= 8.0
AND type = 'SHOW'
ORDER BY imdb_score DESC
LIMIT 10
```
Result: 

![Q2](https://i.ibb.co/QppHsN2/Screen-Shot-2023-07-09-at-9-45-58-PM.png)

- Bottom 10 Movies
```mysql
SELECT title, 
type, 
imdb_score
FROM shows_movies.titles
WHERE type = 'MOVIE'
ORDER BY imdb_score ASC
LIMIT 10
```
Result: 

![Q3](https://i.ibb.co/tMXV1yp/Screen-Shot-2023-07-09-at-9-47-24-PM.png)

- Bottom 10 Shows
```mysql
SELECT title, 
type, 
imdb_score
FROM shows_movies.titles
WHERE type = 'SHOW'
ORDER BY imdb_score ASC
LIMIT 10
```
Result: 

![Q4](https://i.ibb.co/Y7Qjvg5/Screen-Shot-2023-07-09-at-9-49-36-PM.png)

An IMDB score is a widely recognized measure of the overall quality and popularity of a movie or show. The top 10 movies and shows stood out for their exceptional IMDB scores, indicating that they are highly regarded by viewers. These titles have likely garnered significant acclaim and positive reviews, contributing to their high rankings within the Netflix library. Viewers who are seeking quality content would find these selections very appealing. On the other hand, the bottom 10 movies and shows had lower IMDB scores. While these entries may not have resonated as strongly with audiences, it's important to note that many factors influence these rankings such as individual preferences, weak plot, poor acting, and low-quality production. By uncovering the top and bottom performers based on IMDB scores, this project sheds light on the varying levels of audience reception and highlights titles that are likely to be well-received and those that may have room for improvement. These findings can provide valuable insights for viewers seeking highly-rated content and can serve as a basis for further analysis and decision-making for Netflix's audience recommendations. 

## 2. How many movies and shows fall in each decade in Netflix's library?
```mysql
SELECT CONCAT(FLOOR(release_year / 10) * 10, 's') AS decade,
	COUNT(*) AS movies_shows_count
FROM shows_movies.titles
WHERE release_year >= 1940
GROUP BY CONCAT(FLOOR(release_year / 10) * 10, 's')
ORDER BY decade;
```
Result: 

![Q5](https://i.ibb.co/8dTzVZ3/Screen-Shot-2023-07-09-at-10-02-18-PM.png)

The results of the SQL query provide a fascinating insight into the distribution of movies and shows across different decades in Netflix's library. The data reveals a significant shift in content availability over time, with a notable increase in the number of titles from the 2000s onwards. Starting from the earlier decades, the 1940s-1980s showcase a small fraction of the total entries, suggesting that Netflix's collection from these decades is relatively limited. The 1990s demonstrate a large surge in offerings, with 121 titles. However, the true turning point in Netflix's library occurs in the 2010s with a remarkable 3,304 movies and shows from this decade. This abundance highlights Netflix's dedication to featuring contemporary content that aligns with current trends and audience preferences.

Even though the 2020s are still in progress, the dataset reveals an impressive count of 1,972 movies and shows, indicating a strong focus on acquiring and producing content from recent years. Overall, these findings shed light on Netflix's strategy of curating library that covers a wide range of decades. The significant increase in content availability from the 2000s onwards suggests a concerted effort to offer a diverse selection of titles. This collection spanning multiple decades allows Netflix's audience to explore a variety of movies and shows that reflect different eras. 

## 3. How did age-certifications impact the dataset?
```mysql
SELECT DISTINCT age_certification, 
ROUND(AVG(imdb_score),2) AS avg_imdb_score,
ROUND(AVG(tmdb_score),2) AS avg_tmdb_score
FROM shows_movies.titles
GROUP BY age_certification
ORDER BY avg_imdb_score DESC
```
Result: 

![Q6](https://i.ibb.co/SvJyjgF/Screen-Shot-2023-07-09-at-10-16-52-PM.png)

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
Results: 

![Q7](https://i.ibb.co/T0f5cNq/Screen-Shot-2023-07-09-at-10-20-23-PM.png)

The first query focused on the average IMDB scores associated with each age certification, revealing interesting trends in audience ratings. According to the data, TV-14 emerges as the age certification with the highest average IMDB score of 6.71. This suggests that content designated for viewers aged 14 and older tends to receive relatively favorable ratings. The age certification TV-G obtains an average score of 6.01, signaling the appreciation for content suitable for all audiences. On the other hand, the age certifications R and TV-Y, each with average scores of 6, demonstrate that while they may have lower ratings, there is still a substantial audience that finds enjoyment in these respective categories.

When examining the distribution of movies and shows across age certifications, the second query showcases the varying prevalence of different certifications within Netflix's dataset. R emerges as the most prevalent age certification, with 556 titles falling under this category. PG-13 closely follows with 451 titles, reflecting a significant number of movies and shows targeted at mature audiences. The age certification PG accounts for 233 titles, indicating a considerable selection suitable for general audiences. The dataset also includes 124 titles classified as G, which mostly caters to a younger audience. Lastly, the least represented certification is NC-17, with only 16 titles available. These findings highlight the diverse range of age certifications present in Netflix's movies and shows dataset and provide valuable insights into both audience preferences and content distribution. The higher average scores associated with TV-14, TV-MA, and TV-PG certifications suggest that content aligned with these age categories tends to resonate positively with viewers. 

## 4. Which genres are the most common? 
- Top 10 most common genres for MOVIES
```mysql
SELECT genres, 
COUNT(*) AS title_count
FROM shows_movies.titles 
WHERE type = 'Movie'
GROUP BY genres
ORDER BY title_count DESC
LIMIT 10;
```
Result:

![Q8](https://i.ibb.co/VWrgd8m/Screen-Shot-2023-07-10-at-12-25-40-PM.png)

- Top 10 most common genres for SHOWS
```mysql
SELECT genres, 
COUNT(*) AS title_count
FROM shows_movies.titles 
WHERE type = 'Show'
GROUP BY genres
ORDER BY title_count DESC
LIMIT 10;
```
Result: 

![Q9](https://i.ibb.co/P59s4X7/Screen-Shot-2023-07-10-at-12-27-41-PM.png)

- Top 3 most common genres OVERALL
```mysql
SELECT t.genres, 
COUNT(*) AS genre_count
FROM shows_movies.titles AS t
WHERE t.type = 'Movie' or t.type = 'Show'
GROUP BY t.genres
ORDER BY genre_count DESC
LIMIT 3;
```
Result: 

![Q10](https://i.ibb.co/qMvMBGf/Screen-Shot-2023-07-10-at-12-30-04-PM.png)

By analyzing the frequency of genres, we can gain a better understanding of the content that dominates the platform and the preferences of its audience. Starting with movies, the first query reveals the top 10 most common genres. Comedy emerges as the most popular genre with a total of 384 movies, reflecting its widespread appeal. Following closely behind are documentation with 230 movies and drama with 224 movies, indicating the significance of these genres in Netflix's movie collection. Combinations of genres also feature prominently, with comedy + documentation and comedy + drama occupying the fourth and fifth positions respectively. The presence of drama + romance, drama + comedy, and comedy + romance further emphasizes the audience's likeness for movies that blend multiple genres. These findings highlight the diverse range of movie genres available on Netflix and the platform's commitment to catering to a wide array of preferences.

Shifting the focus, the second query presents the top 10 most common genres for shows. Reality takes the lead with 113 shows, showcasing the popularity of this genre among Netflix viewers. Drama follows closely behind with 104 shows. Comedy and documentation also emerge as prevalent genres with 100 shows each. Similar to movies, combinations of genres such as comedy + drama and drama + romance are present, indicating viewer interest in multi-genre shows. 

Combining the results from both movies and shows, the third query provides an overview of the top three most common genres overall. Comedy takes the lead with a total of 484 entries, reaffirming its position as a very popular genre among Netflix subscribers. Documentation follows closely behind with 329 entries, reflecting the popularity of informative content. Finally, drama secures the third spot with 328 entries. Overall, these findings shed light on the genres that dominate Netflix's library, showcasing the platform's efforts to cater to a diverse range of viewer preferences.

## Conclusion 
By exploring various aspects of the dataset, a comprehensive understanding of Netflix's content landscape was gained. The analysis revealed the top 10 and bottom 10 movies and shows based on their IMDB scores, which highlighted the titles that garnered high praise and those that received lower ratings. This information can assist viewers in making informed choices and highlight areas for potential improvement in content quality. The examination of movies and shows distributed across different decades showed significant shifts in content availability over time. Notably, the dataset showcased a substantial increase in offerings from the 2000s onwards, emphasizing Netflix's commitment to featuring newer content that resonates with current trends and audience preferences.

Age certifications played a crucial role in the dataset, impacting both the average IMDB scores and the distribution of movies and shows. The analysis revealed audience preferences for specific age certifications, with TV-14 garnering the highest average score, suggesting its high popularity among viewers. Furthermore, the different age certifications also showed the diverse range of content available on Netflix. Finally, the exploration of the most common genres in Netflix's library provided insights into viewer preferences and content distribution. Comedy emerged as the dominant genre across both movies and shows, followed by documentation and drama. Combinations of genres were also frequent, highlighting the audience's appreciation for multi-genre content.


