# Cleaned datasets for Data Science Assignment 2

**Student:** Zeeshan Zahid, 23L-0645  
**Version:** v1.0  


This repository preserves the cleaned Netflix and Supermarket Sales datasets used for CS4048 Assignment 2 and the later Assignment 3. Both source datasets retain every original observation. Amounts and labels retain their original meaning.

## Original sources

- Netflix Movies and TV Shows by Shivam Bansal: https://www.kaggle.com/datasets/shivamb/netflix-shows. The supplied source contains 8,807 rows and 12 columns.
- Supermarket Sales: the instructor-provided CSV, used exactly as supplied, with 1,000 rows and 17 columns. No alternative version was downloaded.

The original authors retain their source-data rights. Source licences and terms continue to apply; this repository does not assert a new licence over their data.

## Versioned cleaned files

| Filename | Rows | Columns | Unique identifier |
| --- | ---: | ---: | --- |
| 23L_0645_A2_netflix_cleaned.csv | 8,807 | 22 | show_id |
| 23L_0645_A2_supermarket_cleaned.csv | 1,000 | 24 | Invoice ID |

The v1.0 tag identifies the exact cleaned file version. The SHA-256 hashes in checksums.sha256 verify the CSV bytes. Future cleaning changes should receive a new version rather than replacing v1.0.

## Netflix cleaning summary

- Trimmed string edges, removed empty tokens and standardised comma-space separators in multi-valued metadata without merging different valid labels.
- Moved three misplaced duration strings (74 min, 84 min and 66 min) from rating into the otherwise missing movie duration. The true ratings remain missing; seven ratings are unknown after repair.
- Parsed date_added. All nonmissing dates parse, while 10 absent dates remain absent.
- Preserved missing directors, cast and countries rather than inventing identities or deleting titles.
- Added added_year, added_month, movie_minutes, tv_seasons, country_count, genre_count, release_decade, addition_lag_years, negative_lag_flag and valid_addition_lag_years.
- Kept movie minutes separate from TV seasons. Missing duration in the opposite content type is structural.
- Retained 14 negative year gaps in addition_lag_years, flagged those rows, and masked only valid_addition_lag_years. Missing addition dates also yield missing gaps.
- Retained all valid older titles, unusual movie lengths and multi-season shows flagged by the 1.5 times IQR rule.

## Supermarket cleaning summary

- Found no missing values, repeated invoices, full-row duplicates, category whitespace/case conflicts or invalid date/time values. No values were imputed or transactions removed.
- Parsed Date with an explicit month/day/year format and standardised Time as HH:MM.
- Added transaction_datetime, hour, weekday, weekday_number, month, year_month and is_weekend.
- Preserved all recorded numerical amounts. Recomputed cost, tax, total, gross income and gross margin relationships; all five checks passed their stated tolerances.
- Retained nine IQR-flagged transaction totals because the recorded price and quantity explain their amounts.
- Documented that Branch and City map one-to-one, gross margin is constant, and several financial fields overlap by definition.

## Loading notes for Assignment 3

CSV does not store pandas data types. Parse Netflix date_added and Supermarket Date/transaction_datetime when loading. Empty Netflix cells mean unknown metadata or structural missingness; they are not the literal category Unknown. The catalogue addition window ends on 25 September 2021. Supermarket covers 1 January to 30 March 2019, so these files do not establish present-day behaviour or annual seasonality.
