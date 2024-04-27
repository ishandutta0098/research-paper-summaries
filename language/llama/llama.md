# LLAMA

## Introduction
- Best performances are not achieved by largest models but by smaller models trained on more data.
- Key thing to note is inference speed matters a lot. We prefer a faster inference model, so a smaller model trained longer will be cheaper at inference.

## PreTraining Data
- English Commoncrawl (67%)
  - Perform Language identification with a fastText linear classifier to remove no English pages
  - Filter low quality content with n gram language model
  - Trained a linear model to classify pages used as references in Wikipedia vs randomly sampled pages and discarded pages not classified as references.

- C4 (15%)
- GitHub (4.5%)
  - Filtered low quality files with heuristics based on the line length and removed boilerplate such as headers, with regular expressions.
 
- Wikipedia (4.5%)
  - 20 Languages
  - Remove hyperlinks, comments, etc
    
- Gutenberg and Books3 (4.5%)
  
- ArXiv (2.5%)
  - Remove first section, bibliography, inline expanded definitions, macros
    
- Stack Exchange (2%)
  - 28 Largest Websites
  - Removed HTML Tags
  - Sorted by score
 
### Tokenizer - 
- Bytepair encoding (BPE) algorithm (Sennrich et al.,2015), using the implementation from SentencePiece (Kudo and Richardson, 2018).
- Split all numbers into individual digits, and fallback to bytes to decompose unknown UTF-8 characters.
- Training dataset contains roughly 1.4T tokens after tokenization.
