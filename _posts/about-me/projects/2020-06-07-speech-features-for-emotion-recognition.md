---
layout: page
subheadline: "My Projects"
title: "Speech Features for Emotion Recognition"
header:
    image_fullwidth: "posts/about-me/projects/speech-features-for-emotion-recognition/speech-features-for-emotion-recognition-header.jpg"
image:
    thumb:  posts/about-me/projects/speech-features-for-emotion-recognition/speech-features-for-emotion-recognition-thumbnail.png
    homepage: posts/about-me/projects/deep-learning-semantic-layout-to-realistic-image/speech-features-for-emotion-recognition-setup.jpeg
categories:
    - about-me
    - projects
    - technical
tags:
    - projects
    - technical
    - deep-learning
    - machine-learning
comments: true
show_meta: false
breadcrumb: true
---

<div class="row">
<div class="medium-4 medium-push-8 columns" markdown="1">
<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>
</div><!-- /.medium-4.columns -->

<div class="medium-8 medium-pull-4 columns" markdown="1">

## DESCRIPTION

Extracted and evaluated all the features for speech emotion recognition task. Used utterances that belong to the following 4 categories, happy, neutral, sad and angry for recognizing emotions. The performance of each feature representation was evaluated using Leave-One-Subject-Out cross validation approach with weighted and unweighted recall, precision and F1-score as metrics.

## TRAINING DATA
The training data used in this experiment comes from the Interactive Emotional Dyadic Motion Capture (IEMOCAP) database. This is an acted, multimodal and multispeaker database, recently collected at SAIL lab at USC

![Data Collection Setup]({{site.urlimg}}posts/about-me/projects/speech-features-for-emotion-recognition/speech-features-for-emotion-recognition-setup.jpeg)

## TECHNOLOGIES USED

Python, Numpy, Linux

## TOOLS USED
TAMU High Performance Computing Grid, IEMOCAP database

## Feature Extraction
### Low - Level descriptors (Frame - level features)
Speech is a non-stationary signal as the frequencies and the way speech is produced
changes rapidly. Hence, speech is analyzed at a frame-level where the whole speech is
divided into frames of certain length and features for each frame are computed. In this
work, we have computed various frame level features using the PRAAT software in
which the frame length was set as 25 ms and frame shift was set as 10 ms. Various
features we computed are as follows:

#### Mel - Frequency Cepstral Coefficients (MFCC)
MFCC are traditional features used for representing speech and are used in various
tasks such as speech recognition, speaker recognition, speech emotion recognition etc.
as they capture the vocal tract information from a speech signal. We extracted 13
dimensional MFCC features from speech signal with 1 of them being the intensity
(energy) of the signal.

#### Linear Prediction Cepstral Coefficients (LPCC)
LPCC are features which are generally used in voice coding tasks where voice
compression is the main goal. Linear prediction technique allows speech signal to be
separated into vocal tract filter and excitation source approximations where LPCC
coefficients are used to represent the vocal tract filter characteristics. There have been
only a few works or no works that have explored speech emotion recognition using
LPCC. Hence, we have explored LPCC features for this task.

#### Residual - Mel Frequency Cepstral Coefficients (RMFCC)
As the features mentioned above, MFCC and LPCC capture information related to the
vocal tract, the other key part of speech production is being underrepresented or not
used. But, the excitation source signal contains information related to emotion
recognition as mentioned in [][][]. Hence, various works have explored features that can
be extracted from the residual signal which is an approximation of the excitation source
signal computed using linear prediction analysis. One of such works is RMFCC, which
was first introduced in [] and has been used for various speech related tasks such as
[][][].

### Utterance - level Descriptors
As categorical emotion labels for a speech signal are given at an utterance level, there
are two ways of constructing speech emotion recognition systems. One is to develop
features at an utterance level and other is to consider the label given to an utterance as
the label for all the frames within it and to develop systems using all frames as training
data. In the latter systems, testing can be done using some sort of majority voting or
median voting strategies. But, the disadvantage of such systems is that temporal
information is not taken into account and statistics are only captured at a classifier level
instead of feature level. Hence, we use the former approach where we develop features
for each utterance. Even in such kind of approach, one approach that has been followed
in the past was gaussian mixture modeling (GMM) where the differences in gaussian
mixture components between each utterances are analyzed and used for classifying
between emotions. But the disadvantages of this approach included requirement of a
huge dataset for initial modeling of speech for starting point of GMM called the universal
background model (UBM) and the inability to capture the time-series information. Hence,
we have used long short term memory (LSTM) based deep-features for capturing
information from the low-level descriptors described in section 4.1 and have also
extracted a few other features that are mentioned in the literature for speech emotion
recognition. These included LSTM autoencoder representation, LSTM categorical
embeddings, jitter, shimmer, harmonics-to-noise ratio (HNR) and probability of voicing.
Apart from these, we have used the opensmile toolkit to extract features in the
paralinguistic challenge 2010 configuration as these features are used in various
emotion recognition and paralinguistic works in the past. The details of the features and
extraction procedure followed for utterance-level features are described below.

#### LSTM - Autoencoder Representation
To capture the information in an utterance as a single vector, we train a LSTM network
to predict itself and collect the hidden representation at the end of the encoder to
represent the speech signal. During this process, all the utterances are required to have
a unique length as the LSTM network is fixed. Hence, after extracting the LLDs, the
number of frames in each utterance was analyzed. A histogram plot of number of frames
vs number of utterances can be found in fig. … Since, there was a peak around 250
frames, we have computed the accuracy of LSTM Autoencoder using 150, 200, 250 and
300 frames where training was performed on first 80% of the training set and validation
on the last 20% and it has been observed that 200 frames gave slightly higher
performance. Hence, all the utterances were either truncated or padded with zeros to
contain 200 frames only. An LSTM encoder is developed in this way for each Low-level
descriptor which we call MFCC_LSTM_Autoencoder, LPCC_LSTM_Autoencoder and
RMFCC_LSTM_Autoencoder. The dimension of these representations is designed to be
256.

#### LSTM - categorical Embedding
The processing of the Low-level descriptors was performed in a similar fashion as
mentioned in LSTM - autoencoder representation framework where 200 frames were
chosen to represent each utterance. The structure of the network contained two-stacked
LSTMs with 512 and 256 hidden units respectively followed by a fully connected layer to
a hidden layer of 256 nodes with a ReLU activation function. This layer is considered as
the representation layer and it is in turn connected to the output layer with softmax
activation containing 4 nodes. A network is trained for each LLD and they are called
MFCC_cat_embedding, LPCC_cat_embedding and RMFCC_cat_embedding.

## Data Description
In this project, we used the Interactive Emotional Dyadic Motion Capture (IEMOCAP)
database for training and testing our model. IEMOCAP stands for Interactive Emotional
Dyadic Motion Capture database and has the following features:
1. It is an acted, multimodal and multi speaker database
2. It was recently collected at SAIL lab at USC
3. It contains roughly 12 hours of audiovisual data, including video, speech, motion
capture of face and text transcriptions.
4. The data is captured through dyadic sessions where actors improvise on certain
scenes or scripted scenarios which are especially designed to elicit emotional
expressions in the dataset.
5. The database is annotated by multiple annotators into categorical labels, such
as:
a. Anger, happiness, excitement, sadness, frustration, fear, surprise, other
and neutral state.
6. The dataset also contains dimensional labels such as:
a. Valence, Activation and Dominance values.
7. The dataset sessions are manually segmented into utterances.
8. Each utterance is annotated by at least 3 human annotators.
9. 
In our project, we only use the speech data from the database to train and test our
system. We use the speech data annotated into categorical labels as well as
dimensional labels at utterance level. For classification task, we only use the following 4
categorical labels namely Anger, Happiness, Sadness and Neutrality. We also used the
dimensionals labels of Valence, Activation and Dominance to train our model in order to
achieve better results.

## RESULTS
The performance of each feature representation is evaluated using
Leave-One-Subject-Out cross validation approach where weighted recall and
unweighted recall are computed. The results for the same can be observed in tab. …. In
case of jitter, shimmer, HNR, probability of voicing and opensmile features, data from
one participant is left out for testing and remaining data from 9 participants is used for
training the classifier. In case of LSTM based deep features, both the network and the
classifier are only trained on 9 participants leaving one subject out for testing. Hence, the
network has been trained only for 50 epochs where the model with best results is
captured.

## CONCLUSION & FUTURE WORK
By looking at the results, we can conclude that we have found features(which also
include deep features) for emotion recognition in speech. We can also say that the
results of the deep features are comparable with that of the OpenSmile features, and the
performance of the model can only increase in the future because there will not be
sparse data, also, as deep Neural Networks need vast amount of data to interpret subtle
features, we can say that the results will eventually improve over time.


## PROJECT PRESENTATION

Click to view [PROJECT PRESENTATION]({{site.urlimg}}posts/about-me/projects/speech-features-for-emotion-recognition/emotion-recognition-pres.pdf){:target="_blank"}

## PROJECT REPORT

Click to view [PROJECT REPORT]({{site.urlimg}}posts/about-me/projects/speech-features-for-emotion-recognition/emotion-recognition.pdf){:target="_blank"}



{% include list-posts entries='3' offset='1' tag='projects' %}
{% include next-previous-post-in-category %}
