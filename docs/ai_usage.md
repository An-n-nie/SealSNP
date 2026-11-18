# AI Use and Reproducibility Documentation

## Project
**Predicting Gray Seal Geographic Populations Using SNP Genotypes**

**Research question:** How well can machine learning models classify Northwest Atlantic gray seals (*Halichoerus grypus atlantica*) into geographic populations using SNP genotypes?

## How ChatGPT Was Used

ChatGPT was used as an AI-assisted learning and coding tool throughout this project. Its main contributions included:

- Helping develop and troubleshoot Python code for data parsing, preprocessing, exploratory analysis, and classification.
- Suggesting quality-control checks and ways to validate genotype parsing and metadata.

AI-generated code and explanations were treated as suggestions rather than automatically accepted results. Code was run in Google Colab, and outputs were examined and checked against the dataset and expected behavior.

## ChatGPT: Prompts, AI Outputs, and Verification

The following are representative prompts and summaries of AI assistance used during the project. They are organized by work stage.

| Stage | Representative prompt | AI output / assistance | How the output was verified |
|---|---|---|---|
| Dataset Selection | "What datasets can we use for an ML-assisted bioinformatics project? (preferably supervised learning and animal related) | Helped searched for potential datasets and weighed the pros and cons of each selection while refining suggestions | Examined each respective dataset source and cross-checked suggestion with another AI|
| File selection | "Which of the downloaded files should we use for our seal project, and what is each file for?" | Explained the roles of the Genepop genotype file, SNP catalog, and consensus FASTA file. Helped distinguish gray seal data from harbor seal data. | Inspected the downloaded files, their contents, and the associated Dryad dataset and research paper. Only gray seal genotypes were used for the classification analysis. |
| Genotype parsing | "How do we parse the Genepop file into a genotype matrix, and how can we check that the genotype codes are correct?" | Helped write code to extract individual IDs and genotype tokens, split the four-digit codes into two alleles, and convert biallelic genotypes into dosage values. | Checked matrix dimensions, missing-value codes, token lengths, allele mappings, catalog matches, and manually inspected example genotype tokens against their expected dosage values. |
| Metadata and cohorts | "How can we identify the geographic population and sampling cohort for each individual?" | Helped organize individual IDs and construct population and cohort labels for the analysis. | Checked the ID prefixes against the sample information, manually established the prefix-to-population/cohort mapping, and verified population counts and cohort distributions using cross-tabulations. |
| Missingness and quality control | "How should we examine missing genotype values, and should individuals with high missingness be removed?" | Suggested examining missingness by individual and SNP marker before deciding whether filtering was necessary. | Calculated missingness directly from the genotype matrix and inspected distributions, thresholds, and population/cohort patterns. No individuals were automatically removed based solely on missingness. |
| Exploratory PCA | "Can we use PCA to visualize whether the three gray seal populations separate based on SNP genotypes?" | Helped construct an imputation, standardization, and PCA workflow and interpret the explained variance. | Independently inspected the explained variance ratios and PCA visualization. PC1 and PC2 explained approximately 2% of variance combined. |
| Classification models | "How should we compare a majority baseline, Logistic Regression, and Random Forest for this classification problem while keeping codes clean and reproducible?" | Helped construct scikit-learn pipelines and compare models using accuracy, balanced accuracy, and macro F1. | Ran the models in Google Colab, examined cross-validation results, and compared predictions against the majority-class baseline. |
| PCA for classification | "Can we include PCA inside the machine learning pipeline so that PCA does not introduce data leakage?" | Suggested placing imputation, scaling, and PCA inside the pipeline before the classifier. | Checked that preprocessing was fitted within each cross-validation training fold. Compared results with models trained without PCA. |
| Cohort-aware validation | "How can we test whether the models generalize to sampling cohorts that were not included in training?" | Suggested Leave-One-Group-Out validation using cohort IDs as groups and provided corresponding cohort-aware pipeline | Verified the number and distribution of cohorts, confirmed that entire cohorts were held out, and generated pooled out-of-fold predictions for evaluation. |
| Model tuning | "How should we tune our models in a way that aligns with each respective validation strategy?" | Suggested testing different class weights, tree depths, and minimum leaf sizes. | Compared the selected model's pooled out-of-fold predictions against the original modeling performance and majority baseline.|

*Note: Prompts are representative summaries of the questions asked during the project, rather than a verbatim transcript of every conversation.*

## How Claude Was Used

Claude (Anthropic) was used as an AI-assisted brainstorming, research, explanation, writing, and code-review tool throughout this project. Its main contributions included:

- Brainstorming bioinformatics project topics and comparing their feasibility against the course rubric.
- Searching for and summarizing public datasets including what files each contains and what could not be confirmed.
- Reading my notebook on GitHub to draft a README template, and reviewing the notebook for code correctness and clarity.

AI-generated suggestions and explanations were treated as suggestions rather than accepted results. Claude could not open some data files (the Dryad downloads were blocked to it), so I opened and checked the files myself. The numbers in the notebook and report come from running the code, not from AI.

## Claude: Prompts, AI Outputs, and Verification

The following are representative prompts (paraphrased from the conversation) and summaries of AI assistance, organized by work stage.

| Stage | Representative prompt | AI output / assistance | How the output was verified |
|---|---|---|---|
| Choosing the dataset | "Do you think we can look into animal-related datasets that have a genetic connection that we can use ML on?" | Found the Cammen et al. (2018) Dryad dataset; described the genepop, SNP catalog, and FASTA files; suggested classifying colonies. Noted the colony labels needed to be confirmed. | Downloaded the files, opened the genepop file, and confirmed the Pop blocks. Checked 252 gray seals and 55 harbor seals against the Dryad page. |
| README | "Can you give me a template for my Github's readme (use various styles of bolding words or italics, tables and so on)?" | Read the notebook on GitHub (code and saved outputs, without re-running it) and a README template. | Examined the Readme file and edited the text. |
| Notebook code review | "Check my notebook on GitHub for code correctness and clarity" | Reviewed the notebook and listed findings. | Suggestions are taken into consideration and changes handled manually. |


