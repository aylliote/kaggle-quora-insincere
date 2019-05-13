# kaggle-quora-insincere

Late submission for the Kaggle Quora Insincere Competition.
It scores 0.70704 F1 score on private leaderboad (rank 27/1401).

This is an assembling of several methods found in the discussion space of the competition, with a few personal ideas.

Texts are pre-processed with many regexs, and some corrections are computed (with the Peter Norvig spellchecker algorithm) on words which have no embedding found in GLOVE or Paragram.

When filling the embedding matrix, lower, upper, capitalized and corrected versions of words are tested, in order to gather as many embeddings as possible. This is done for GLOVE and Paragram, and matrices are then concatenated (leading to a huge \[MAX_WORDS,2\*300] embedding matrix.

Stacked LSTMS followed with FC layer are then trained on 8 Stratied Folds with batchsize 512 and adam optimizer with default parameters. The hold-out fold at each step is used to compute the optimal threshold. As suggested in the winning solution, threshold are computed on ranks rather than direct probabilities, and we chose then the threshold which has the least average deviation from the optimal threshold. Both of these clever strategies allow to find a much more robust threshold at test time (thresholds showed high variance across folds otherwise)
