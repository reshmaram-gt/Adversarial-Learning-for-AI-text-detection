# AI-text-detection

## Table of Contents
- [Introduction](#introduction)
- [Dataset](#dataset)
- [Baseline](#baseline)
- [Our Approach](#our-approach)
- [Authors](#authors)

## Introduction
Lately, Large Language Models (LLMs) are being explored quite extensively for text generation. However, with the advent of such technologies, it is very important to prevent misinformation. Thus, detection of text generated by Artificial Intelligence (AI) is necessary to distinguish between actual and fake information. This project proposes a model that can identify if the given text was AI-generated or not using adversarial learning. Inspired by the [RADAR framework](https://arxiv.org/pdf/2307.03838) , we train two networks, an ensemble paraphraser and a detector, that compete with one another to learn and enhance the efficiency of the detection model. The proposed approach outperforms the RADAR implementation on the Wiki Intro Dataset with an AUROC score of 0.78.


## Dataset

- [The Wiki Intro Dataset](https://huggingface.co/datasets/aadityaubhat/GPT-wiki-intro) is used to train the model. It comprises of 150k topics, with a varied distribution of machine-generated text, created by GPT (Curie) model and human-written text. These samples cover various domains, collected from Wikipedia. A prompt was used to generate the GPT response using the title of the Wikipedia page and the first seven words from the introduction paragraph. Prompt used for generating text: **200 word Wikipedia-style introduction on '\{title\}' {starter\_text\}** where \{title\} is the title for the Wikipedia page, and \{starter\_text\} is the first seven words of the Wikipedia introduction.

## Baseline

Two baseline models were implemented:<br>
i. Model 1: Probabilistic Loss for Detector Training: The paraphraser model, who's weights are frozen, generates paraphrased outputs of given input text. The detector trains on this data, attempting to predict whether the text is AI-generated or not. The detector model trains on Mean-Squared Error loss(MSE), and the loss is backpropagated during  training, across the entire LM. This model helps analyze the effect of adversarial learning in improving AI-text detection.

<img width="400" alt="image" src="https://github.com/reshmaram-gt/AI-text-detection/assets/115122663/07b3cf03-23f2-45f3-b716-eb42b04d103b"> <br>

ii. Model 2: RADAR framework: Here, the detector model, a pre-trained RoBERTa model, goes through the training loop to distinguish between AI-generated and human-written text. Simultaneously, a pre-trained T5-large paraphraser rephrases AI-generated text, aiming to evade detection by the aforementioned detector model. The paraphraser undergoes training where the output from the detector serves as a reward signal via the Proximal Policy Optimization (PPO) framework. This incentivizes the paraphrasers to generate text that the detector is less likely to classify as AI-generated. This model helps compare the proposed approach to the state-of-the-art RADAR approach.

## Our Approach

- Run the [notebook](https://github.com/reshmaram-gt/AI-text-detection/blob/main/RADAR.ipynb), which will automatically install the necessary dependencies to yield the output.

- We utilized the Google Cloud Platform(GCP) to avail GPU’s to train our model. We used a ’g2-standard-24’ Machine Type, using two ’Nvidia L4 GPU’s as accelerators, with a Disk Space of 100GB. For evaluating the outputs, we utilized another machine so as to avoid running into resource issues. We used a ’g2-standard-16’ Machine Type supported with one ’Nvidia L4 GPU’ accelerator, with a Disk Space of 100GB.

- The proposed approach utilizes an ensemble paraphraser and a fine-tuned RoBERTa-based detector for AI text detection.

<img width="400" alt="image" src="https://github.com/reshmaram-gt/AI-text-detection/assets/115122663/c1d8959b-1583-455c-8151-8678b17636e7">

- The AUROC plots for the three models shows that the proposed model outperforms the two baselines for the Wiki Intro Dataset.

<img width="400" alt="image" src="https://github.com/reshmaram-gt/AI-text-detection/assets/115122663/b7f3a8ce-73f6-4d49-b772-2536ac69386f">

- The final outputs and evaluation results are detailed in the [Final Report](https://github.com/reshmaram-gt/AI-text-detection/blob/main/Final%20Report.pdf)

## Authors

- Reshma Anugundanahalli Ramachandra
- Aryan Vats
- Deeksha Manjunath
