# Neural Network Modeling Workflow

## Step 1 — Build a Baseline Model

I first created a baseline neural network to establish a reference point for later experiments. The baseline model used a small funnel architecture (12 → 6 → 1) with ReLU activation functions and a sigmoid output layer for binary classification.

The model was trained using:

* Adam optimizer
* binary cross-entropy loss
* batch size of 32
* 20 epochs
* validation split of 0.2

The purpose of the baseline model was to evaluate the initial learning behavior and determine whether the model showed signs of underfitting or overfitting before making more advanced modifications.

The baseline model showed stable learning behavior with steadily decreasing loss and gradually increasing accuracy. However, the validation performance plateaued relatively early, suggesting mild overfitting while still maintaining relatively strong baseline performance.

---

## Step 2 — Round 1: Add Dropout Regularization

Since the baseline model showed slight overfitting, the first experiment focused on improving generalization through regularization rather than immediately increasing complexity.

I added:

* `Dropout(0.2)`

while keeping the same baseline architecture.

The goal of this round was to determine whether dropout could reduce overfitting by preventing the network from relying too heavily on individual neurons during training. The validation curves became slightly smoother and more stable, although the improvement in overall model performance was relatively small.

---

## Step 3 — Round 2: Increase Model Complexity

After testing regularization, I explored whether increasing model complexity would improve performance.

I increased:

* the number of hidden layers,
* and the number of neurons.

The architecture became:

* `24 → 12 → 6 → 1`

This round tested whether the baseline model was underfitting due to insufficient model capacity. The deeper funnel architecture increased model capacity and improved training performance slightly, but validation performance remained relatively stable and showed only modest improvement. The results suggested diminishing returns from simply increasing model depth.

---

## Step 4 — Round 3: Add L2 Regularization

Because the deeper architecture introduced a greater risk of overfitting, I next tested L2 regularization.

I added:

* `kernel_regularizer=keras.regularizers.l2(0.001)`

to the hidden layers while keeping the Round 2 architecture.

The purpose of this round was to penalize excessively large weights and encourage simpler model representations. Training and validation curves remained more stable and closely aligned throughout training. However, the overall performance gains remained relatively modest.

---

## Step 5 — Round 4: Dropout + EarlyStopping

In this round, I combined dropout regularization with EarlyStopping while keeping the deeper funnel architecture.

The purpose was to determine whether additional regularization and automatic stopping could further improve validation behavior and reduce overfitting. The validation curves became slightly more stable, although the overall performance remained very similar to previous rounds.

At this stage, manual experimentation was producing only relatively small differences between models.

---

## Step 6 — Round 5: Optuna Hyperparameter Tuning

After completing the manual experiments, I used Optuna to automate hyperparameter tuning.

Optuna tuned:

* number of hidden layers,
* neurons per layer,
* learning rate,
* dropout rates,
* and batch size.

This step was necessary because manually testing all possible hyperparameter combinations becomes inefficient as the number of parameters increases. Optuna provided a more systematic and efficient method for identifying stronger model configurations.

For Optuna tuning, I used:

* `epochs=50`
* EarlyStopping

This allowed the model to train longer while automatically stopping training when validation performance stopped improving.

The final Optuna-tuned model produced the strongest overall ROC AUC score, although the improvement over earlier experiments remained relatively modest. This suggested that the baseline model was already fairly effective for the Adult Census dataset, and that later experiments mainly improved training stability and generalization behavior rather than substantially increasing predictive performance.
