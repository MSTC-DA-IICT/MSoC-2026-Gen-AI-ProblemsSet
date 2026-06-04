# Week 2 - Python Programming

Welcome to Week 2 of the Generative AI Track!

This week focuses on strengthening your Python fundamentals through practical coding tasks. Python is the primary programming language used throughout the Generative AI ecosystem, from data preprocessing and model training to inference and deployment.

By completing these exercises, you will build the programming foundation required for future topics such as tokenization, embeddings, transformers, and Large Language Models (LLMs).

---

# Task 1: Find Unique Elements from an Array

**Difficulty:** Easy

## Problem Statement

Write a Python program that:

1. Takes an array (list) as input from the user.
2. Passes the array to a function.
3. The function should return all unique elements present in the array.
4. Display the unique elements to the user.

### Example

Input:

```python
[1, 2, 3, 2, 4, 1, 5]
```

Output:

```python
[1, 2, 3, 4, 5]
```

## Learning Objectives

* Variables
* Lists
* Loops
* Functions
* Basic Python Syntax

These concepts form the foundation for writing AI and Machine Learning applications in Python.

---

# Task 2: Matrix Operations

**Difficulty:** Medium

## Problem Statement

### Part A: Matrix Transpose

Given a matrix, print its transpose.

Example:

Input:

```text
1 2 3
4 5 6
```

Output:

```text
1 4
2 5
3 6
```

### Part B: Diagonal Sums

For a square matrix, calculate:

* Primary Diagonal Sum
* Secondary Diagonal Sum

Example:

Input:

```text
1 2 3
4 5 6
7 8 9
```

Output:

```text
Primary Diagonal Sum = 15
Secondary Diagonal Sum = 15
```

## Learning Objectives

* Working with 2D Lists (Matrices)
* Nested Loops
* Matrix Traversal
* Index-Based Operations

Matrices are fundamental structures in Machine Learning, Deep Learning, and Generative AI systems.

---

# Task 3: String Tokenization

**Difficulty:** Hard

## Problem Statement

Tokenization is one of the most important preprocessing steps used in Large Language Models (LLMs).

Your task is to tokenize a given string into individual words.

Example:

Input:

```python
"I am Tirth Patel"
```

Output:

```python
["I", "am", "Tirth", "Patel"]
```

### Dataset

Use the following code to download the text data:

```python
import requests

url = "https://raw.githubusercontent.com/spyguessgame-boop/own_dataset/refs/heads/main/data.txt"

response = requests.get(url)
response.raise_for_status()

text_data = response.text

text_data = text_data[:1000]
print(text_data)
```

Run the code above to obtain the text and then write a Python program that tokenizes the content into words.

### Bonus Challenge

* Count the total number of tokens.
* Find the most frequent token.
* Remove punctuation before tokenization.

## Learning Objectives

* String Manipulation
* Text Processing
* Lists
* Data Preprocessing
* Introduction to LLM Pipelines

Every modern LLM processes text by first converting it into tokens. Understanding tokenization is the first step toward understanding how language models work internally.

---

# Submission Guidelines

* Write clean and readable code.
* Add comments wherever necessary.
* Push your solutions to your GitHub repository.
* Ensure that your code runs without errors.

Happy Coding!
