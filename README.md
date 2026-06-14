# Employer Branding vs Product-Focused Content on Swiss Corporate YouTube Channels

This repository contains the full notebook workflow for an RPV course project on corporate YouTube communication.

## Research Question

To what extent do employer-branding videos on Swiss corporate YouTube channels generate more audience conversation than product- and technology-focused videos?

## Project Summary

The project compares two types of corporate YouTube content:

- employer-branding videos
- product- and technology-focused videos

The workflow starts with MongoDB-based classification, removes ambiguous overlap between categories, and then evaluates audience conversation with descriptive statistics, normalized engagement measures, significance tests, and robustness checks.

The repository already includes exported JSON datasets, so you can inspect most of the workflow without reconnecting to MongoDB. MongoDB access is only required if you want to rerun the initial classification step from the source collection.

## Repository Overview

### Notebooks

Run the notebooks in this order:

1. [`notebooks/01-classification.ipynb`](notebooks/01-classification.ipynb)  
   Connects to the MongoDB `youtube.videos` collection, extracts frequent tags, applies the classification rules, and exports the initial employer-branding and product-technology datasets.

2. [`notebooks/02-classification-validation.ipynb`](notebooks/02-classification-validation.ipynb)  
   Validates the two exported datasets, identifies videos that appear in both groups, removes those overlaps, and writes the cleaned comparison datasets.

3. [`notebooks/03-audience-conversation-analysis.ipynb`](notebooks/03-audience-conversation-analysis.ipynb)  
   Answers the research question using the cleaned datasets with descriptive summaries, normalized conversation measures, distribution plots, statistical tests, regression-style controls, and a company-year robustness check.

### Data Files

The `data/` folder contains the exported datasets used throughout the workflow:

- `data/employee-based-content.json`: initial employer-branding classification output
- `data/product-based-content.json`: initial product-technology classification output
- `data/employee-based-content-clean.json`: cleaned employer-branding dataset after overlap removal
- `data/product-based-content-clean.json`: cleaned product-technology dataset after overlap removal
- `data/youtube_tag_frequencies.json`: frequent tag export used to make the classification step reproducible

## Workflow

### 1. Classification

The first notebook performs the database-heavy part of the project. It:

- connects to MongoDB with `pymongo`
- reads from the `youtube.videos` collection
- aggregates and exports frequent YouTube tags
- classifies videos into employer-branding and product-technology groups
- writes both datasets to the `data/` folder

This step produced the initial classified sets shown in the notebooks:

- employer-branding: 3,120 videos
- product-technology: 4,605 videos

### 2. Validation And Overlap Removal

The second notebook checks whether the two rule-based categories overlap. Videos that appear in both groups are treated as ambiguous and removed from both sides before the final comparison.

After cleaning:

- employer-branding: 2,164 videos
- product-technology: 3,649 videos
- removed as overlapping: 956 videos from each category

### 3. Audience Conversation Analysis

The final notebook compares the two content types using:

- raw comments
- likes
- views
- comments per 1,000 views
- likes per 1,000 views
- share of videos with at least one comment

It also includes:

- Mann-Whitney tests for distributional differences
- control models with company and publication year
- a company-year robustness check using Wilcoxon tests

## Main Finding

Based on the final notebook output currently stored in the repository, the dataset does not support the hypothesis that employer-branding videos generate more audience conversation.

The main conclusion reported in the analysis notebook is:

- employer-branding videos have lower mean raw comments than product-technology videos
- employer-branding videos also have slightly lower mean comments per 1,000 views
- the direction of the observed differences does not support the employer-branding hypothesis
- that pattern does not reverse after controls for company and publication year

## Requirements

The notebooks use Python 3 and standard data-analysis libraries. The imports visible in the notebooks include:

- `pandas`
- `numpy`
- `matplotlib`
- `scipy`
- `pymongo`
- `certifi`

You will also need Jupyter Notebook or JupyterLab to run the `.ipynb` files.

## Running The Project

### Option 1: Review The Existing Analysis

If you only want to inspect the project:

1. Open the notebooks in numeric order.
2. Start with the cleaned datasets in `data/`.
3. Use the final notebook to review the statistical analysis and conclusion.

### Option 2: Reproduce The Full Pipeline

If you want to rerun the full workflow from MongoDB:

1. Install the Python dependencies listed above.
2. Start Jupyter Notebook or JupyterLab in the repository root.
3. Set `MONGODB_URI` in your environment, or enter it when prompted by the first notebook.
4. Run the notebooks in order from `01` to `03`.

## Data Source

The project is based on video metadata and audience engagement information collected from Swiss corporate YouTube channels and stored in MongoDB. The first notebook accesses the `youtube` database and works primarily with the `videos` collection.

## Notes

- The classification logic is rule-based and built around tags, titles, and related metadata extracted from the MongoDB source data.
- The repository may contain tracked exports that differ from the current local working tree, depending on whether the notebooks were rerun locally.
- The JSON exports in `data/` are line-delimited records and can be loaded directly with `pandas.read_json(..., lines=True)`.
