---
title: 'Music Information Retrieval on FMA'
author:
- Michaël Defferrard^1^
- Chibueze Ukachi^1^
include-before: ^1^ LTS2, EPFL, Switzerland
date: \today
numbersections: true
lang: en
header-includes:
- \hypersetup{colorlinks=true,allcolors=blue}
---

# Administration

* People involved
	* Student: Chibueze Ukachi, \texttt{chibueze.ukachi@epfl.ch}
	* Supervisor: Michaël Defferrard, \texttt{michael.defferrard@epfl.ch}
	* Professor: Pierre Vandergheynst, \texttt{pierre.vandergheynst@epfl.ch}
* Dates
	* Project duration: 20.02 - 02.06.2017 (14 weeks)
	* Delivery of the report to the lab: 09.06.2017
	* Grades to be given to the Registar's office: 23.06.2017
* Amount of work: 10 ECTS (1/3 of a semester)
* Evaluation: A Jupyter notebook which explains (i) the problem, (ii) the
  solution and, (iii) the results. Produced at the end of the project, no
  presentation required.
* Meeting of ~1h every week
* Repository: <https://lts2.epfl.ch/gitlab/michael.defferrard/fma-baselines>
	* Every file produced during the project should be in there.
* Dataset: Free Music Archive
	* Code & data: <https://github.com/mdeff/fma>
	* Paper: <https://arxiv.org/abs/1612.01840>
* The results obtained during this project will hopefully be integrated in the above paper.
	* ISMIR paper deadline: abstract April 21, full paper April 28

# Goal

The goal of this semester project is to provide baselines for genre
classification, and maybe other problems in Music Information Retrieval (MIR)
if time allows, on the [FMA dataset](https://github.com/mdeff/fma). We will
first test simple ML algorithms then devise and test Deep Networks. The idea is
to show that the dataset is large enough for end-to-end learning, i.e. from the
raw audio, with current DL techniques. This task involves the use of Python,
the Jupyter notebook and libraries such as scikit-learn and Keras (TensorFlow
backend).

## Plan

1. Try simple classifiers from scikit-learn.
	* Play with the dataset.
	* Get a sense of how difficult the problem is.
	* Learn about ML.
1. Implement a first simple fully connected feed-forward NN.
	* Learn Keras and see how NN works.
1. Find state-of-the-art DL and non-DL methods for MIR from raw audio.
1. Implement more convoluted models.
	* 1D CNNs on raw waveforms.
	* 2D CNNs on spectrograms.
	* RNNs: LSTMs, GRUs.
1. Eventually compare with echonest features.
1. Iterate and incrementally increase accuracy.

# Meetings

## 2017-02-07 (email)

Become familiar with Deep Learning. You won't need an in-depth knowledge to
work on the project, but an understanding of what is going on will be
necessary. Good resources are:

* this very complete book: <http://www.deeplearningbook.org/>
* or this course: <http://cs231n.github.io/>
* or this MOOC: <https://www.coursera.org/learn/neural-networks>

You may also want to look at the Keras library: <https://keras.io/>

## 2017-02-14

**Admin**

* Find a place in lab, ask Rosie for accreditation
* Links: GitHub, paper
* Fix day and time to meet
* git repository on gitlab
* no machine with GPU, we'll make an account on the CDK

**Practice**

* Chibu knows about scikit-learn, git, Jupyter, and ML from ADA
* Will have to learn Keras and Deep Learning

**For next week**

* Register on IS-Academia
* Find a meeting time
* Import *fma_small* meta-data with pandas and explore the dataset
* Load and listen to audio clips from Python

## 2017-02-22

**Admin**

* We set up gitlab and cloned our private repository

**Theory**

* Loss function for classification: [cross entropy (negative log loss)](https://en.wikipedia.org/wiki/Cross_entropy)
* Optimization algorithm (for Neural Networks): [stochastic gradient descent](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)

**Practice**

* Python library for audio feature extraction: [librosa](https://github.com/librosa/librosa)
* Music Information Retrieval (MIR) resources
	* [Stanford CCRMA workshop material](http://musicinformationretrieval.com)
	* [Code & slides from a book](http://www.audiocontentanalysis.org)

**For next week**

* Familiarize yourself with MIR
* Import *fma_small* meta-data with pandas and explore the dataset
* Load and listen to audio clips from Python
* If time allows, try a linear regression with sklearn for genre recognition

## 2017-03-03

**Achieved**

* Found references on ML for MIR
* Notebook: read DataFrame, load and listen to songs, visualize various spectrograms computed by librosa, spectral features extraction
* Good pipeline!
* Found 2 problems in `fma_small`: some interviews and duplicate songs

**Discussed**

* Computer memory: don't expect to be able to load whole datasets
	* Load by mini-batches when you need them, e.g. compute features for 100 songs, then for the next 100, etc.
	* Store as [HDF5](https://support.hdfgroup.org/HDF5/) which only load when data is accessed
	* If the features take time to be computed, it could be worthwhile to save them to disk so that feature extraction does not have to be done again
* librosa resampling is very slow, use `librosa.load()` with `sr=None` for a ten folds speedup
* Features extraction is a transformation of the representation
	* For a better representation (be it sparse, compressed, etc.) for the ML task
	* For dimensionality reduction (compression / summarization) --> computational efficiency, data visualization
* Little trick to scale your figures in the notebook: `plt.figure(figsize=(15, 5))`

**For next week**

* Extract only one feature first and use it with a classifier for Genre Recognition
	* Compute accuracy on training and testing sets
* Try adding / removing features and observe the impact on the accuracy
* Goal: identify the best features, i.e. the most discriminative for our task

## 2017-03-10

**Achieved**

* Started some Machine Learning, inspired by NTDS

**Discussed**

* What is time-frequency analysis. Then log scale on frequency axis.
* Filling the feature matrix one row at a time.

**For next week**

* Compute and save a feature matrix of size 4'000 by #features
* Try linear regression on it and see the accuracy

## 2017-03-16

**Achieved**

* Fixed the feature extraction bug.
* Obtained between 30 and 40% accuracy on `fma_small`.
* Used features: (i) mean and std on whole MFCC, (ii) mean per MFCC row, (iii) std per MFCC row.

**Discussed**

* What is MFCC, how is it computed.
* Paper with list of features: [A Survey of Audio-Based Music Classification and Annotation](http://ieeexplore.ieee.org/abstract/document/5664796/).
* MIR system: (i) clipping, (ii) feature extraction, (iii) classification, (iv) voting.

**For next week**

* Refactor code to have less copy / paste, e.g. by using functions.
* Instead of mean and std, try with whole MFCC.
* Then either:
	* Choose another feature and try to beat MFCC! Maybe join the two.
	* Or cut in clips and use voting.

## 2017-03-27

**Achieved**

* Divided songs into sections (or clips).
* New features: spectral centroids and onsets for whole songs.

**Discussed**

* Shape of arrays and arrangement in computer memory. Numpy innermost convention.
* STFT window size `n_fft` should be set, smaller (meaning overlaps) or equal to the hop size.
* Never use `np.matrix`. Only use `np.ndarray` and `*` for element-wise multiplication and `@` for matrix multiplication.

**For next week**

* Implement voting
* Continue with spectral centroids
* Continue last week's tasks

## 2017-04-04

**Achieved**

* Train on features extracted from sections
* Voting implemented

**Discussed**

* Statistics or not to resume features
* `n_fft` and `hop_length`
* Confusion about sections and tracks in ML part

**For next week**

* Compute features for all sections
* Do the train / test split on songs, not sections

## 2017-04-13

**Achieved**

* Make sure about dimensions: song, segment, hop, #features.

**Discussed**

* Don't waste computations by reusing the spectrogram for multiple features.
* Split is given
* I will send you paper and code about new data format

**For next week**

* Be done with features
* You can test ML part on Echonest features if you want

## 2017-04-28

**Achieved**

* Not much, prediction on segments is still not working

**Discussed**

* Report
	* Length: $\approx 15$ pages
	* Questions you have to answer:
		1. What is the problem?
		2. Why is it important?
		3. What others are doing?
		4. What did you do?
		5. Show and interpret your results.
		6. Conclusion
	* Deadline: June 9th
* Feedback on his way of working
	1. Focus & try to be more organized, e.g. with idea or code
	2. Keep a steady pace (rapid at the beginning, slow in the middle)
	3. Start simple and build complexity on top, do not move further while
	   something is not working. Never do something you don't understand.
* Links to new dataset version, code and paper
* Precisely explained how do extract features by segments.

**For next week**

* Write the structure of the report.
	* Titles (table of contents) and one sentence about what you'll write in each section.
* Implement the presented method for feature extraction. It should not be more than 10 lines of Python!
* Then use the ML and voting scheme already developed.

## 2017-05-05

**Achieved**

* Report shell
* Feature extraction on segments

**Discussed**

* Report
	* Table of contents
	* What to write where
* Hyper-parameters
* Voting: only during testing, training is done on segments
* Accuracy: measured on the segments and on the songs
	* Accuracy on songs should be larger than the one on the segments

**For next week**

* Train and test ML models on segments
* Measure accuracy on segments
