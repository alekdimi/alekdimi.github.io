---
layout: post
title:  Data Scientist | The Sexiest Job of the 21<sup>st</sup> Century
date:   2016-02-25
description: Data scientist - a machine that turns coffee into linear models
comments: true
---
<p><strong>Data scientist</strong>: <em>a machine that turns coffee into linear models.</em></p>

It seems like everyone and their manager wants a data scientist in their company to boost profits and use #bigdata, yet there does not seem to be a good definition of what a data scientist is supposed to do or even what kind of knowledge and expertise he/she must possess. From Drew Conway's famous <a href="http://static1.squarespace.com/static/5150aec6e4b0e340ec52710a/t/51525c33e4b0b3e0d10f77ab/1364352052403/Data_Science_VD.png" target="_blank" rel="noopener">Venn diagram</a> that probably oversimplifies things, to the recent length discussion on <a href="http://stats.stackexchange.com/" target="_blank" rel="noopener">CrossValidated</a>, the aptly-named stack exchange for statisticians, that probably overcomplicates it,

I will not try to present a succinct, yet encompassing definition which is just going to get lost in the sea of failed attempts. But we can at least enumerate the plethora of inter-disciplinary skills that data scientists are expected to have. The degree requirements alone showcase the versatility of this position,  ranging from a degree in any of the following: Computer Science, Statistics, Applied Math, Physics, Engineering, or basically any quantitative field. On top of this, the degree can also be either a BSc, MSc or PhD in any of these areas. Now, turning to the skills, we can split them into a few broad areas of expertise, and the more the better when it comes to a candidate possessing them. So basically, you're expected to be familiar with every concept described below.

<strong>Computer Science</strong>
<ul>
 	<li><em>R &amp; Python </em>- You want a scripted language for fast prototyping, and these two are equipped with excellent data manipulation (<em>numpy</em>, <em>pandas</em>) and visualization (<em>ggplot</em>, <em>matplotlib</em>), in addition to machine learning frameworks (<em>scikit-learn</em>).</li>
 	<li><em>Parallelism &amp; MapReduce </em>- And the flavor-of-the-month implementation, which used to be <em>Hadoop</em>, but is being overtaken by <em>Spark</em></li>
 	<li><em>Algorithms &amp; CS fundamentals </em>- Time/space complexity can prove to be very useful if you happen to use a model with <em>O(n³)</em> training time, which is going to take years in the age of <strong>#</strong><em>bigdata</em><strong>.</strong></li>
 	<li><em>Databases &amp; Relational Algebra</em> - Although 90% of the time, you'll get a <em>csv </em>file, it doesn't hurt to be familiar with <em>SQL</em>, <em>NoSQL </em>etc.</li>
</ul>
<strong>Math/Statistics </strong>
<ul>
 	<li><em>Linear &amp; Matrix Algebra </em>- The backbone of machine learning and statistics, a must-know since half of machine learning is matrix products (<em>OLS</em>, neural nets, <em>PCA</em>, Gaussian processes, recommendation systems, the list goes on).</li>
 	<li>Probability - It's important to know your distributions and their assumptions, e.g. when you use a Gaussian for non-normal data (which probably happens often</li>
 	<li>Frequentist statistics - Correlation, t-tests, maximum likelihood estimation and statistical significance broadly is the bare minimum, but really, this is one area where it is vital to have some background so as to avoid pitfalls such as p-value hacking (wheor false positives</li>
 	<li>Bayesian statistics - When you realize that all of the above is just Bayesian inference with a flat prior and that it's actually more intuitive than the ridiculous definition of a frequentist confidence interval*, you will come to see its superiority, only to realize that obtaining full posterior probability distributions is actually intractable for the slightly large data sets that you will mostly encounter.</li>
</ul>
<strong>Machine Learning</strong>
<ul>
 	<li>Supervised learning - Whenever you hear about <i>ML </i>in the news, its yet another breakthrough and an application of supervised algorithms, and you should definitely know as many as you can. From tree-based methods and their famous representative<em> random forests</em>, to <em>deep learning </em>and <em>convolutional nets</em> , to max-margin methods (SVM), there is an endless supply, though the aforementioned are ones that consistently perform well and are commonly used.</li>
 	<li>Unsupervised learning - The untamed wilderness, where the data can always be clustered and the evaluation criteria don't matter. Although it's a legitimate area of research, it's most often used as a mere preprocessing step (PCA) for supervised methods. However, the clustering methods are still useful when the goal is to find some structure.</li>
 	<li>Overfitting - If there is key insight from <em>ML</em>, it's that you shouldn't test on the same data set you trained. Although this is something that anyone with at least a bit of <em>ML</em> knowledge will know, there is surprising number of people that are not familiar with this concept. For those who think this obvious, there is a simple mistake that many make, which is using feature selection on the dataset before setting aside the test. Since feature selection works by looking at the relationship between the features and what you're trying to predict, you have now implicitly used the test set's values that you will later be trying to predict, which will probably lead to overfitting.</li>
</ul>
* in the limit of an infinite number of estimations of the confidence interval, the true value of the parameter will lie in 95% of all random samples of the given size, simply put.

Lastly, how does the workflow of a data scientist look like? This is probably the least contentious area to define:
<ul>
 	<li><em>Data acquisition</em> - Although sometimes you have to scrape the data from the web etc., most of the time you are just given a #<em>bigdata</em> file.</li>
 	<li><em>Data cleaning/munging</em> (the hard part) - If you are blessed enough to obtain a relatively clean data set, then you are either a <a href="https://www.kaggle.com/" target="_blank" rel="noopener">kaggler</a> or really lucky, because an exorbitant amount of time is used to shape up your features, remove redundant and noisy ones etc. All this is only exacerbated by the fact that this step is by far the most drudging.</li>
 	<li><em>Data modelling</em> - Now that you've shaped up your data, you can finally show off your machine learning and statistics skills to amaze everyone with the predictive power at your disposal. Turns out, the simple models usually work well enough, and no one likes the black-box random forests and neural nets, since you don't know how your model arrived at the answer. Well, at least you used a library and didn't code your own algorithms.</li>
 	<li><em>Data presentation</em> - Whatever insights you may have gleaned through sweat and tears, it's the presentation that matters a lot. You need to showcase your results and your effort and amaze everyone with the power of machine learning that enamored you in college. Don't forget to use <em>ggplot</em> for some beautiful bars and graphs (no pie charts!) that show how much profit this will bring.</li>
</ul>
So there you have it, yet another list of competences a data scientist is supposed to have, but we are probably no close to finding the pithy definition. Still, it's probably useful to be versed in these categories. Whatever I've omitted is probably either inapplicable to most data scientists, or is easy and quick to learn (e.g. plotting and visualization).

<strong><em>Sources:</em></strong>
<ul>
 	<li><a href="http://stats.stackexchange.com/questions/195034/what-is-a-data-scientist" target="_blank" rel="noopener">What is a data scientist?</a></li>
 	<li><a href="http://www.dataists.com/2010/09/the-data-science-venn-diagram/">The Data Science Venn Diagram</a></li>
 	<li><a href="http://ouzor.github.io/blog/2015/02/02/data-science-definition.html" target="_blank" rel="noopener">Data scientist - statistician, programmer, consultant and visualizer?</a></li>
 	<li><a href="https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century/" target="_blank" rel="noopener">Data Scientist: The Sexiest Job of the 21st Century</a></li>
</ul>