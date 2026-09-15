# Prompt Engineering for Clothing Review Analysis

## Task

This project designs, tests, and evaluates prompt engineering strategies across four core natural language processing tasks: data analysis, summarization, classification, and content generation. Each task compares a naive baseline prompt against a structured, constrained prompt to observe differences in output quality, consistency, and alignment with business requirements. Deliverables include exploratory data analysis, four task-specific evaluation notebooks, a consolidated iteration summary, and this submission report.

## Dataset

Women's Clothing E-Commerce Reviews (`data/reviews.csv`, 23,486 rows)[cite: 1]. The dataset includes structured numeric attributes (`Rating`, `Recommended IND`, `Positive Feedback Count`) suitable for aggregate pattern analysis[cite: 1]. Natural language fields (`Review Text`, `Title`) provide rich source material for text summarization, serve as ground-truth targets for sentiment classification against star ratings and recommendation flags, and supply customer perspectives for grounded content generation[cite: 1].

## Project structure

* `data/reviews.csv`: Raw e-commerce customer reviews dataset containing 23,486 records across 10 features[cite: 1].
* `prompts/01_data_exploration.ipynb`: Exploratory data analysis covering catalog distributions, missing text rows, rating skews, and text cleaning observations[cite: 1].
* `prompts/02_data_analysis_prompts.ipynb`: Naive versus structured prompt evaluation for recurring theme extraction across a stratified sample of 20 reviews[cite: 2].
* `prompts/03_summarization_prompts.ipynb`: Testing single-review sentence compression (under 25 words) and multi-review synthesis (3 executive bullets with frequency counts)[cite: 4].
* `prompts/04_classification_prompts.ipynb`: Zero-shot sentiment classification evaluated against ground-truth ratings and recommendation flags[cite: 5].
* `prompts/05_content_generation_prompts.ipynb`: Customer recovery replies for low-rated reviews and marketing blurbs grounded in high-rated dresses.
* `outputs/iteration_summary.md`: Consolidated summary table documenting prompt iterations, weaknesses, improvements, and quantitative metrics across all tasks.

## Approach summary

**01 — Exploratory Data Analysis:** Evaluated dataset shape, data types, completeness, and rating distributions across product categories[cite: 1]. The analysis identified an 82.2% recommendation rate, a strong 5-star skew (~56% of reviews), 845 missing text entries, and literal `\r\n` line breaks requiring cleaning prior to prompt insertion[cite: 1].

**02 — Data Analysis Prompts:** Contrasted an open-ended request with an improved prompt specifying a product analytics persona, theme groupings (fit, quality, style), and dual Markdown tables with review quotes[cite: 2]. While the naive prompt produced unstructured prose and unsolicited data mining advice, the improved prompt strictly enforced tabular boundaries and isolated key customer feedback[cite: 3].

**03 — Summarization Prompts:** Evaluated single-review compression and multi-review category synthesis[cite: 4]. The improved prompts constrained single-review summaries to a single factual sentence under 25 words, and condensed multi-review feedback into exactly three executive bullets featuring explicit sample frequency counts (e.g., "X of 8 reviews") and direct quotes[cite: 4].

**04 — Classification Prompts:** Tested naive classification against a 3-label schema (`Positive`, `Negative`, `Neutral`) formatted as a Markdown table across 10 stratified reviews with hidden labels[cite: 5]. The structured version achieved a 6/10 baseline rating match, 100% precision on 1–2 star negative reviews, and surfaced discrepancies where customers rated items 5 stars despite mentioning severe personal fit issues[cite: 5].

**05 — Content Generation Prompts:** Designed customer service replies to negative feedback (Rating <= 2) and marketing copy for dresses (Rating >= 4). The improved prompts enforced an empathetic customer support persona with concrete resolution steps under 80 words, and restricted marketing blurbs to a 3-sentence structure grounded strictly in documented customer praise.

## Key learnings about prompt engineering

* Assigning explicit domain personas focuses the model's voice and eliminates conversational filler[cite: 2, 4].
* Specifying tabular schemas or bulleted structures prevents rambling prose and makes outputs immediately scannable[cite: 2, 4, 5].
* Enforcing negative constraints (word limits, sentence caps, closed label taxonomies) prevents hallucinations and keeps generated content bounded[cite: 4, 5].
* Grounding prompts in exploratory findings—such as rating skews and rating-vs-recommendation gaps—ensures balanced sampling and uncovers edge cases where review text contradicts numerical scores[cite: 2, 5].

## Limitations

* Testing was performed manually via copy-paste into an LLM chat interface rather than through automated API pipelines, restricting sample sizes to 10–20 reviews per experiment.
* A known copy-paste truncation affected the input in notebook 02, limiting the model's visibility to the tail end of the review sample[cite: 3].
* Outputs represent a single generation run per prompt version rather than repeated trials, meaning metrics are qualitative and illustrative rather than statistically generalizable.

## How to run

1. Clone the repository to your local workspace:
   ```bash
   git clone [https://github.com/himandi02/prompt-engineering-task.git](https://github.com/himandi02/prompt-engineering-task.git)
   cd prompt-engineering-task