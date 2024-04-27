# LLAMA

- [LLaMA: Open and Efficient Foundation Language Models](https://arxiv.org/abs/2302.13971)
  
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

## Architecture
Picked up on improvements over the transformer architecture.

1. Pre-normalization [GPT3]
- To improve the training stability, we normalize the input of each transformer sub-layer, instead of normalizing the output.
- We use the RMSNorm normalizing function, introduced by Zhang and Sennrich (2019).
2. SwiGLU activation function [PaLM]
- We replace the ReLU non-linearity by the SwiGLU activation function, introduced by Shazeer (2020) to improve the performance.
3. Rotary Embeddings [GPTNeo]
- We remove the absolute positional embeddings, and instead, add rotary positional embeddings (RoPE), introduced by Su et al. (2021), at each layer of the network.

## Optimizer
- AdamW
- Cosine Learning Rate Schedule

## Efficient Implementation
1. We use an efficient implementation of the causal multi-head attention to reduce memory usage and runtime. This is achieved
by not storing the attention weights and not computing the key/query scores that are masked due to
the causal nature of the language modeling task.
2. We reduced the amount of activations that are recomputed during the backward pass with checkpointing. More precisely, we save the activations that
are expensive to compute, such as the outputs of linear layers.
3. We also overlap the computation of activations and the communication between GPUs over the network (due toall_reduce operations) as much as possible.
