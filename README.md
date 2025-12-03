# Intelligent Subset Selection for LLM Benchmarking   [![DOI](https://zenodo.org/badge/1049040333.svg)](https://doi.org/10.5281/zenodo.17800453)   

## 📋 Project Overview

This project explores strategies for reducing the size of benchmarks used in Large Language Model (LLM) evaluation while maintaining a good approximation of the metric obtained on the full question set. The core hypothesis is that by intelligently selecting a representative subset of questions, we can significantly reduce computational costs without compromising result reliability.

## 🎯 Objectives

- Investigate the relationship between subset size and accuracy estimation stability
- Compare random sampling against representative selection using KMeans clustering
- Provide a methodology for efficient LLM benchmarking with reduced computational requirements
- Analyze how different subject domains affect selection strategy effectiveness

## 📊 Dataset & Experimental Setup

The project uses a subset of the MMLU benchmark containing three topics:

- **high_school_macroeconomics**
- **professional_law** 
- **professional_psychology**

Each data point contains:
- Question text
- Answer choices
- Index of correct answer
- Question embedding
- Model output and correctness label

The evaluation focuses on a single LLM assessed using the `correct` metric.

## 🧪 Methodology

### Baseline: Random Sampling
The baseline approach calculates the average correct answers by randomly sampling increasingly larger question groups (k=1 to 300). Sampling was performed with 1000 repetitions for each subset size to ensure statistical significance.

### Intelligent Selection: KMeans Clustering
As an alternative to random selection, KMeans clustering was applied to select representative question subsets based on their vector embeddings. The approach:

1. Groups questions into k clusters using KMeans
2. Selects the question closest to each cluster centroid
3. Creates a subset that broadly covers the dataset content
4. Repeats the process 100 times with different random seeds for robustness
5. Calculates the average accuracy across all repetitions

## 📈 Results & Findings

### Key Observations:
- **Random sampling** converges rapidly to the global mean, becoming very stable with k between 50-100 questions
- **KMeans selection** provides a smoother and more structured approach compared to random sampling
- Different topics show varying behaviors: more noisy curves where variance is higher
- KMeans performs better on topics with more homogeneous content (high_school_macroeconomics)
- For professional_law, convergence was less regular, suggesting embeddings don't always perfectly capture domain structure

### Performance Metrics by Topic:

**professional_psychology**
- Overall mean accuracy: 0.821
- Both methods converge effectively with KMeans showing slightly more stability

**professional_law**
- Overall mean accuracy: 0.624
- KMeans shows some irregular convergence patterns
- Higher variability at lower k values

**high_school_macroeconomics**
- Overall mean accuracy: 0.788
- KMeans demonstrates excellent performance and stability
- Semantic representativeness is more easily achievable in this uniform domain

## 🚀 Usage

The project includes a Jupyter notebook (`LLM_benchmarking.ipynb`) with complete implementation:

## 🔮 Future Work

1. **Model Ranking Evaluation**: Assess how subset selection affects ranking between different LLMs (TREC-style evaluation)
2. **Alternative Selection Methods**: Explore other techniques like diversity-aware sampling or learnable selection methods
3. **Embedding Quality Analysis**: Investigate how different embedding models affect selection effectiveness
4. **Cross-Domain Validation**: Extend the approach to more diverse benchmark domains
5. **Dynamic Subset Sizing**: Develop methods to determine optimal subset size based on desired confidence levels

## 📊 Conclusion

- Random sampling is effective but shows high variability at low k values
- KMeans provides a more stable and semantically informed selection strategy
- Representativeness is more easily achievable in uniform domains
- Benchmark reduction strategies represent a valid compromise between accuracy and computational efficiency
- KMeans proved effective in providing stable selection, but its effectiveness depends on embedding quality and semantic homogeneity of the domain
