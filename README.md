# Markov Chain Text Generator

Welcome to the Markov Chain Text Generator! This Python project uses a Markov Chain model to generate text based on the structure and patterns found in a given input text. It’s perfect for creating unique and coherent sequences that mimic the style of your source material.

## Features

- **Text Generation**: Create new text sequences based on a given input using a Markov Chain model.
- **Flexible Context Size**: Customize the length of the context (n-grams) for more varied text outputs.
- **Probability-Based Sampling**: Generate text with probabilities derived from the input text to ensure realistic outputs.

## Installation

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Vamsi404/Markov-chain-based-text-generator
   ```
2. **Navigate to the Directory**
   ```bash
   cd markov-chain-text-generator
   ```
3. **Install Required Libraries**
   Make sure you have `numpy` installed. You can install it using pip:
   ```bash
   pip install numpy
   ```

## Usage

### 1. Generate the Frequency Table

This function creates a table of character frequencies based on the input text and context length (`k`).

```python
def generateTable(data, k=4):
    T = {}
    for i in range(len(data) - k):
        X = data[i:i + k]
        Y = data[i + k]

        if T.get(X) is None:
            T[X] = {}
            T[X][Y] = 1
        else:
            if T[X].get(Y) is None:
                T[X][Y] = 1
            else:
                T[X][Y] += 1

    return T
```

### 2. Convert Frequencies to Probabilities

Convert the frequency counts in the table to probabilities.

```python
def convertfreqintoprob(T):
    for kx in T.keys():
        s = float(sum(T[kx].values()))
        for k in T[kx].keys():
            T[kx][k] = T[kx][k] / s

    return T
```

### 3. Load Text from a File

Load and preprocess text data from a file.

```python
def load_text(filename):
    with open(filename, encoding='utf8') as f:
        return f.read().lower()
```

### 4. Train the Markov Chain Model

Train the Markov Chain model using the input text.

```python
def trainMarkovChain(text, k=4):
    T = generateTable(text, k)
    T = convertfreqintoprob(T)
    return T
```

### 5. Sample the Next Character

Sample the next character based on the current context and the trained model.

```python
def sample_next(ctx, T, k):
    ctx = ctx[-k:]
    if T.get(ctx) is None:
        return " "
    possible_Chars = list(T[ctx].keys())
    possible_values = list(T[ctx].values())

    return np.random.choice(possible_Chars, p=possible_values)
```

### 6. Generate Text

Generate a new text sequence starting from a given sentence.

```python
def generateText(starting_sent, k=4, max_len=1000):
    sentence = starting_sent
    ctx = starting_sent[-k:]

    for ix in range(max_len):
        next_prediction = sample_next(ctx, model, k)
        sentence += next_prediction
        ctx = sentence[-k:]

    return sentence
```

## Example

Here's a quick example of how to use the code:

```python
text_path = "path/to/your/text_file.txt"
text = load_text(text_path)
model = trainMarkovChain(text, k=4)
generated_text = generateText("starting sentence", k=4, max_len=200)
print(generated_text)
```

## Contributing

Contributions are welcome! If you have suggestions or improvements, feel free to open an issue or submit a pull request.

## Contact

For any questions or inquiries, please contact [your email](mandavamsi302001@gmail.com).

---

Happy text generating! 🎉
