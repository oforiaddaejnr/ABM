Data is the new oil and truth be told only a few big players have the strongest hold on that currency. Googles and Facebooks of this world are so generous with their latest machine learning algorithms and packages (they give those away freely) because the entry barrier to the world of algorithms is pretty low right now.

Even with so much data out there, no matter how large your dataset is, it is still fixed, with a fixed number of samples, patterns, fixed degree of class separation between positive and negative samples - assuming we are looking at a classification problem

So what can one do to solve this problem? Look for a much larger dataset with the samples you need? Sure, but this takes a lot of time to do. So what can one do instead in situations like this? Synthetic datasets

As the name suggests, quite obviously, a synthetic dataset is a repository of data that is generated programmatically. So, it is not collected by any real-life survey or experiment. Its main purpose, therefore, is to be flexible and rich enough to help an ML practitioner conduct fascinating experiments with various classification, regression, and clustering algorithms.

Synthetic datasets can even be far reaching in cases where it helps one get around security and privacy concerns with real datasets, such as medical data or military data.

Luckily there are various library packages that can assist with this. Some are as follows:

Scikit-learn: This python library has various functions for tackling the need for synthetic data such as the make_regression function which can create random regression problems with arbitrary number of input features, output targets, and controllable degree of informative coupling between them. It can also mix Gaussian noise. Another is make_classification which generates a random multi-class classification problem (dataset) with controllable class separation and added noise. You can also randomly flip any percentage of output signs to create a harder classification dataset if you want. However, while these functions are great, the user has no easy control over what goes on under the hood.
Sympy: This enables you to create functions similar to those available in scikit-learn, but allows you to generate regression and classification datasets with symbolic expression of high degree of complexity


https://towardsdatascience.com/synthetic-data-generation-a-must-have-skill-for-new-data-scientists-915896c0c1ae
