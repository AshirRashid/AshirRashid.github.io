---
title: "Effective Dataset Inference"
excerpt: "Exploring how effective current dataset inference methods truly are.<br/>"
collection: portfolio
---

The project utilizes the Pythia model family (1.4B-6.9B parameters) due to its transparent training and validation data composition, which enables controlled experiments with known membership. These models were been trained on the PILE dataset consisting of multiple comprehensive sources, which can be used to simulate real-world.

I analysed the following membership inference attacks presented in literature:

* Thresholding Based - Leveraging loss or perplexity
as scores and thresholding them to classify samples

* MIN-K% PROB - Evaluating the likelihood of the K%
of tokens in a sequence that have the lowest probability

* Perturbation Based - Comparing perplexity of origi-
nal text versus perturbed versions

* White-space perturbation
    * Synonym substitution
    * Character-level typos
    * Random deletion
    * Changing character case

* DetectGPT - Using an external language model to in-
fill randomly masked-out spans and comparing proba-
bilities

* Reference Model Based - Comparing perplexity ra-
tio between suspect and reference models
    * Using SILO model as reference
    * Using Tinystories-33M model as reference
    * Using Tinystories-1M model as reference
    * Using Phi-1.5 model as reference
    * zlib Ratio - Comparing ratio of model’s perplexity to entropy of text when compressed with zlib

After ensuring fair evaluation, which some of these papers failed to do, these methods performed no better than random.

Currently, I'm analysing how these techniques perform on compressed models. The code will be made public when this project concludes.

**Technologies:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Tensorflow</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">NLTK</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">spaCy</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Hugging Face Transformers</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">PyTorch</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">NumPy</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Pandas</span>

**Concepts / Algorithms:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ML</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LLM</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ML security</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ML privacy</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Data Membership Inference</span>
