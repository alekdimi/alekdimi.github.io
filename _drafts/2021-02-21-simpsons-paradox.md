---
layout: post
title:	simpson’s paradox and the shattering of a mental model
date:   2021-02-21
description: not to be confused with The Simpsons paradox, which requires time travel to explain the amazingly correct predictions of the show
hidden: true
---
### the paradox

Let's say that in some hospital there are 1000 people hospitalized with COVID, with 500 of them having a mild case and 500 with a severe case. Also, 500 people were given Advil, and another 500 were given Bleach. Is it possible that the following two things could be true at the same time?

1. Advil has a *lower* survival rate than Bleach (from looking at the 1000 patients in total)
2. Advil has a *higher* survival rate than Bleach for *both* mild (500 patients) and severe COVID cases (500 patients)

Surprisingly, the answer is *yes*! It is so unintuitive that it is called a paradox. Here is a few numbers that I just made up:

| Treatment \| Case |           Mild           |         Severe          | Total survival rate                 |
| :---------------: | :----------------------: | :---------------------: | ----------------------------------- |
|       Advil       | $\frac{90}{100} = 90\%$  | $\frac{60}{400} = 15\%$ | $\frac{90 + 60}{100 + 400} = 30\%$  |
|      Bleach       | $\frac{340}{400} = 85\%$ | $\frac{10}{100} = 10\%$ | $\frac{340 + 10}{400 + 100} = 70\%$ |

### the shattering

Let's recap what the table says:

1. A is worse than B for total survival: 30% < 70%
2. A is better than B for mild cases: 90% > 85%
3. A is better than B for severe cases: 15% > 10%

Okay, so which information do we use, the aggregated data or the data separated by severity? First thought: if you don't know the severity of the case, give them Bleach, and if you do, give them Advil. But Advil works better for both mild and severe cases, so even if you don't know the severity you should give them Advil. We've arrived at a contradiction, moving on. Next thought: it is better to look at the separated data because it has "more information". But how could you have known in advance that you should have looked at case severity separately? How do you know there isn't yet another variable? For example, lets further separate the severe cases into fever or no fever:

| Treatment \| Mild case |          Fever           |        No fever        |        Total survival rate         |
| :--------------------: | :----------------------: | :--------------------: | :--------------------------------: |
|         Advil          |  $\frac{5}{10} = 50\%$   | $\frac{85}{90} = 94\%$ |  $\frac{5 + 85}{10 + 90} = 90\%$   |
|         Bleach         | $\frac{306}{365} = 84\%$ | $\frac{34}{35} = 97\%$ | $\frac{306 + 34}{365 + 35} = 85\%$ |

So for both (mild, fever) and (mild, no fever), Bleach is better, but for overall mild cases, Advil is better. I won't bother you with another table, but you can imagine we can do the same for the severe cases. Putting it together, we would have that:

1. Bleach is better than Advil for *overall* COVID cases 
2. Advil is better than Bleach for mild cases *and* severe cases
3. Bleach is better than Advil for *each* of the four combinations: (mild, fever), (mild, no fever), (severe, fever), (severe, no fever)

But this can go ad infinitum, we can keep recursively separating the data like this and alternate the treatment superiority at each level. Clearly something is wrong again, moving on. Final thought: in our original table, most of the severe cases coincided with Advil, and mild cases coincided with bleach. So you might think that I tricked you and that there's no issue: the data lied because Advil was given to severe cases and Bleach to mild cases. So if you were a decision maker you'd give Advil to both mild and severe cases because it performed better in both, right? The causal approach is the right one as we will see, but you might have assumed the wrong causal model. How do you know that severity was a cause of the treatment? In other words, how do you know that people were developing severe cases and were subsequently given Advil? What if it was the other way around: people were given a treatment first, and Advil treated patients were getting severe cases much more often than Bleach? 

### the restoration

The key to answering the question above and solving the paradox lies in causality. We must have a causal model that explains *why* there are more severe cases in the Advil group. Our decision whether to use the aggregate or separated data will depend on whether:

1. Advil causes more severe cases
2. Severe cases call for the usage of Advil

Let's paint a picture of these two very different worlds:

1. A person with a case of COVID enters the hospital and is randomly given either Advil or Bleach. Then the people taking Advil develop severe symptoms and subsequently die a lot more often
2. People with severe cases coming into the hospital are given Advil, and those with mild cases are given Bleach. Even though Advil is better, people that are given it still die a lot more because they are mostly severe cases

Simpson's paradox is tricky because people prescribe some presumably obvious causal mechanism to the data without thinking too hard. For some reason, 2. comes to mind much more easily than 1. But both are equally valid a priori. Only with some amount of external knowledge of the world can we decide between the two.

Of course, reality could be more complicated and that some other variable might be interfering or causing both the breakdown and treatment variable. But the paradox emerges even with one, and that is all we need to showcase its ubiquity and trickiness. Before we look at real world examples of Simpson's paradox, I want to illustrate this theme of jumping to causal conclusions with two fictitious tables. First, imagine we have to choose between two schools, Hogwarts and Durmstrang, and our goal is to maximize GPA. Also, 80% of the Hogwarts classes are hard and 80% of the Durmstrang classes are easy. Here's what a possible breakdown of GPA could look like:

| School \| Class |   Easy    |   Hard    |       Overall GPA        |
| :-------------: | :-------: | :-------: | :----------------------: |
|    Hogwarts     | 3.7 (20%) | 2.3 (80%) | 3.7×0.2 + 2.3×0.8 = 2.58 |
|   Durmstrang    | 3.5 (80%) | 2.1 (20%) | 3.5×0.8 + 2.1×0.2 = 3.22 |

Once again, there are two interpretations here, and if you want to maximize your GPA you must understand which one corresponds to reality:

1. Hogwarts forces you to take hard classes, and Durmstrang easy. Therefore, to maximize GPA, go to Durmstrang, because GPA is higher for easy classes, which you will mostly be taking.
2. You get to choose which classes to take, and for both types, Hogwarts students' GPA is higher. Therefore, go to Hogwarts and choose easy classes. 

Both of these are equally valid, but I have a feeling most people would immediately jump to 2. and not 1, probably because in reality you do get to choose classes. The problem is that this information is not contained within the numbers, and thus we are completely agnostic as to how to use the data. 

Simpson's paradox is so subtle that even people who write articles about it screw up the explanation! Let's look at the explanation that this [article](http://www.hardlyapparent.com/the-counter-intuitive-in-life/simpsons-paradox-in-evaluating-hospitals) gives for the following data that they made up to illustrate the paradox:

| Hospital \| Stage |          Early           |           Late           |         Total survival rate          |
| :---------------: | :----------------------: | :----------------------: | :----------------------------------: |
|         A         | $\frac{370}{500} = 74\%$ | $\frac{250}{500} = 50\%$ | $\frac{370 + 250}{500 + 500} = 62\%$ |
|         B         | $\frac{430}{600} = 72\%$ | $\frac{90}{200} = 45\%$  | $\frac{430 + 90}{600 + 200} = 65\%$  |

Their explanation, *for data that they made up*, is:

> What is happening is that Hospital A gets a larger percentage of late stage patients, and their lower survival rates in general tends to lower the aggregate survival rate of Hospital A, even though it is the better hospital for both late stage and early stage patients.

Even though he completely made up these numbers, his mind immediately jumped to one possible causal interpretation, as if it is reality! Here is an alternative interpretation that I just made up, but is equally valid:

> What is happening is that although people in hospital A survive more in both cancer stages, they need to wait so long to get treated that the cancer develops to a late stage, which is more deadly. 

In this case, does it make sense to say that hospital A is better than hospital B? Of course not. By now, you can probably start to discern the unwarranted conclusions, but Simpson's paradox seems to occur more often than we realize and people are not trained to do this. Let's look at some real world examples.

### covid

I came across two occurrences of this paradox just within COVID data. The first one is COVID death rates in Italy vs. China, showcased by [Kügelgen et al](https://arxiv.org/pdf/2005.07180.pdf). The table is on page 12 of the paper, but I will summarize the finding:

1. Italy's overall death rate is higher than China's
2. For each age group, Italy's death rate is lower than China's

The paradox stems from Italy's preponderance of old people. Could we once again find two different interpretations? Yes, the country can be a consequence of age (old people choose to retire to Italy), or age can be a consequence of country (living in Italy helps you survive to an older age). In the first case, if you imagine yourself as a fully grown human with COVID choosing where to go, you should, of course, go to Italy. But if someone asked you 50 years before the pandemic whether you want to live in Italy or China, you should choose...Italy again. Wait what? Well it depends on what you care about. If you don't want to die from COVID, then go to China, because you are less likely to grow old at all, so you'll die from something else in the mean time. But if you care about not dying, then still go to Italy. Life is complicated! Let's look at another COVID paradox.

The second example is data on COVID deaths by race and age, uncovered by Dana Mackenzie on [Judea Pearl's blog](http://causality.cs.ucla.edu/blog/index.php/2020/07/06/race-covid-mortality-and-simpsons-paradox-by-dana-mackenzie/) (Pearl is the granddaddy of causal inference and has been instrumental in my understanding of this paradox). Unfortunately, the table for all races and age brackets is a bit unwieldy, so I won't reproduce it here, but I will summarize the paradox:

1. White people are more likely to die from COVID than other races combined
2. For each age group, white people are less likely to die from COVID than other races combined

The reason is, once again, that old people are dying from COVID much more, and old people are more likely to be white. Ignoring 1. while focusing solely on 2. is misguided. Let's use the following analogy: men are [less likely to die](https://pubmed.ncbi.nlm.nih.gov/21390560) when deployed to a war zone than women. But there are so many more men fighting than women, that most deaths will be men.  The media's focus on 2. as opposed to 1. is tantamount to mourning women's higher war zone death rates while standing in a graveyard full of men. If you are still not convinced, think of how we partitioned our units of care between Italy and China. Did we focus on the fact that people in China were more likely to die across all ages? No, because it was dwarfed by the sheer number of old people in Italy who were filling morgues. The same principle applies here too.

### [is our children learning?](https://www.youtube.com/watch?v=-ej7ZEnjSeA)

In 2014, the NAEP reported that in twenty years, our eight graders' geography scores have [stagnated](https://www.nationsreportcard.gov/hgc_2014/#geography/scores). Lamentations [abounded](https://www.edweek.org/leadership/eighth-graders-flatline-on-naep-u-s-history-civics-and-geography-tests/2015/04) and screeds were written against our failing educational system. But one brave [Chad](https://aheadoftheheard.org/simpsons-paradox-hides-naep-gains-again/) noticed that Simpson's paradox might be at play here, because each race made [significant gains](https://www.nationsreportcard.gov/hgc_2014/#geography/groups). The reason for the paradox in this case is that the demographics changed significantly in these 20 years. The proportion of Hispanics (with lower scores) grew and the proportion of Whites (with high scores) shrank. The table below shows the score separated by race with the share of the population in parentheses:

| Year \| Race |   White   |   Black   |  Hispanic  | Asian     | Combined |
| :----------: | :-------: | :-------: | :--------: | --------- | -------- |
|     1994     | 269 (72%) | 229 (15%) |  238 (8%)  | 262 (3%)  | 255      |
|     2014     | 273 (50%) | 240 (15%) | 248 (26%)  | 275 (6%)  | 253      |
|      Δ       | +4 (-22%) | +11 (0%)  | +10 (+18%) | +13 (+3%) | -2       |

Note that the other column is missing (from their data) so the percentages don't sum to exactly 100%, but combining the scores shows the paradox. For 1994, the overall score is: 269 * 0.72 + 229 * 0.15 + 238 * 0.08 + 262 * 0.03 ≈ 253, and for 2014 it is: 273 * 0.50 + 240 * 0.15 + 248 * 0.26 + 275 * 0.06 ≈ 255. Thus, if we don't account for demographic change, the overall picture does look bleak, despite huge gains!

### u.s. median wage

The median wage for *every* possible educational group [has fallen](https://economix.blogs.nytimes.com/2013/05/01/can-every-group-be-worse-than-average-yes/) between 2000 and 2013. In other words, no matter your educational level, you became poorer:

|  Period   | Less than HS | High School | HS and some college | Bachelor's or more | Combined |
| :-------: | :----------: | :---------: | :-----------------: | ------------------ | -------- |
| 2000-2013 |    -7.9%     |    -4.7%    |        -7.6%        | -1.2%              | +0.9%    |

But the overall median wage has somehow increased by 0.9%. How is that possible? Because the percentage of people with a college degree increased by a lot, and those people make the most money out of any group. It's not really that we're talking about the same cohort of people in 2000 and 2013, but you can visualize as if some percentage of them went back and got a college degree and got a much better job. This more than offset all the people who did not get more educated and whose wages have fallen. 

So, what is the true picture here? Did wages go down or up? Well, holding education levels constant they fell, but should we hold education levels constant? I'm not sure. Imagine the extreme. Let's say that the wages for both people with and without a bachelor's are falling and imagine that the no college crowd was 90% of the population and made 50k, whereas the college crowd is 10% but makes 100k, with an average wage of 55k. If in 10 years wages fell by 10%, but now everyone has a college degree, then the average is 90k. So did both groups get poorer? Not at all, those people with no college (most of them) increased their wages by a lot and more than offset the college wage decline.

### gender wage gap

When we usually discuss the gender wage gap, it is to point out that the gap shrinks when we compare the salaries for different types of jobs. But interestingly, women could be paid more than men for every type of job, and still men can overall make more money than women. This is exactly what the company Buffer found in their [data](https://docs.google.com/spreadsheets/d/1BeRMXIfOWdkIZUOIauwUcMMpdlbB0lvwxIlLLX7u7Uc/edit#gid=1892754969), a [complete reversal](https://buffer.com/resources/equal-pay/). Women were paid more than men for every type of role within the company, but men were still paid more overall:

| Gender \| Role | Technical | Non-technical | Leadership | Overall |
| :------------: | :-------: | :-----------: | :--------: | ------- |
|      Men       | $102,291  |    $69,783    |  $145,722  | $95,221 |
|     Women      | $103,714  |    $72,284    |  $147,876  | $92,817 |

Although this is just one company, a [wider analysis](https://www.payscale.com/data/gender-pay-gap) does show that if there is any difference in how men and women are paid in different occupations it is within 2%, though it does not usually yield Simpson's paradox. But I don't think it's clear whether we should be looking at this separated data. Clearly the job does not cause you to become a man or a woman, so the arrow of time can only go from gender to job type. Then at least to some extent, gender influences job choice (though the details could take up the whole post). But this is precisely when it is fair to say that there is a gap, because according to the data, to be a woman means to be more likely to go into lower paying occupations. Although it seems hopelessly harder to persuade someone to change careers, at least we know where the disparity lies. 

### stop and frisk

Discussing something controversial like the wage gap can only result in nuanced responses, so let's plow into another heated issue: stop and frisk. This [RAND report](https://www.rand.org/content/dam/rand/pubs/technical_reports/2007/RAND_TR534.pdf) comprehensively analyzes the NYC stop and frisk data, but within minutes of looking at the data I was able to find a clear case of Simpson's paradox in the first two neighborhoods in Table 5.2. Let's say we only have use of force data on white or black civilians in two neighborhoods: Bronx and North Brooklyn. The table contains all races and neighborhoods, but to illustrate the paradox this is enough. The fraction of time that force was used in a stop and frisk encounter is:

| Race  |            Bronx             |       North Brooklyn        |                  Overall                  |
| :---: | :--------------------------: | :-------------------------: | :---------------------------------------: |
| White | $\frac{836}{2758} = 30.3\%$  | $\frac{471}{4705} = 10.0\%$ | $\frac{836 + 471}{2758 + 4705} = 17.5\%$  |
| Black | $\frac{1193}{4045} = 29.5\%$ | $\frac{253}{2391} = 10.6\%$ | $\frac{1193 + 253}{4045 + 2391} = 22.5\%$ |

Within both neighborhoods, force was used on white people about as often as on black people. The overall data, however, seemingly shows grim discrimination by police, with force 30% more likely to be used on black people overall! But this is only because Bronx has more crime (and thus higher use of force) than Brooklyn, and the ratio of black to white people is also higher. Should we control for neighborhood? I think so, since the crime rate is a consequence of neighborhood. 

**tldr**; Simpson's paradox is both poorly understood and ignored by the statistical community, which means lay people are also unaware of it. But it is important to have a good understanding, because it shows up more often than we think, and its implications are wide reaching.