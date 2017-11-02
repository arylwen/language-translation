# language-translation
Udacity nanodegree language-translation project

# Running with a GPU
Running this project was a bit tricky.

1. My GPU credits on Floyd expired
2. The AWS instance comes with Tensorflow 1.0 and the project requires Tensorflow 1.1 or above

Choices:
1. Enter credit card and run the project
2. Fiddle with the aws instance and learn something new

I was not, under any circumstance going to upgrade tensoflow in the default env, dl. If ain't broken, dont fix it :)
This is how it went:

1. I created a new env called tensorflow (of course)

conda create -n tensorflow python=3.5

(In truth, I created it this way: conda create -n tensorflow , which installed python 3.6. Tensorflow complained 
right away it was not compiled against 3.6; remove env and create a 3.5 one)

2. Install supporting dependencies
conda install pandas matplotlib jupyter notebook scipy scikit-learn

3. Install tensorflow
pip install tensorflow keras

Nope! Definitely nope! Ok, after some googling I found about tensorflow-gpu. Well, tensoflow and tensoflow-gpu can coexist and any code will pick up the first one.
Redux:
pip install tensorflow-gpu keras  (no tensorflow)

4. Restart jupyter (or at least the kernel for the notebook) to pick up the new version
(Well, I found out the hard way, when I got a __version__ not found. What??? This is how I discovered print(dir(tf))).

5. Surprise! yes, we have a special talent to build up stories and be surprised by them
I mean, "Failed to load the native TensorFlow runtime" is quite a message.
Here is a hint: https://github.com/tensorflow/tensorflow/issues/10026

What do you mean the aws instance has an old version of gcc? I looked at the tensorflow version: 1.4. Then it dawned on me.
I downgraded tensorflow until, eureka, I got the much expected message:

Default GPU Device: /gpu:0

You will also see: TensorFlow Version: 1.2.0 which is apparently the highest tensorflow version which could run on the aws standard gpu build.

# Translation results

Here are some good results for "he saw an old yellow truck":

Epochs 10  Keep prob 0.75  "il a vu un vieux camion prochain"
Epochs 10  Keep prob 0.5   "il a vu la vieille voiture"
Epochs 10  Keep prob 0.85  "il vu cette vieille voiture"
Epochs 10  Keep prob 0.45  "il a de petite voiture bleue"

And whoa! check out these settings!
Epochs 12  Keep prob 0.70  "il a vu un vieux camion jaune"
Talk about perfect translation!  

# Overfitting

Overfitting is indeed one of the main issues with seq2seq networks. 

More is not better, as we would usually expect. 20 epochs with .075 keep prob plumeted the loss all right.

However the translation to "he saw an old yellow truck" resulted in "il n'aimait pas un vieux en été"

Although I wouldn't call it overfitting.

I'd call it poetry.


