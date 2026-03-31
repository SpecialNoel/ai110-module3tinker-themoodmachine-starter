# Model Card: Mood Machine

This model card is for the Mood Machine project, which includes **two** versions of a mood classifier:

1. A **rule based model** implemented in `mood_analyzer.py`
2. A **machine learning model** implemented in `ml_experiments.py` using scikit learn

You may complete this model card for whichever version you used, or compare both if you explored them.

## 1. Model Overview

**Model type:**

- I used the rule based model only

**Intended purpose:**

- The model is trying to classify short text messages as moods like positive, negative, neutral, or mixed.

**How it works (brief):**

- The model gives scores based on particular words of the text messages. If the word appears in the predefined
  positive word list, a score of 1 will be given; otherwise, a score of -1 will be given if it appears in the
  predefined negative word list. If the sentence contains negation words, the model will give correct score
  for at most 1 word immediately after the negation word. If the sentence contains words which show that it
  is in fact a sarcasm sentence, the model will flip the original score it got.

## 2. Data

**Dataset description:**

- I added 5 more posts in `SAMPLE_POSTS`. For each added new post, I add the corresponding attribute (e.g. positive,
  negative, etc.) to the `TRUE_LABELS` immediately.

**Labeling process:**

- I chose labels for these new sample posts by my personal judgement.
- The sample post 'She drove me crazy.' is hard to label as it could have very different meanings, depending on the circumstances.

**Important characteristics of your dataset:**

- The dataset contains some ambiguous messages that are hard to label without context.

**Possible issues with the dataset:**

- The dataset does not contain any sentences with emojis and emojis constructed by special characters.

## 3. How the Rule Based Model Works

**Your scoring rules:**

- The model handles the negation words in the sentence by check the token immediately after the
  negation word. If there's such a pair in the sentence, the sentence will get a flipped score for
  these two words.

**Strengths of this approach:**

- The model behaves reasonably well when the sentence has only one major interpretation,
  contains no or only short negation pairs, is no sarcasm, and contains positive words and negative
  words listed in predefined lists. For example, the model can label the sentence:
  "I'm not happy" correctly as negative.

**Weaknesses of this approach:**

- The model fails to behave as expected when there are multiple negation words in a row, or multiple
  words after a single negation words, which could completely alternate the meaning of the whole sentence
  multiple times. For example, the model cannot correctly label the sentence:
  "I'm not not happy", since it predicts as negative, but its true label should be positive.

## 4. Evaluation

**How you evaluated the model:**

- The Rule Based Model's accuracy on `SAMPLE_POSTS` is 0.64.

**Examples of correct predictions:**

- "i love this class so much": prediction is correct (positive) since "love" is a positive word,
  and there are no negative words in the sentence.
- "today was a terrible day": prediction is correct (negative) since "terrible" is a negative word,
  and there are no positive words in the sentence.

**Examples of incorrect predictions:**

- "lowkey stressed but kind of proud of myself": prediction is incorrect (negative) since stressed was
  defined on the dataset while proud was not, meaning that it will give -1 to stressed for this sentence.
  The true label should be mixed.
- "she drove me crazy": prediction is incorrect (neutral) since there are no positive words
  nor negative words in the sentence that was defined on the dataset. The true label should be
  negative.

## 5. Limitations

- Limitations about this model:
  -- The dataset contains very limited cases and labeled vocabularies.
  -- The model does not generalize well to longer sentences.
  -- It cannot predict labels correctly if the sentence can have lots
  of different meanings.

## 6. Ethical Considerations

- Potential impacts of using this model in real applications:
  -- Misinterpreting moods for sentences when the sentences have different
  meanings for different cultures.
  -- Limited labeled vocabularies could misclassify messages

## 7. Ideas for Improvement

- Ways to improve the Rule Based Model:
  -- Add more labeled data in lists in dataset.py, such as `POSITIVE_WORDS`, `NEGATIVE_WORDS`, etc.
  -- Add better preprocessing for emojis or slang
  -- Improve the rule based scoring method
