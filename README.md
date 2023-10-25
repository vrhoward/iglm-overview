# Generative language modeling for antibody design

This repository serves as a descriptive and functional overview of the Immunoglobulin Language Model (IgLM), presented in the following:

Shuai, Richard W., Jeffrey A. Ruffolo, and Jeffrey J. Gray. “Generative Language Modeling for Antibody Design.” bioRxiv, December 20, 2022. https://doi.org/10.1101/2021.12.13.472419.

## Overview

(Five-minute overview providing context, stating the problem the paper is addressing, characterizing the approach, and giving a brief account of how the problem was addressed.)

### Antibody Structure and Function

Antibodies, also known as immunoglobulins, are important protein components of the immune system with the primary function to recognize and neutralize foreign substances.

<img width="594" alt="Screen Shot 2023-10-24 at 11 17 13 PM" src="https://github.com/vrhoward/iglm-overview/assets/107573643/1e6bfaec-7ae4-4b15-ae9b-52098cadee9d">

Owing to their extensive diversity and high specificity in antigen binding, antibodies have also proven to be a powerful therapeutic tool.

In turn, monoclonal antibodies (mAbs) have become a key target for therapeutic discovery and optimization, but these efforts are reliant 

The identification and fine-tuning of monoclonal antibodies for therapeutic use depend on extensive sequence libraries; however, challenges related to developability—such as poor solubility, reduced thermal stability, elevated aggregation, and high immunogenicity—pose significant obstacles.

### Problem

### Approach

## Architecture Overview

The IgLM model uses a standard left-to-right decoder-only transformer architecture (GPT-2), trained on 558 million natural antibody sequences. 

by autoregressive language modeling of reordered antibody sequence segments that are conditioned on chain and species identifier tags.

<img width="929" alt="Screen Shot 2023-10-24 at 11 26 01 PM" src="https://github.com/vrhoward/iglm-overview/assets/107573643/1b5b1488-d1d8-4566-b733-c96b349e32ac">




m is the mask length
j is the mask starting position
note that our vocabulary of tokens now includes tokens for MASK,SEP,ANS, and conditioning tags

infilling method is better because it incurs almost no computational overhead compared to language modeling, sequence lengths remain similar to those encountered for the same x during language modeling. In contrast, using LMs to directly predict x from x ̃ as in Fedus et al. (2018) effectively doubles the sequence length of x. This is particularly problematic when considering models like GPT-2 whose memory usage grows quadratically with sequence length. Second, our framework requires minimal change (three addi- tional tokens) to an existing LM’s vocabulary. Fi- nally, because the entirety of x ̃ is in the “past” when predicting y, the ILM framework combines the abil- ity to attend to incorporate context on both sides of a blank with the simplicity of decoding from LMs.

(Prepare a formal pseudocode description of the proposed model, indicate how it differs from previous models)

## Critical Analysis

(Answer one or more of the following questions: What was overlooked by the authors? What could have been developed further? Were there any errors? Have others disputed the findings?)

## Code Demonstration



## Additional Resource Links

**1. Antibody Structure and Function:** Chiu ML, Goulet DR, Teplyakov A, Gilliland GL. Antibody Structure and Function: The Basis for Engineering Therapeutics. Antibodies (Basel). 2019 Dec 3;8(4):55. doi: 10.3390/antib8040055. PMID: 31816964; PMCID: PMC6963682.

**2. Infilling by Language Modeling (ILM):** Donahue, Chris, Mina Lee, and Percy Liang. “Enabling Language Models to Fill in the Blanks.” arXiv, September 10, 2020. http://arxiv.org/abs/2005.05339.

**3. Relevant Notation for Pseudocode:** Phuong, Mary, and Marcus Hutter. “Formal Algorithms for Transformers.” arXiv, July 19, 2022. https://doi.org/10.48550/arXiv.2207.09238.

**4. Official Repository for IgLM:** https://github.com/Graylab/IgLM

**5. IgLM Google Colab:** https://colab.research.google.com/github/Graylab/IgLM/blob/main/IgLM.ipynb

## Video Recording


