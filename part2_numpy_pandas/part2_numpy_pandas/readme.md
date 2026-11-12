#  Trending Data Analysis

This project explores  trending videos across three countries — **US, India, and Great Britain** — using **NumPy** and **Pandas**.  


---

##  Features Implemented

### 1. NumPy Arrays
- Loaded `views`, `likes`, `dislikes`, and `comment_count` into NumPy arrays.
- Reported **shape** and **dtype** for each.
- Practiced indexing and slicing:
  - Top 10 largest view counts.
  - Every 100th record.
  - Sub‑array combining likes and dislikes.

### 2. Engagement Score (Vectorized, No Loops)
- **Like ratio**: `likes / (likes + dislikes)` with safe handling of zero denominators.
- **Comment rate**: `comments ÷ views`, safely handling zero views.
- **Min‑max normalization** for views, likes, and comments.
- **Engagement score formula**: (views * 0.5) + (likes * 0.3) + (comment rate * 0.2)
  

  > Justification: Views carry the most weight, likes indicate approval, and comments reflect interaction.
- **Log‑scaled views** (`np.log1p`) to handle skewed distributions (raw views are hard to plot due to extreme outliers).
- Boolean masks to flag **top 10% records** by engagement score.

### 3. Reshape & Broadcasting
- Built a `(days × countries)` array of daily trending counts.
- Used broadcasting to compute:
  - Each country’s share of daily trending videos.
  - Each day’s value relative to that country’s average.

### 4. Data Loading & Inspection
- Loaded CSVs with `pd.read_csv()`.
- Added a **country column** to each dataset.
- Combined datasets with `concat()`.
- Inspected with `head()`, `info()`, `describe()`, and `shape`.

### 5. Data Quality Handling
- **Issues found**:
  - Missing descriptions → filled with `"Unknown"`.
  - Nulls in likes/comments → filled with `0`.
  - Duplicate rows → dropped.
  - Removed/errored videos → counted.
  - Ratings/comments disabled → counted.
  - Date formats inconsistent → converted to proper `datetime`.
- All issues handled before analysis.

### 6. Filtering
- Records with **1M+ views and 50k+ likes**.
- Top 20 records by **engagement score**.
- Music/Gaming records with views above the overall median.

### 7. Missing Values & Duplicates
- Reported counts with `isna()`.
- Applied `fillna()` strategies (0 for numeric, `"Unknown"` for text).
- Dropped duplicates (`drop_duplicates()`).
- Defined duplicates as **same video_id + trending_date + country**.

### 8. Joining Category Names
- Loaded `categories.csv`.
- Merged with trending data using **inner join** (ensures only valid categories are kept).
- Verified no records lost.

### 9. Aggregation
- Grouped by **country & category**:
  - Average views, likes, engagement score.
- Category distribution:
  - Counts and percentages.
- Publishing analysis:
  - Found which **day of week** and **hour** produced highest views.

### 10. Time to Trend & Lifespan
- Computed `days_to_trend` (publish → trending).
- Distribution described.
- Trending lifespan:
  - Distinct trending days per video.
  - Top 10 longest‑lasting videos.
  - Average lifespan per category.

### 11. Channel & Video Summary
- Per‑video summary: total views, trending days.
- Per‑channel summary: number of trending videos, total views.
- Merged both to show **channel performance**.

---

## 📈 Key Insights
- Engagement score highlights videos with balanced popularity and interaction.
- Log scaling is essential for plotting views due to extreme skew.
- Music and Gaming dominate trending lists, but median filtering reveals standout performers.
- Certain days/hours consistently yield higher views — useful for upload strategies.
- Lifespan analysis shows which videos sustain popularity beyond initial trending.

---

## 🛠️ Tech Stack
- **Python**
- **NumPy**
- **Pandas**


