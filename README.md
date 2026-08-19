# Hockey-Web-Scraping-and-Pandas-Data-Analysis
## Introduction

This project demonstrates an end-to-end workflow for collecting publicly available hockey statistics from a webpage, transforming the scraped HTML into a structured Pandas DataFrame, exporting the data to CSV, and performing exploratory analysis.

The project uses Python, Requests, BeautifulSoup, and Pandas. The source is the hockey statistics section of Scrape This Site, with the analysis focused on page 3 of the dataset.

The resulting dataset contains 25 hockey team records and 9 variables covering team identity, year, wins, losses, win percentage, goals scored, goals conceded, and goal differential.

## 1. Story of Data

### Data Source

The data was collected from the hockey section of Scrape This Site, a public sandbox designed for learning web scraping.

The project targeted:

`https://www.scrapethissite.com/pages/forms/?page_num=3`

The use of `page_num=3` means that the project intentionally scraped one page rather than crawling the entire website.

### Data Collection Process

The workflow followed these steps:

1. Import the required Python libraries.
2. Define the target URL.
3. Send an HTTP request using Requests.
4. Retrieve the HTML response.
5. Parse the HTML using BeautifulSoup.
6. Locate the hockey statistics table.
7. Extract the table headers.
8. Extract the table rows.
9. Extract individual table cells.
10. Clean extracted text using `.strip()`.
11. Create a Pandas DataFrame.
12. Add the scraped records.
13. Inspect the completed DataFrame.
14. Export the resulting dataset to CSV.

### Dataset Structure

The dataset contains nine columns:

| Column | Description |
|---|---|
| Team Name | Name of the hockey team |
| Year | Season or year represented |
| Wins | Number of games won |
| Losses | Number of games lost |
| OT Losses | Overtime losses |
| Win % | Winning percentage |
| Goals For (GF) | Goals scored |
| Goals Against (GA) | Goals conceded |
| + / - | Goal differential |

The dataset contains records from 1992 and 1993.

One notable data-quality characteristic is that the OT Losses field is blank for all 25 records.

## 2. Data Splitting and Preprocessing

### HTML Structure

The source page stores the hockey statistics inside an HTML table. The scraper works with:

- `<table>` for the overall table
- `<tr>` for table rows
- `<th>` for table headers
- `<td>` for individual data cells

### Header Extraction

The table headers are extracted using BeautifulSoup and cleaned with `.strip()`.

```python
hockey_team = table.find_all('th')

hockey_team_table = [
    team.text.strip()
    for team in hockey_team
]
```

### Row and Cell Extraction

The first table row contains the headers, so it is skipped when extracting records.

```python
column_data = table.find_all('tr')

for row in column_data[1:]:
    row_data = row.find_all('td')

    individual_row_data = [
        data.text.strip()
        for data in row_data
    ]
```

### Handling Missing Values

The OT Losses field contains blank values across the dataset.

The blanks should not automatically be converted to zero because a blank could represent missing, unavailable, not applicable, or omitted information. The meaning of the missing field should be investigated before any imputation is performed.

### DataFrame Construction

The extracted headers are used to create the DataFrame:

```python
df = pd.DataFrame(columns=hockey_team_table)
```

Records are then added to the DataFrame.

### Exporting the Dataset

The completed DataFrame is exported using:

```python
df.to_csv("Hockey.csv", index=False)
```

## 3. Pre-Analysis

### Dataset Dimensions

The dataset contains:

- 25 rows
- 9 columns
- 25 unique team records
- 2 years: 1992 and 1993

The dataset is relatively small and is therefore best suited for descriptive and exploratory analysis.

### Data Types

| Variable | Data Type |
|---|---|
| Team Name | Object/String |
| Year | Integer |
| Wins | Integer |
| Losses | Integer |
| OT Losses | Float/Missing |
| Win % | Float |
| Goals For | Integer |
| Goals Against | Integer |
| + / - | Integer |

### Completeness

The main data-quality issue is the OT Losses column, which has zero populated values.

The other analytical fields are populated across the 25 records.

### Descriptive Statistics

| Metric | Result |
|---|---:|
| Average Wins | 37.68 |
| Average Losses | 37.48 |
| Average Win % | 44.86% |
| Maximum Wins | 56 |
| Minimum Wins | 10 |
| Maximum Win % | 66.7% |
| Minimum Win % | 11.9% |

### Analysis Questions

The analysis was guided by questions including:

1. Which team recorded the highest number of wins?
2. Which team had the highest win percentage?
3. Which team had the strongest goal differential?
4. Which team conceded the most goals?
5. Which teams demonstrated positive goal differentials?
6. How does winning performance relate to goal differential?
7. How do the 1992 and 1993 records compare?
8. Which teams appear strongest and weakest in the selected sample?

## 4. In-Analysis

### Ranking Teams by Wins

The Pittsburgh Penguins recorded the highest number of wins:

- 56 wins
- 21 losses
- 66.7% win percentage
- +99 goal differential

The Ottawa Senators recorded the lowest number of wins:

- 10 wins
- 70 losses
- 11.9% win percentage
- -193 goal differential

### Win Percentage

Pittsburgh recorded the highest win percentage at 66.7%.

Ottawa recorded the lowest at 11.9%.

### Goals Scored

The Pittsburgh Penguins recorded the highest Goals For figure at 367.

Other strong offensive performances included:

- Detroit Red Wings: 356
- Quebec Nordiques: 351
- Vancouver Canucks: 346
- Los Angeles Kings: 338
- New York Islanders: 335

### Goals Conceded

The San Jose Sharks recorded the highest Goals Against figure at 414.

The Ottawa Senators also conceded heavily with 395 goals.

The Toronto Maple Leafs recorded 241 goals against in the selected sample.

### Goal Differential

The strongest goal differential was recorded by the Pittsburgh Penguins at +99.

The weakest was recorded by the San Jose Sharks at -196.

### Positive and Negative Goal Differentials

Among the 25 records:

- 16 teams, or 64%, had positive goal differentials.
- 8 teams, or 32%, had negative goal differentials.
- 1 team, or 4%, had a zero differential.

### Wins and Goal Differential

Several strong-performing teams combined high win totals with positive goal differentials.

| Team | Wins | Win % | Goal Differential |
|---|---:|---:|---:|
| Pittsburgh Penguins | 56 | 66.7% | +99 |
| Quebec Nordiques | 47 | 56.0% | +51 |
| Vancouver Canucks | 46 | 54.8% | +68 |
| Detroit Red Wings | 46 | 54.8% | +81 |
| Toronto Maple Leafs | 44 | 52.4% | +47 |

The dataset suggests that stronger winning records generally coincide with positive goal differentials.

## 5. Post-Analysis and Insights

### Insight 1: Pittsburgh Penguins Were the Strongest Record

The Pittsburgh Penguins were the standout team in the selected sample.

They recorded the highest:

- Number of wins
- Win percentage
- Goals For
- Goal differential

This consistency across multiple indicators strengthens the conclusion that Pittsburgh was the strongest performer within this dataset.

### Insight 2: Ottawa and San Jose Show Severe Underperformance

The Ottawa Senators and San Jose Sharks demonstrate the weakest performance patterns.

Ottawa recorded only 10 wins and a -193 goal differential.

San Jose recorded 11 wins and a -196 goal differential.

Both teams combined poor win records with substantial negative goal differentials.

### Insight 3: Goal Differential Reinforces Win Performance

The strongest teams generally recorded positive goal differentials.

This suggests that successful teams were not simply winning narrowly. Many were also creating a substantial difference between goals scored and goals conceded.

### Insight 4: Offensive Strength Is Visible Among Top Teams

Pittsburgh, the highest-performing team in the sample, also recorded the highest Goals For figure at 367.

Detroit recorded 356, Quebec 351, and Vancouver 346.

The data suggests that offensive production is an important part of the performance story, although the dataset does not establish that scoring alone causes winning.

### Insight 5: Defensive Performance Matters

San Jose conceded 414 goals, the highest figure in the dataset.

Ottawa conceded 395 goals.

These high Goals Against values contributed to their highly negative goal differentials.

### Insight 6: Data Quality Needs Attention

The scraping process successfully produced a structured dataset, but the OT Losses column is completely blank.

This demonstrates that successful data extraction does not automatically produce a completely analysis-ready dataset.

The data must still be inspected, validated, cleaned, and understood before analysis.

## 6. Data Visualizations & Charts

The dataset supports several useful visualizations.

### Wins by Team

A horizontal bar chart can rank teams according to the number of wins.

The chart should use:

- X-axis: Wins
- Y-axis: Team Name

Pittsburgh would appear at the top with 56 wins, while Ottawa would appear at the bottom with 10 wins.

### Win Percentage by Team

A horizontal bar chart can rank teams by Win %.

Suggested title:

**Hockey Team Performance by Win Percentage**

### Goals For vs Goals Against

A scatter plot can compare Goals For and Goals Against for each team.

This allows viewers to identify teams that:

- Scored more than they conceded
- Conceded more than they scored
- Had relatively balanced scoring

### Goal Differential

A diverging bar chart can show positive and negative goal differentials around a zero baseline.

This makes the contrast between Pittsburgh at +99 and San Jose at -196 immediately visible.

### Wins vs Goal Differential

A scatter plot can compare:

- X-axis: Wins
- Y-axis: Goal Differential

Each point represents a team.

This visualization can help investigate whether teams with more wins also tend to have stronger goal differentials.

### Correlation Heatmap

A correlation heatmap can be used to examine relationships among numerical variables such as Wins, Losses, Win %, Goals For, Goals Against, and Goal Differential.

### Average Goals by Year

Because the dataset contains records from 1992 and 1993, average Goals For and Goals Against can be compared by year.

### Suggested Dashboard

A simple analytical dashboard could include the following KPI cards:

- Total Teams: 25
- Average Wins: 37.68
- Average Win %: 44.86%
- Highest Wins: 56
- Highest Goal Differential: +99
- OT Losses Populated: 0/25

Recommended charts:

1. Wins by Team
2. Win % by Team
3. Goals For vs Goals Against
4. Goal Differential
5. Wins vs Goal Differential

## 7. Recommendations and Observations

### Recommendation 1: Validate Scraped Data Before Analysis

A scraping pipeline should include automated checks for:

- HTTP status codes
- Expected table existence
- Expected number of columns
- Expected number of rows
- Missing values
- Data types

Example:

```python
print(df.shape)
print(df.columns)
print(df.dtypes)
print(df.isna().sum())
```

### Recommendation 2: Investigate Missing OT Losses

The OT Losses field should be investigated before it is used in further analysis.

The analyst should determine whether:

- The source does not provide the information
- The field is not applicable
- The scraper failed to extract it
- The source HTML intentionally contains blank cells

### Recommendation 3: Improve Scraper Reliability

The scraper can be strengthened through:

- Exception handling
- Request timeouts
- Appropriate user-agent headers
- Target-table validation
- Automatic pagination
- Duplicate detection

A basic HTTP response check could be implemented as:

```python
response = requests.get(url)

if response.status_code == 200:
    ...
else:
    print("Request failed")
```

### Recommendation 4: Automate Pagination

The current project retrieves only page 3.

A more advanced scraper could loop through multiple pages:

```python
for page_num in range(1, 14):
    url = f"https://www.scrapethissite.com/pages/forms/?page_num={page_num}"
```

This would allow the project to become a larger data-collection pipeline.

### Recommendation 5: Preserve Raw and Clean Data Separately

A robust workflow should maintain:

**Raw data → Cleaned data → Analysis dataset**

This makes it possible to review or reproduce cleaning decisions.

### Recommendation 6: Use Pandas for Post-Scraping Analysis

Pandas can be used to:

- Filter teams
- Sort performance
- Calculate averages
- Group by year
- Identify top performers
- Calculate correlations
- Create analytical tables
- Export processed datasets

Web scraping and Pandas therefore work together as complementary stages of a data-analysis workflow.

## 8. Conclusion

This project demonstrates a complete introductory workflow for transforming publicly available web information into a structured analytical dataset.

The project begins with a hockey statistics webpage and uses Requests to retrieve the HTML, BeautifulSoup to navigate the page structure, and Pandas to create and manage the resulting DataFrame.

The final dataset contains 25 records and 9 fields covering team identity, season, wins, losses, win percentage, goals scored, goals conceded, and goal differential.

The analysis identifies the Pittsburgh Penguins as the strongest team in the selected sample, with 56 wins, a 66.7% win percentage, 367 goals scored, and a +99 goal differential.

At the other end, Ottawa and San Jose show particularly weak performance, with low win totals and highly negative goal differentials.

The project also demonstrates an important data-analysis lesson: data extraction is only the beginning.

Once data has been scraped, it must be inspected, validated, cleaned, analyzed, visualized, and communicated effectively.

The project provides a strong foundation for progressing from basic HTML scraping toward more advanced Python workflows involving automated pagination, data cleaning, exploratory data analysis, visualization, and reproducible pipelines.

## 9. References & Appendices

### References

#### Primary Project Files

**Webscraping Test (1).ipynb**

Primary notebook containing the scraping workflow, including Requests, BeautifulSoup, HTML table selection, header extraction, row extraction, DataFrame construction, and CSV export.

**Hockey.csv**

Exported dataset containing 25 hockey team records and 9 fields.

**Pandas Study Documentation: Hockey Web Scraping Project**

Supporting study documentation developed from the notebook and dataset.

#### Data Source

The project retrieves hockey statistics from the hockey statistics sandbox hosted by Scrape This Site.

Source URL:

`https://www.scrapethissite.com/pages/forms/?page_num=3`

### Appendix A: Complete Scraping Workflow

```python
from bs4 import BeautifulSoup
import requests
import pandas as pd

# 1. Define URL
url = 'https://www.scrapethissite.com/pages/forms/?page_num=3'

# 2. Retrieve page
page = requests.get(url)

# 3. Parse HTML
soup = BeautifulSoup(page.text, 'html')

# 4. Locate table
table = soup.find('table')

# 5. Extract headers
hockey_team = table.find_all('th')

hockey_team_table = [
    team.text.strip()
    for team in hockey_team
]

# 6. Create DataFrame
df = pd.DataFrame(columns=hockey_team_table)

# 7. Extract rows
column_data = table.find_all('tr')

for row in column_data[1:]:
    row_data = row.find_all('td')

    individual_row_data = [
        data.text.strip()
        for data in row_data
    ]

    length = len(df)
    df.loc[length] = individual_row_data

# 8. Inspect
print(df)

# 9. Export
df.to_csv("Hockey.csv", index=False)
```

### Appendix B: Recommended Pandas Validation

```python
# Dataset dimensions
df.shape

# Column names
df.columns

# First five records
df.head()

# Data types
df.dtypes

# Missing values
df.isna().sum()

# Summary statistics
df.describe()
```

### Appendix C: Recommended Analytical Queries

#### Top Teams by Wins

```python
df.sort_values(
    "Wins",
    ascending=False
)[["Team Name", "Year", "Wins"]]
```

#### Top Teams by Win Percentage

```python
df.sort_values(
    "Win %",
    ascending=False
)[["Team Name", "Year", "Win %"]]
```

#### Strongest Goal Differential

```python
df.sort_values(
    "+ / -",
    ascending=False
)[["Team Name", "Year", "+ / -"]]
```

#### Teams with Negative Goal Differential

```python
df[df["+ / -"] < 0]
```

#### Average Performance

```python
df[[
    "Wins",
    "Losses",
    "Win %",
    "Goals For (GF)",
    "Goals Against (GA)",
    "+ / -"
]].mean()
```

### Appendix D: Key Project Findings

| Finding | Result |
|---|---:|
| Total records | 25 |
| Total fields | 9 |
| Years represented | 1992–1993 |
| Average wins | 37.68 |
| Average losses | 37.48 |
| Average win percentage | 44.86% |
| Highest wins | Pittsburgh Penguins — 56 |
| Highest win percentage | Pittsburgh Penguins — 66.7% |
| Highest Goals For | Pittsburgh Penguins — 367 |
| Highest goal differential | Pittsburgh Penguins — +99 |
| Lowest wins | Ottawa Senators — 10 |
| Lowest win percentage | Ottawa Senators — 11.9% |
| Lowest goal differential | San Jose Sharks — -196 |
| Highest Goals Against | San Jose Sharks — 414 |
| OT Losses populated | 0 |

## Project Takeaway

The project demonstrates the transition from:

**Unstructured web content → Parsed HTML → Structured Pandas DataFrame → CSV dataset → Analytical insight**

The central lesson is that web scraping is not the end of the data journey. It is the beginning of a broader process involving preprocessing, validation, analysis, visualization, and communication.

