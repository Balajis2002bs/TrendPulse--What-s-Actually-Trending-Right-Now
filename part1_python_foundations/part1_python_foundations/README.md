#  Trending Videos Analysis

## About the Project

This project is a hands-on analysis of  trending video data from
the United States, Great Britain and India.

The main idea was to work with real-world CSV data and practise Python
fundamentals while exploring how videos perform across different
countries. Along the way, I worked on loading and cleaning the data,
using Python data structures, writing loops and functions, and applying
a few rules to classify video performance.

This is mainly a Python foundations project, with the analysis built
around the trending video records.

## What the project covers

-   Loading video data from CSV files for the US, GB and IN.
-   Checking that important columns are available before processing the
    records.
-   Cleaning values and converting numeric, date/time, text and boolean
    fields into suitable formats.
-   Using lists and dictionaries to organise the records.
-   Using tuples to keep selected video fields together.
-   Using sets to find videos that trend in more than one country, as
    well as videos unique to a country.
-   Counting channel appearances and displaying the 15 channels with the
    most appearances.
-   Creating a country-wise summary of video categories.
-   Practising local and global variable scope.
-   Extracting simple features from video titles.
-   Classifying videos using a set of engagement rules.
-   Refactoring repeated integer conversion logic into a helper
    function.

## Dataset

The notebook expects the following files:

``` text
data/
├── USvideos.csv
├── GBvideos.csv
├── INvideos.csv
└── categories.csv
```

The three country files contain the video records. The `categories.csv`
file is used to map each category ID to its category name.

The code checks for these required video columns:

-   `video_id`
-   `views`
-   `likes`
-   `dislikes`
-   `comment_count`

Other fields used during processing include publishing and trending
dates, title, channel name, tags, thumbnail link, category ID and
comment/rating status fields.

## Tools and libraries

-   Python
-   Pandas
-   `datetime` from Python's standard library
-   `random` from Python's standard library

## How the notebook works

### 1. Setup and configuration

The notebook defines the countries to process (`US`, `GB` and `IN`) and
a minimum-view setting. It also prints the start time and the configured
country list.

### 2. Loading the data

A `load_country()` function reads a country's CSV file, adds a `country`
field to each row and checks that the required columns are present. The
rows are then converted into a list of dictionaries so they can be
processed using normal Python operations.

The category file is loaded separately and converted into a dictionary
that maps category IDs to category names.

### 3. Cleaning and type conversion

The notebook processes each record and attempts to convert:

-   Views, likes, category ID, dislikes and comment count into integers.
-   Publishing time and trending date into `datetime` values.
-   Text fields such as video ID, title, channel title, tags and
    thumbnail link into strings.
-   Comment and rating status fields into boolean values.

When a numeric value cannot be converted, the code uses `0` as the
fallback. Invalid date values are set to `None`. The notebook also keeps
failure counts for fields where conversion or validation fails.

### 4. Working with Python data structures

Different data structures are used for different tasks:

-   **Lists of dictionaries:** Keep the video records organised and easy
    to access.
-   **Tuples:** Store selected fields (`video_id`, `trending_date`,
    `views` and `likes`) together.
-   **Sets:** Find common and country-specific video IDs using set
    operations.
-   **Dictionaries:** Count channel appearances and build country-wise
    category summaries.

The notebook also demonstrates the difference between mutable data
structures (lists, dictionaries and sets) and immutable data structures
(tuples).

### 5. Comparing videos across countries

The video IDs are collected into separate sets for the US, GB and IN.
Set intersections are used to find IDs shared by two countries. Set
differences are used to identify IDs that are unique to each country.

The notebook also counts how often each channel appears and prints the
top 15 channels by appearance count.

### 6. Category summary

Using the category ID-to-name mapping, the notebook builds a nested
dictionary in this format:

``` text
country
  └── category
        └── record count
```

This makes it possible to see how many trending records belong to each
category in each country.

### 7. Scope and title features

A small example demonstrates local and global variable scope. It shows
that a variable created inside a function cannot normally be accessed
outside that function.

The `title_features()` function works with a video title and returns:

-   Number of uppercase words.
-   Number of exclamation marks.
-   Number of question marks.
-   Total word count.

It also uses `for`, `range()` and `while` loops, along with `continue`
and `break`.

### 8. Rule-based classification

Each video record is assigned one of the following labels based on its
views, like ratio and rating status:

  -----------------------------------------------------------------------
  Label                               Rule used in the notebook
  ----------------------------------- -----------------------------------
  Viral Hit                           Views are at least 1,000,000 and
                                      like ratio is greater than 0.9

  Steady Performer                    Views are greater than 100,000 and
                                      like ratio is greater than 0.7

  Divisive                            Views are greater than 50,000 and
                                      like ratio is less than 0.6

  Low Engagement                      Views are less than 50,000

  Unrated                             No views, no likes/dislikes,
                                      disabled ratings, or no other rule
                                      matched
  -----------------------------------------------------------------------

The like ratio is calculated as:

``` python
likes / (likes + dislikes)
```

If the views are zero or the total of likes and dislikes is zero, the
record is labelled `Unrated` before the ratio is calculated.

After classification, the notebook counts the number of videos in each
label for each country.

### 9. Refactoring

The notebook introduces an `int_conversion()` helper function to handle
integer conversion in one place. It accepts a record, a key and a
default value. If conversion fails, it returns the default instead of
repeating the same conversion logic throughout the code.

## Running the project

1.  Download or clone the project.
2.  Keep the CSV files in the expected `data/` directory.
3.  Open `trend_analysis.ipynb` in Jupyter Notebook, JupyterLab or
    Google Colab.
4.  Check the file paths in the setup and loading sections. Update them
    if your dataset is stored somewhere else.
5.  Run the notebook from top to bottom. Using **Restart and Run All**
    helps make sure the results do not depend on variables left over
    from an earlier run.

## A note on the current implementation

This README describes the code currently present in the notebook. A few
configuration and naming details may need attention before running it in
a fresh environment:

-   `data_path` is defined in the setup section, but the loading loop
    currently uses relative paths such as `data/USvideos.csv`.
-   `min_views` is defined but is not currently applied as a filter.
-   `max_rows` is accepted by `load_country()` but is not currently used
    to limit the rows.
-   The cleaning code uses the field name `ratings_disabled`, while the
    classification function checks `rating_disabled`. These names should
    be made consistent if the rating-disabled condition is expected to
    work.
-   The notebook imports `random`, but it is not currently used.

## What I practised

This project gave me a chance to apply Python basics to a dataset with
multiple country files instead of working only with small examples. I
got practice with file loading, data validation, type conversion, data
structures, loops, functions, set operations, dictionary-based summaries
and rule-based classification.
