# GML_NN — a neural-network library in C

[![Language](https://img.shields.io/badge/language-C-blue.svg)](https://en.wikipedia.org/wiki/C_(programming_language))
[![Platform](https://img.shields.io/badge/platform-GNU%2FLinux-green.svg)](https://www.gnu.org/)
[![License: MIT](https://img.shields.io/badge/license-MIT-lightgrey.svg)](LICENSE)

**GML_NN** is a library written from scratch in **standard C**, with no dependencies beyond the
maths library, to create, train and use **multilayer perceptrons (MLP)**. It is my Bachelor's
thesis in Computer Engineering at the School of Computer Engineering (ETSIINF), Universidad
Politécnica de Madrid — graded **9.6/10**.

- 📄 **Thesis** (Spanish): theory from the perceptron to backpropagation, design decisions and
  experiments — [UPM Digital Archive](https://oa.upm.es/82476/).
- 📘 **User manuals** (Spanish) for `gml_nn.h`, `matrix.h` and `data_handler.h` in [`manual/`](manual/).

## Features

- **Arbitrary architectures:** any number of layers and neurons per layer.
- **Supervised training** with backpropagation (generalised delta rule).
- **Activation functions:** sigmoid, tanh, LeCun's optimised sigmoid, ReLU, Leaky ReLU, softplus
  and Heaviside step.
- **Error functions:** mean squared error (MSE) and MSE/2.
- **Optimisers:** stochastic, batch and mini-batch gradient descent, optionally with momentum.
- **Custom functions:** plug in your own activation or error function; derivatives are computed
  by finite differences.
- **Persistence:** save a trained network to a `.nn` file and load it back.
- **Helper modules:** `matrix.h` (matrix operations) and `data_handler.h` (CSV loading,
  train/test split, Fisher–Yates shuffling, min-max normalisation, one-hot encoding).

## Quick start

Requirements: `gcc` (or any C99 compiler) and `make`, on GNU/Linux.

```bash
git clone https://github.com/alvarogmendez/gml_nn.git
cd gml_nn/test_cases
make xor           # builds the example against the library in src/
./xor              # learns the XOR function (accuracy 1.0)
```

`make` builds every example; `make clean` removes the binaries. To use the library from your own
program, build it once with `make -C src` and link against it:

```bash
gcc -Isrc my_program.c -Lsrc -lgml_nn -lm -o my_program
```

A minimal network, taken from [`test_cases/xor.c`](test_cases/xor.c):

```c
int layers[] = {2, 1};                                   // 2 hidden neurons, 1 output
neural_net nn = nn_create(ACT_OPSIGMOID, 2, layers, 2);  // 2 inputs
nn_set_learning_rate(&nn, 0.3);
nn_weight_randf(&nn);

parser_result data = parse_data("../datasets/xor.csv", 2);
nn_set_training_data(nn, 4, data.data_input, data.data_output);
train_network(nn, 80, 1, COST_TRAIN);                    // 80 epochs, print the cost every epoch
```

## Repository layout

| Folder | Contents |
|---|---|
| `src/` | The library: `gml_nn`, `matrix` and `data_handler` (`.c` / `.h`) |
| `test_cases/` | Examples: logic gates, XOR, 2-D spirals, 6-class problem, diabetes, HIV, MNIST |
| `datasets/` | Data for the examples (only `xor.csv` is included — see below) |
| `code_examples/` | Snippets used in the user manuals |
| `experimental/` | Early prototypes (perceptron, ADALINE, first backpropagation versions) |
| `manual/` | User manuals (PDF, Spanish) |

### Datasets

The examples read their data from `../datasets/*.csv`. Only `xor.csv` ships with the repository;
the real-world datasets used in the thesis (diabetes, HIV/AIDS, MNIST) must be downloaded
separately — the thesis describes each of them.

## Architecture

The library revolves around two `struct`s:

1. **`layer`** stores the layer's weights `W` (weights and biases in a single matrix), the
   auxiliary matrices used during training (`dW`, `vw`, …) and its output `out`.
2. **`neural_net`** holds the global configuration — learning rate, momentum decay, random seed,
   batch size and error function — and the array of layers.

Training propagates the error gradient from the output layer back to the input using function
pointers for the derivatives, so new activation or error functions can be added without touching
the backpropagation algorithm.

## Results and limitations

The library solves linear and non-linear classification problems (logic gates, XOR, spirals) and
reaches accuracy comparable to other tools on the diabetes and HIV datasets. On **MNIST** it stays
around **58 %** in the best case; the thesis attributes this to:

- using a fully connected MLP where convolutional networks are the right tool for images,
- min-max normalisation, which is not ideal for pixel data,
- the lack of advanced optimisers (Adam, RMSProp) and of a cross-entropy loss for multi-class
  classification.

## Future work

- Refactor the function-pointer design for speed.
- Cross-entropy and more activation/error functions.
- Adam and RMSProp optimisers.
- Sparse (not fully connected) layers, convolution and pooling.
- Better visualisation tools.

## Author

Álvaro González Méndez — [alvarogmendez.es](https://alvarogmendez.es) · [LinkedIn](https://www.linkedin.com/in/alvarogmendez/)

Released under the [MIT License](LICENSE).
