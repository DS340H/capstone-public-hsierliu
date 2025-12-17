# Analyzing Emotions in #SAHM TikTok Videos

This repository contains datasets and code for a Data Science capstone project, analyzing how emotions in #StayAtHomeMom TikTok videos relate to viewer engagement, and whether relatability changes (moderates) or explains (mediates) this relationship across five parenting stages. This repository was last updated *December 17th, 2025*.


> [!NOTE]
> To protect privacy, direct identifiers were removed for all datasets in this public repository (e.g., original usernames, post IDs, and free-text descriptions). Creator usernames are replaced with anonymous IDs of the form `anon_####`.



## Repository structure

```bash
final-project-hsierliu/
├── 1-variables.ipynb               # dataset construction & feature engineering (exports data/finalD.csv)
├── 2-analyses.ipynb                # analyses + visualization (exports figures.png)
├── data/
│   ├── initialD.jsonl              # original dataset with 2M+ videos
│   ├── data0.csv                   # filtered for 2024, US, and at least minimum activity videos
│   ├── data1.csv                   # initial dataset with 1,000 videos per age group; 5,000 total
│   ├── data2.csv                   # data1 + engagement
│   ├── data3.csv                   # data2 + relatability label/score
│   ├── data4.csv                   # data3 + emotions 
│   ├── finalD.csv                  # final analysis dataset used in 2-analyses.ipynb
│   ├── test_relatability.csv       # test run (first 10 rows) of relatability classification
│   └── test_emotion.csv            # test run (first 10 rows) of emotion classification
├── figures/
│   ├── figure1.png                 # exploratory emotion profiles by age group
│   └── figure2.png                 # emotion → engagement effects (by age group)
└── README.md                       # final poster (added to repo root separately)
```

## Roadmap

- **Create datasets** with `1-variables.ipynb`:
  - Reads `data/initialD.jsonl`, filters based on set criteria, and exports `data/data0.csv`.
  - Assigns `age_group` based on exclusive hashtags, samples 1,000 videos per group, and exports `data/data1.csv`.
  - Computes `engagement` and exports `data/data2.csv`.
  - Scores relatability (label + confidence) and exports `data/data3.csv`.
  - Scores emotion probabilities and exports `data/data4.csv`.
  - Drops non-analysis columns and exports `data/finalD.csv`.
- **Run analyses** with `2-analyses.ipynb`:
  - Loads `data/finalD.csv`
  - Generates `figures/figure1.png` (exploratory emotion profiles by age group)
  - Generates `figures/figure2.png` (emotion → engagement effects)
  - Includes moderator and mediation analyses involving `relatability_score`

## Descriptions of variables for final dataset (`finalD.csv`)

<details>
  <summary><strong>username</strong></summary>

  - **Description**: anonymized creator identifier of the form `anon_####`.

  - **Example**: `anon_0001`
</details>

<details>
  <summary><strong>age_group</strong></summary>

  - **Description**: Classified each video into one of five age-groups based on hashtags. 
    - `age1`: Pregnancy (pre-birth)
    - `age2`: Newborn/Infant (0–1)
    - `age3`: Toddler/Preschool (1–5)
    - `age4`: Elementary/Preteen (5–11)
    - `age5`: Teen (12–18)

  - **Hashtags used for assignment (exclusion rules apply)**:
    - `age1` (pregnancy): `pregnancy`, `pregnant`, `pregnantlife`, `thirdtrimester`, `pregnancyjourney`, `pregnanttiktok`
    - `age2` (newborn/infant): `newborn`, `newbornbaby`, `baby`, `4monthsold`
    - `age3` (toddler/preschool): `toddler`, `toddlermom`, `toddlertok`, `toddlersoftiktok`, `toddleractivities`, `toddlermama`, `preschool`
    - `age4` (elementary/preteen): `kindergarten`, `kindergartener`, `elementary`, `elementaryteacher`, `elementaryschool`, `firstgrade`, `secondgrade`, `preteen`, `kindergartenlife`, `5thgrade`, `kindergartenmom`, `kindergartenteacher`, `homeschoolkindergarten`, `kindergartenhomeschool`, `elementaryeducation`, `preteenmom`, `elementarymusic`, `4thgrade`
    - `age5` (teen): `teens`, `teen`, `teenager`, `teenagers`, `middleschool`, `highschool`, `momofteens`, `raisingteens`, `teenagersbelike`, `momsofteens`, `teensoftiktok`

  - **Example**: A video is classified as "age1" if they contain at least one pregnancy hashtag (e.g. #pregnancy) and do not contain hashtags in any of the other age group (e.g., no #newborn hashtag).
</details>

<details>
  <summary><strong>engagement</strong></summary>

  - **Description**: computed as (likes + comments + shares) / views.

  - **Example**: If a video has 120 likes, 30 comments, 0 shares, and 500 views, then engagement is 150/500 = 0.3.
</details>

<details>
  <summary><strong>relatability_score</strong></summary>

  - **Description**: Labels each video from 0 to 1, depending on how relatable the video description is, using a zero-shot classification model (HuggingFace `typeform/distilbert-base-uncased-mnli`) and provided labels.

  - **Example**: a row with `relatability_score = 0.98` indicates high relatability.

</details>

<details>
  <summary><strong>Emotion probability columns (28)</strong></summary>

  - **Description**: Labels each viideo from 0 to 1 for each of the 28 available emotions based on the video descriptions, using a text-classification model (HuggingFace `cirimus/modernbert-base-go-emotions`)

  - **28 Labels**: `admiration`, `amusement`, `anger`, `annoyance`, `approval`, `caring`, `confusion`, `curiosity`, `desire`, `disappointment`, `disapproval`, `disgust`, `embarrassment`, `excitement`, `fear`, `gratitude`, `grief`, `joy`, `love`, `nervousness`, `optimism`, `pride`, `realization`, `relief`, `remorse`, `sadness`, `surprise`, `neutral`.

  - **Example**: a caption with affectionate language typically yields a higher score on `love` relative to other emotion columns
</details>
