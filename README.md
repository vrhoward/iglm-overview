# Generative language modeling for antibody design

This repository serves as a descriptive and functional overview of the Immunoglobulin Language Model (IgLM), presented in the following:

Shuai, Richard W., Jeffrey A. Ruffolo, and Jeffrey J. Gray. “Generative Language Modeling for Antibody Design.” bioRxiv, December 20, 2022. https://doi.org/10.1101/2021.12.13.472419.

## Overview

### Antibody Structure and Function

Antibodies, also known as immunoglobulins, are important protein components of the immune system with the primary function to recognize and neutralize foreign substances, or antigens. 

<img width="594" alt="Screen Shot 2023-10-24 at 11 17 13 PM" src="https://github.com/vrhoward/iglm-overview/assets/107573643/1e6bfaec-7ae4-4b15-ae9b-52098cadee9d">

Figure adapted from [Wang](https://www.rapidnovor.com/identifying-cdrs-antibody-sequencing/).

Antibodies are composed of 
1) two identical peptide chain pairs: heavy chain and light chain
2) variable (V) and constant (C) regions
3) three CDR loops forming the majority of the antigen-binding site contained within each variable region
4) framework sequences outside the CDR loops that stabilize the loop structure

The CDRs composing the antigen-binding sites are highly variable, allowing for extensive diversity and high specificity in antigen binding. For these reasons, antibodies have proven to be a powerful therapeutic tool for treatment and diagnostics.

### Problem

The discovery and optimization of novel antibodies for therapeutic application requires the construction of large sequence libraries, which typically focuses on diversification of the CDR loop sequences in order to facilitate binding to a diverse set of antigens. While these synthetic antibody libraries can be prepared via direct experimental modifications to antibody CDRs, the possible sequence space within the CDRs is too large to sample, making this method intractable. In response to these challenges, a number of artificial intelligence (AI)-based approaches have been developed for protein sequence generation tasks. However, these approaches typically adopt left-to-right decoding strategies that are ill-equipped to re-design specific segments of interest within proteins.

### Approach

This paper presents the Immunoglobulin Language Model (IgLM), an infilling language model for antibody sequences. This model still uses a standard left-to-right decoder-only transformer architecture (GPT-2), but it is further trained to infill chosen sequence segments, such as CDRs. This functionality is achieved through rearrangement of the amino acid sequences in the dataset, wherein arbitrary-length sequence segments (spans) are masked during training and appended to the end of the sequence. In this manner, the model may learn to predict the masked spans conditioned with the surrounding sequence as the previous context.

## Architecture Overview

IgLM was trained on a collection of antibody sequences from the Observed Antibody Space (OAS), which contains natural antibody sequences from six species: human, mouse, rat, rabbit, rhesus, and camel. The training protocol introduces three main components that vary from the [basic pseudocode](https://doi.org/10.48550/arXiv.2207.09238):

1) conditioning tags for chain type (heavy or light) and species-of-origin are prepended to training sequences, allowing for controllable generation of sequences during inference
2) randomly masked spans of ($10-20$) residues are appended to the end of each amino acid sequence
3) new tokens are introduced for training and inference

The vocabulary of tokens, $V^*$, in these algorithms will include each of the following: the $20$ common amino acids, [MASK], [SEP], [ANS], and the conditioning tags.

<img width="929" alt="Screen Shot 2023-10-24 at 11 26 01 PM" src="https://github.com/vrhoward/iglm-overview/assets/107573643/1b5b1488-d1d8-4566-b733-c96b349e32ac">

&nbsp;

where $m$ is the mask length and $j$ is the mask starting position. During training, these values are chosen from uniform random distributions, as denoted in Algorithm 1. During inference, these values are provided by the user.

## Critical Analysis

The following critical analysis tests the reproducibility of the preliminary results presented in the paper, specifically results from Figure 2c and Figure 2d.

**Figure 2c:** Adherence of generated sequences to species conditioning tags. Each plot shows the species classifications of antibody sequences generated with a particular species conditioning tag (indicated above plots). Solid and dashed lines correspond to sequences generated with heavy- and light-chain conditioning, respectively.

<img width="943" alt="Screen Shot 2023-10-25 at 5 35 50 AM" src="https://github.com/vrhoward/iglm-overview/assets/107573643/53b16097-81a9-47b2-b800-4514602fb51b">

&nbsp;

While some plots demonstrate reasonable matches in the results for sequence adherence to species conditioning tags, there are notable discrepancies for certain species. For example, sequences conditioned on the [HUMAN] species tag and the [RAT] species tag generate nearly 100% alpaca sequences, unlike the results seen in the paper.

Further, some of the paper results display error. The [RAT] species tag, for example, is unable to produce rat sequences. 

**Figure 2d:** Adherence of generated sequences to chain conditioning tags. Left plot shows the percentage of heavy-chain-conditioned sequences classified as heavy chains, for each species conditioning tag. The remaining plots show the percentage of light-chain-conditioned sequences, further divided by whether initial residues were characteristic of lambda or kappa chains, classified as lambda or kappa chains.

<img width="922" alt="Screen Shot 2023-10-25 at 5 36 03 AM" src="https://github.com/vrhoward/iglm-overview/assets/107573643/969963e5-65d7-4b99-8ce4-8eaeeab54546">

&nbsp;

While the results of the heavy-chain-conditioned sequences and light(kappa)-chain-conditioned sequences seem to match the results in the paper, the results of the light(lambda)-chain-conditioned sequences have distinct differences. When conditioned on the [LIGHT] chain tag and prompted with characteristic lambda residues, the generated sequences show a 50/50 distribution between lambda and kappa chains for most of the species rather than a 100% generation of lambda sequences, as seen in the paper's results.

Visit the [ReproduceResults.ipynb](https://github.com/vrhoward/iglm-overview/blob/main/ReproduceResults.ipynb) to see the code used for recreation of the results shown in Figures 2c and 2d from the original paper.

## Code Demonstration

Visit the [Examples.ipynb](https://github.com/vrhoward/iglm-overview/blob/main/Examples.ipynb) for a brief demonstration of the model usage for sequence infilling, full sequence generation, and likelihood calculations.

## Additional Resources and Links

**1. To learn more about antibody structure and function:** Chiu ML, Goulet DR, Teplyakov A, Gilliland GL. Antibody Structure and Function: The Basis for Engineering Therapeutics. Antibodies (Basel). 2019 Dec 3;8(4):55. doi: 10.3390/antib8040055. PMID: 31816964; PMCID: PMC6963682.

**2. To read the original paper presenting Infilling by Language Modeling (ILM):** Donahue, Chris, Mina Lee, and Percy Liang. “Enabling Language Models to Fill in the Blanks.” arXiv, September 10, 2020. http://arxiv.org/abs/2005.05339.

**3. To reference relevant notation for the pseudocode shown above:** Phuong, Mary, and Marcus Hutter. “Formal Algorithms for Transformers.” arXiv, July 19, 2022. https://doi.org/10.48550/arXiv.2207.09238.

**4. [Official Repository for IgLM](https://github.com/Graylab/IgLM)**

**5. [IgLM Google Colab](https://colab.research.google.com/github/Graylab/IgLM/blob/main/IgLM.ipynb)** 

## Video Recording


https://github.com/vrhoward/iglm-overview/assets/107573643/190d8417-f514-4dc2-9196-0e14bef5e1cc


