# Google Looker Studio Dashboard for IMDb Movies Dataset

This documentation serves as a comprehensive guide to creating a Google Looker Studio (formerly Data Studio) dashboard using the IMDb Movies dataset. The process includes data preparation, handling missing and non-numeric data, creating computed fields, and developing insightful visualizations. Follow these steps to replicate the project, and use the placeholders for images where applicable.

## Dataset Overview

### About the IMDb Movies Dataset
The dataset contains over 5000 records with key attributes about movies, including:
- **Title**: Movie name.
- **Average Rating**: IMDb average rating.
- **Director**: Movie director(s).
- **Writer**: Movie writer(s).
- **Metascore**: Critic score (some missing data).
- **Cast**: Key actors.
- **Release Date**: Release date of the movie.
- **Country of Origin**: Country(ies) of production.
- **Languages**: Languages used in the movie.
- **Budget**: Movie production budget (some non-numeric values).
- **Worldwide Gross**: Total earnings from worldwide gross (contains missing data).
- **Runtime**: Length of the movie in minutes.

---

## Step 1: Setting Up Google Looker Studio

### 1.1: Create a New Report
1. Open [Google Looker Studio](https://datastudio.google.com/) and log in with your Google account.
2. Click **+ Blank Report** to create a new report.
3. Add your data source (details in Step 2).

### 1.2: Prepare Your Dataset in Google Sheets
1. Upload your IMDb dataset to Google Sheets.
   - Format the columns as: `Title`, `Average Rating`, `Director`, `Writer`, `Metascore`, `Cast`, `Release Date`, `Country of Origin`, `Languages`, `Budget`, `Worldwide Gross`, `Runtime`.
2. Ensure the first row contains column headers.

---

## Step 2: Connect Your Dataset to Looker Studio

1. In Looker Studio, click **Add Data**.
2. Select **Google Sheets** as the data source.
3. Choose the sheet containing the IMDb dataset and click **Connect**.

---

## Step 3: Handling Missing and Non-Numeric Data

### 3.1: Create Computed Fields
To clean and prepare the data, use **calculated fields** in Looker Studio.

#### Replace Missing Values
Use the `IFNULL` function to replace null or missing values.
- **Replace Missing `Average Rating`:**
  ```
  IFNULL(Average Rating, 0)
  ```
- **Replace Missing `Worldwide Gross`:**
  ```
  IFNULL(Worldwide Gross, 0)
  ```

#### Convert Non-Numeric to Numeric
Use `REGEXP_REPLACE` to remove non-numeric characters.
- **Clean `Budget` Field:**
  ```
  REGEXP_REPLACE(Budget, "[^0-9]", "")
  ```

#### Extract Year from Release Date
Use `SUBSTR` to extract the year from the `Release Date` field.
- **Extract Year:**
  ```
  SUBSTR(Release Date, 0, 4)
  ```

#### Calculate Profit
Subtract `Budget` from `Worldwide Gross` to calculate profit.
- **Profit Calculation:**
  ```
  CAST(Worldwide Gross AS NUMBER) - CAST(Budget AS NUMBER)
  ```

#### Group Movies by Budget Range
Use `CASE` to group movies based on their budget range.
- **Budget Categories:**
  ```
  CASE 
    WHEN CAST(Budget AS NUMBER) < 1000000 THEN "Low Budget"
    WHEN CAST(Budget AS NUMBER) BETWEEN 1000000 AND 50000000 THEN "Medium Budget"
    ELSE "High Budget"
  END
  ```

---

## Step 4: Creating Visualizations

### Visualization 1: Rating Distribution (Histogram)
- **Chart Type:** Bar Chart or Histogram
- **Dimension:** Title
- **Metric:** Average Rating
- **Filter:** Exclude movies with `Average Rating = 0`.

### Visualization 2: Gross Earnings vs. Budget (Scatter Plot)
- **Chart Type:** Scatter Plot
- **X-Axis:** Budget
- **Y-Axis:** Worldwide Gross
- **Filter:** Exclude missing values in `Budget` or `Worldwide Gross`.

### Visualization 3: Average Rating by Director (Bar Chart)
- **Chart Type:** Bar Chart
- **Dimension:** Director
- **Metric:** Average Rating
- **Filter:** Limit to top 10 directors by the number of movies.

### Visualization 4: Movie Count by Country (Pie Chart)
- **Chart Type:** Pie Chart
- **Dimension:** Country of Origin
- **Metric:** Count of Title

### Visualization 5: Budget vs. Runtime (Bubble Chart)
- **Chart Type:** Bubble Chart
- **X-Axis:** Runtime
- **Y-Axis:** Budget
- **Bubble Size:** Worldwide Gross
  
### Visualization 6: Average Rating by Release Year (Line Chart)
- **Chart Type:** Line Chart
- **Dimension:** Extracted Year
- **Metric:** Average Rating

---

## Step 5: Styling and Sharing

### Styling the Dashboard
- Use consistent colors, fonts, and sizes for all visualizations.
- Add titles, legends, and descriptions to improve readability.

### Sharing the Dashboard
1. Click **Share** in the top-right corner.
2. Adjust sharing permissions to allow others to view or edit.

---

## Conclusion
This guide provides a step-by-step approach to building a comprehensive Google Looker Studio dashboard using the IMDb Movies dataset. The computed fields and visualizations outlined here enable you to gain insights into movie trends, performance metrics, and more.

Feel free to customize the dashboard and extend it with additional visualizations or computations to suit your analysis needs.
