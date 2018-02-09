---
title:  "Coursera Deep learning [Course 2 - Week 1]"
last_modified_at: 2018-02-08T11:45:04-04:00
categories: 
  - deeplearning
tags:
  - knowledge
header:
  overlay_image: /assets/images/nn-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/7.1.2/mermaid.js">
</script>

{% include toc title="Week Objectives" %}

## Train/Dev/Test/ Sets

Sometimes you need to test different algorithms (models) on your data, doing so on such big data can be time consuming, so the best way to do this is divide your data into train, dev and test set.
- train set used for training and learning the parameters.
- dev set for model selection.
- test set for testing the final model (the selected model) that is already trained.

* other names for dev set:
    * development set
    * hold-out set
    * cross-validation set
* for big data it’s ok to divide your data like this -> 98% (train), 1% (dev),  1% (test).
* make sure your dev and test set comes from the same distribution
* not having test set might be ok only train/dev sets.


<font color="red">Can we omit the test set and just combine dev and test sets into one set?</font>
Yes, the separation of test and dev set was meant to only get an unbiased error estimate of the final model.


## Bias and Variance

Recall that
- high bias means under-fitting
- high variance means overfitting


<figure>
	<a href="/assets/images/dl/bias-and-variance.png"><img src="/assets/images/dl/bias-and-variance.png"></a>
	<figcaption>Bias vs Variance</figcaption>
</figure>

If we had two features \\(x1, x2\\)  we can easily plot the decision boundary and know if we have high bias or high variance, but for larger features we will use the the train and dev set errors

| Train set error | dev set error |       |
| -------------   |:-------------:| :-----|
| 1%        | 11% | this indicates that we have overfitting on the train set (high variance) |
| 15%       | 16% | this indicates underfitting (high bias) |
| 15%       | 30% | high bias and high variance
| 0.5%      | 1%  | low bias and low variance |

__*NOTE*__:
we alway assume that the human error is 0% which is also called base error, if the base error is something like 15% then for the second example it wouldn’t be underfitting.

## TROUBLESHOOTING

<div class="mermaid">
graph TD;
	A[model]-->B;
    B{High bias}--> |Yes| C["-Try bigger network (more layers or/and more hidden units). <br/>-Train for longer time, use another algorithms.<br/>- Search about other architectures online."];
    B--> |No| D{High variance};
    D--> |Yes| E["-Get more data (may be expensive)<br/>- Use regularization.<br/>-Search about other architecture."];
    D--> |No| F[COOL!];
</div>

** bias and variance tradeoff

## Regularization 

Sometimes it’s expensive to get more data to solve high variance problem, regularization would be your best option.

**_<u>Recall</u>_**: in logistic regression 

$$ J(W, b) = 1/m \sum_{i=1}^m \mathcal{L}(\widehat{y}, y^{i}) + \dfrac{\lambda}{2m} \Vert w \Vert^2_2 + \underline{\dfrac{\lambda}{2m}b^2} $$

* This is called L2 regularization.
* note that the last term can be ignored

to be continued ..

---

<!-- Notes: -->
<!-- * todo -->

**Resources**: [Deep Learning
Specialization on Coursera, by Andrew Ng](https://www.coursera.org/specializations/deep-learning)
