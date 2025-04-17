### EX3 Implementation of GSP Algorithm In Python
### DATE:17/04/2024 
### AIM: To implement GSP Algorithm In Python.
### Description:
The Generalized Sequential Pattern (GSP) algorithm is a data mining technique used for discovering frequent patterns within a sequence database. It operates by identifying sequences that frequently occur together. GSP works by employing a depth-first search strategy to explore and extract frequent patterns efficiently.
### Steps:
1. <strong>Database Scanning:</strong> GSP scans the sequence database to determine the support of each item in the dataset.
2. <strong>Candidate Generation:</strong> It generates a set of candidate sequences using frequent items found in the previous step.
3. <strong>Pattern Growth:</strong> It extends the candidate sequences by merging them to form longer patterns, checking their support against a user-defined minimum support threshold.
4. <strong>Repeat:</strong> The process continues until no new sequences meet the minimum support threshold.
<p align="justify">
GSP finds application in various domains such as market basket analysis, web usage mining, bioinformatics, and more. For instance, in retail, GSP can identify common purchasing patterns, helping businesses understand customer behavior for targeted marketing or inventory management.
</p>

### Procedure:
<p align="justify">
1. From collections import defaultdict, from itertools import combinations: Imports necessary libraries/modules. defaultdict is
used to create a dictionary with default values and combinations generates all possible combinations of a sequence.</p>
<p align="justify">
2. generate_candidates(dataset, k): Function to generate candidate k-item sequences from a dataset. It loops through each sequence in the
dataset and finds combinations of length k for each sequence, updating their counts in a dictionary.</p>
<p align="justify">
3. gsp(dataset, min_support): Function that implements the Generalized Sequential Pattern (GSP) algorithm. It iterates through increasing
sequence lengths (k) until no new frequent patterns are found. It calls generate_candidates() to find patterns of varying lengths.</p>
<p align="justify">
4. Example dataset for each category: Defines example sequences for top wear, bottom wear, and party wear categories.</p>
<p align="justify">
5. Minimum support threshold: Sets the minimum support count required for a pattern to be considered frequent.</p>
<p align="justify">
6. Perform GSP algorithm for each category: Applies the GSP algorithm for each category using the defined example datasets and the
minimum support threshold.</p>
<p align="justify">
7. Output the frequent sequential patterns for each category: Prints the frequent sequential patterns 
    along with their support counts
for each wear category.</p>
<p align="justify">
8. Visulaize the sequence patterns using matplotlib.
</p>

### Program:
```python
from collections import defaultdict
from itertools import combinations

# Function to generate candidate k-item sequences
def generate_candidates(frequent_patterns, k):
    candidates = set()
    patterns = list(frequent_patterns.keys())
    for i in range(len(patterns)):
        for j in range(i+1, len(patterns)):
            pattern1 = patterns[i]
            pattern2 = patterns[j]
            if pattern1[:k-2] == pattern2[:k-2]:
                candidate = pattern1 + (pattern2[-1],)
                candidates.add(candidate)
    return candidates

# Function to check if pattern is a subsequence of sequence
def is_subsequence(pattern, sequence):
    seq_idx = 0
    for item in pattern:
        while seq_idx < len(sequence) and sequence[seq_idx] != item:
            seq_idx += 1
        if seq_idx == len(sequence):
            return False
        seq_idx += 1
    return True

# Function to perform GSP algorithm
def gsp(dataset, min_support):
    frequent_patterns = defaultdict(int)
    item_counts = defaultdict(int)
    for sequence in dataset:
        unique_items = set(sequence)
        for item in unique_items:
            item_counts[(item,)] += 1

    current_frequent = {item: count for item, count in item_counts.items() if count >= min_support}
    all_frequent = dict(current_frequent)

    k = 2
    while current_frequent:
        candidates = generate_candidates(current_frequent, k)
        candidate_counts = defaultdict(int)
        for sequence in dataset:
            for candidate in candidates:
                if is_subsequence(candidate, sequence):
                    candidate_counts[candidate] += 1
        current_frequent = {candidate: count for candidate, count in candidate_counts.items() if count >= min_support}
        all_frequent.update(current_frequent)
        k += 1

    return all_frequent

# Function to print output as a table
def print_table(title, results):
    print(f"\n{title}")
    print("-" * 50)
    print("{:<40} {:<10}".format("Pattern", "Support"))
    print("-" * 50)
    if results:
        for pattern, support in results.items():
            pattern_str = " -> ".join(pattern)
            print("{:<40} {:<10}".format(pattern_str, support))
    else:
        print("{:<40} {:<10}".format("No frequent patterns", "-"))
    print("-" * 50)

# Example dataset for each category
top_wear_data = [
    ["blouse", "t-shirt", "tank_top"],
    ["hoodie", "sweater", "top"],
    ["hoodie"],
    ["hoodie", "sweater"]
]
bottom_wear_data = [
    ["jeans", "trousers", "shorts"],
    ["leggings", "skirt", "chinos"],
]
party_wear_data = [
    ["cocktail_dress", "evening_gown", "blazer"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress"],
    ["party_dress"],
]

# Minimum support threshold
min_support = 2

# Perform GSP algorithm for each category
top_wear_result = gsp(top_wear_data, min_support)
bottom_wear_result = gsp(bottom_wear_data, min_support)
party_wear_result = gsp(party_wear_data, min_support)

# Output the frequent sequential patterns for each category as table
print_table("Frequent Sequential Patterns - Top Wear", top_wear_result)
print_table("Frequent Sequential Patterns - Bottom Wear", bottom_wear_result)
print_table("Frequent Sequential Patterns - Party Wear", party_wear_result)

```
### Output:
![image](https://github.com/user-attachments/assets/b9c0256d-dc9b-4d52-be4e-0cabb1eedc3c)

### Visualization:
```python
import matplotlib.pyplot as plt
# Function to visualize frequent sequential patterns with a line plot
def visualize_patterns_line(result, category):
    if result:
        patterns = list(result.keys())
        support = list(result.values())

        plt.figure(figsize=(10, 6))
        plt.plot([str(pattern) for pattern in patterns], support, marker='o', linestyle='-', color='blue')
        plt.xlabel('Patterns')
        plt.ylabel('Support Count')
        plt.title(f'Frequent Sequential Patterns - {category}')
        plt.xticks(rotation=90)
        plt.tight_layout()
        plt.show()
    else:
        print(f"No frequent sequential patterns found in {category}.")

# Visualize frequent sequential patterns for each category using a line plot
visualize_patterns_line(top_wear_result, 'Top Wear')
visualize_patterns_line(bottom_wear_result, 'Bottom Wear')
visualize_patterns_line(party_wear_result, 'Party Wear')
```
### Output:
![WDM 2](https://github.com/user-attachments/assets/0330be8c-9217-412e-be6b-4b4895d3f6c4)

### Result:
Thus the implementation of the GSP algorithm in python has been successfully executed.
