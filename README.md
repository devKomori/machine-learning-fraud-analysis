# Comparative Analysis of Machine Learning Models for Phishing and Fraud Detection

## Introduction

A phishing email is a fraudulent message from a cybercriminal who impersonates a trusted source to trick you into revealing sensitive information, such as passwords, credit card numbers, or bank details. In this report predictions will be made to distiguish if an email is phishing/fraudulent, or a real email.

The goal of this project is to compare different machine learning models that can distinguish between phishing and legitimate emails based on their content.
We plan to use the columns `subject`, `body`, `sender`, and `urls` to predict the `label` column, which indicates whether an email is phishing or legitimate.

## Data Selection

This project is using the [Phishing Email Dataset](https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset) from Kaggle. This dataset contains approximately 82,500 emails, with 42,891 labeled as phishing and 39,595 as legitimate. The emails are sourced from Enron, Ling, CEAS, Nazario, SpamAssassin, and Nigerian Fraud emails. For this project, we will focus on the CEAS subset, as it provides more relevant features and is less dated.

## Methods



## Results



## Discussion



## Summary

These findings show that machine learning can be an exceptionally effective tool at identifying email legitimacy. It's clear that any modern spam filter making use of machine learning models in some capacity will be more effective than its competitors. A total of 16 misses in the confusion matrix for the Linear SVC model in particular is quite staggering across a data pool of nearly 8000 entries.
