---
title: "Computing inter-rater aggrement scores using Python"
author: "Pankaj Chejara"
date: "2023-09-21"
categories: [python]
image: "./images/agreement/hands.jpg"
code-block-background: true
highlight-style: "arrow"
toc: true
---

<p>Recently, I was involved in some annotation processes involving two coders and I  needed to compute inter-rater reliability scores. There are multiple measures for calculating the agreement between two or more than two coders/annotators. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you have a question regarding ""which measure to use in your case?"", I would suggest reading (Hayes &amp; Krippendorff, 2007) which compares different measures and provides suggestions on which to use when.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> In this post, I am sharing some of our python code on calculating various measures for inter-rater reliability.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Cohen's Kappa</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We will start with Cohen's kappa.  Let's say we have two coders who have coded a particular phenomenon and assigned some code for 10 instances. Now let's write the python code to compute cohen's kappa.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You can use either  <code>sklearn.metrics</code> or <code>nltk.agreement</code> to compute kappa. We will see examples using both of these packages.</p>

```python
from sklearn.metrics import cohen_kappa_score

coder1 = [1,0,2,0,1,1,2,0,1,1]
coder2 = [1,1,0,0,1,1,2,1,1,0]
score = cohen_kappa_score(coder1,coder2)

print('Cohen\'s Kappa:',score)

# output
# Cohen's Kappa: 0.3220338983050848
```

<!-- wp:paragraph -->
<p>In order to use <code>nltk.agreement</code> package, we need to structure our coding data into a format of <code>[coder, instance, code]</code>. For instance, the first code in coder1 is 1 which will be formatted as <code>[1,1,1]</code> which means coder1 assigned <code>1</code> to the first instance. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Let's convert our codes given in the above example in the format of <code>[coder,instance,code]</code>. Here we have two options to do that. I have included the first option for better understanding. Second option is a short one line solution to our problem.</p>

```python
coder1 = [1,0,2,0,1,1,2,0,1,1]

coder1_new = []
coder2_new = []
for i in range(len(coder1)):
    coder1_new.append([1,i,coder1[i]])
    coder2_new.append([2,i,coder2[i]])


formatted_codes = coder1_new + coder2_new
print(formatted_codes)
```

<pre class=""wp-block-code""><code>[[1, 0, 1], [1, 1, 0], [1, 2, 2], [1, 3, 0], [1, 4, 1], [1, 5, 1], [1, 6, 2], [1, 7, 0], [1, 8, 1], [1, 9, 1], [2, 0, 1], [2, 1, 1], [2, 2, 0], [2, 3, 0], [2, 4, 1], [2, 5, 1], [2, 6, 2], [2, 7, 1], [2, 8, 1], [2, 9, 0]]</code></pre>

```python
formatted_codes = [[1,i,coder1[i]] for i in range(len(coder1))] + [[2,i,coder2[i]] for i in range(len(coder2))] 
print(formatted_codes)
```
<!-- /wp:prismatic/blocks -->

<!-- wp:code -->
<pre class=""wp-block-code""><code>[[1, 0, 1], [1, 1, 0], [1, 2, 2], [1, 3, 0], [1, 4, 1], [1, 5, 1], [1, 6, 2], [1, 7, 0], [1, 8, 1], [1, 9, 1], [2, 0, 1], [2, 1, 1], [2, 2, 0], [2, 3, 0], [2, 4, 1], [2, 5, 1], [2, 6, 2], [2, 7, 1], [2, 8, 1], [2, 9, 0]]</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Now, we have our codes in the required format, we can compute cohen's kappa using <code>nltk.agreement</code>.</p>
<!-- /wp:paragraph -->

```python
from nltk import agreement

ratingtask = agreement.AnnotationTask(data=formatted_codes)

print('Cohen\'s Kappa:',ratingtask.kappa())
```

<!-- /wp:prismatic/blocks -->

<!-- wp:code -->
<pre class=""wp-block-code""><code>Cohen's Kappa: 0.32203389830508466</code></pre>
<!-- /wp:code -->

<!-- wp:heading {""level"":4} -->
<h4>Cohen's Kappa using CSV files</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In this section, we will see how to compute cohen's kappa from codes stored in CSV files. So let's say we have two files (coder1.csv, coder2.csv). Each of these files has some columns representing a dimension. Below is the snapshot of such a file. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The files contain 10 columns each representing a dimension coded by first coder. We have a similar file for coder2 and now we want to calculate Cohen's kappa for each of such dimensions.</p>
<!-- /wp:paragraph -->

| SMU | CF | KE | ARG | STR | CO | u1 | u2 | u3 | u4 |
|-----|----|----|-----|-----|----|----|----|----|----|
| 1   | 1  | 1  | 0   | 0   | 1  | 2  | 1  | 1  | 0  |
| 1   | 1  | 1  | 0   | 0   | 2  | 1  | 2  | 1  | 1  |
| 2   | 2  | 1  | -1  | 0   | 1  | 2  | 1  | 2  | 2  |
| 2   | 2  | 1  | 1   | 0   | 2  | 2  | 2  | 2  | 1  |
| 1   | 2  | 1  | 1   | 0   | 2  | 2  | 2  | 1  | 2  |
| 1   | 1  | 1  | 1   | 0   | 1  | 2  | 2  | 1  | 2  |
| 2   | 1  | 1  | 1   | 0   | 2  | 2  | 2  | 1  | 2  |
| 2   | 1  | 2  | 2   | 0   | 2  | 1  | 2  | 2  | 2  |
| 2   | 2  | 2  | 2   | 0   | 2  | 2  | 2  | 2  | 2  |

<!-- wp:paragraph -->
<p>We will use <code>pandas</code> python package to load our CSV file and access each dimension code ([Learn basics of Pandas Library]()). </p>
<!-- /wp:paragraph -->

<!-- wp:prismatic/blocks {""language"":""python""} -->
```python
import pandas as pd
from sklearn.metrics import cohen_kappa_score

coder1 = pd.read_csv('coder1.csv')
coder2 = pd.read_csv('coder2.csv')

dimensions = coder1.columns

#iterate for each dimension
for dim in dimensions:
   
    dim_codes1 = coder1[dim]
    
    dim_codes2 = coder2[dim]
    print('Dimension:',dim)
    
    score = cohen_kappa_score(dim_codes1,dim_codes2)
    
    print(' ',score)
```

<!-- wp:code -->
<pre>Dimension: SMU
  0.3076923076923077
Dimension: CF
  0.55
Dimension: KE
  0.12903225806451613
Dimension: ARG
  0.6896551724137931
Dimension: STR
  0.0
Dimension: CO
  -0.19999999999999996
Dimension: u1
  0.0
Dimension: u2
  0.0
Dimension: u3
  0.3414634146341463
Dimension: u4
  0.4375</pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2>Fleiss's Kappa</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>As per my understanding, Cohen's Kappa can be used if you have codes from only two coders. In case, if you have codes from multiple coders then you need to use Fleiss's kappa.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>We will use <code>nltk.agreement</code> package for calculating Fleiss's Kappa. So now we add one more coder's data to our previous example.</p>
<!-- /wp:paragraph -->

<!-- wp:prismatic/blocks {""language"":""python""} -->
```python
from nltk import agreement

coder1 = [1,0,2,0,1,1,2,0,1,1]
coder2 = [1,1,0,0,1,1,2,1,1,0]
coder3 = [1,2,2,1,2,1,2,1,1,0]

formatted_codes = [[1,i,coder1[i]] for i in range(len(coder1))] + [[2,i,coder2[i]] for i in range(len(coder2))]  + [[3,i,coder3[i]] for i in range(len(coder3))]


ratingtask = agreement.AnnotationTask(data=formatted_codes)

print('Fleiss\'s Kappa:',ratingtask.multi_kappa())
```
<!-- /wp:prismatic/blocks -->

<!-- wp:code -->
<pre class=""wp-block-code""><code>Fleiss's Kappa: 0.3010752688172044</code></pre>
<!-- /wp:code -->

<!-- wp:heading {""level"":4} -->
<h4>Fleiss's Kappa using CSV files</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Now, let's say we have three CSV files, one from each coder. Each coder assigned codes on ten dimensions (as shown in the above example of CSV file).  The following code compute Fleiss's kappa among three coders for each dimension.</p>
<!-- /wp:paragraph -->

<!-- wp:prismatic/blocks {""language"":""python""} -->
```python
import pandas as pd
from nltk import agreement


coder1 = pd.read_csv('coder1.csv')
coder2 = pd.read_csv('coder2.csv')
coder3 = pd.read_csv('coder3.csv')

dimensions = coder1.columns


for dim in dimensions:
   
    dim_codes1 = coder1[dim]
    dim_codes2 = coder2[dim]
    dim_codes3 = coder3[dim]
    
    formatted_codes = [[1,i,dim_codes1[i]] for i in range(len(dim_codes1))] + [[2,i,dim_codes2[i]] for i in range(len(dim_codes2))]  + [[3,i,dim_codes3[i]] for i in range(len(dim_codes3))]
    
    ratingtask = agreement.AnnotationTask(data=formatted_codes)
    print('Dimension:')
    print(' Fleiss\'s Kappa:',ratingtask.multi_kappa())
```
<!-- /wp:prismatic/blocks -->

<!-- wp:heading -->
<h2>Cronbach's Alpha</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Cronbach's alpha is mostly used to measure the internal consistency of a survey or questionnaire. For this measure, I am using <code>Pingouin</code> package (<a href=""https://pingouin-stats.org/index.html"">link</a>). </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Let's say we have data from a questionnaire (which has questions with Likert scale) in a CSV file. For example, I am using a dataset from <code>Pingouin</code> with some missing values.</p>
<!-- /wp:paragraph -->

<!-- wp:prismatic/blocks -->
```python
import pingouin as pg

data = pg.read_dataset('cronbach_wide_missing')

data.head()
```
<!-- /wp:prismatic/blocks -->

<!-- wp:code -->
<pre class=""wp-block-code""><code>Q1,Q2,Q3,Q4,Q5,Q6,Q7,Q8,Q9,Q10,Q11
1.0,1,1.0,1,1.0,1,1,1,1.0,1,1
1.0,1,1.0,1,1.0,1,1,1,0.0,1,0
,0,1.0,1,,1,1,1,1.0,0,0
1.0,1,1.0,0,1.0,1,0,1,1.0,0,0
1.0,1,1.0,1,1.0,0,0,0,1.0,0,0
0.0,1,,0,1.0,1,1,1,0.0,0,0
1.0,1,1.0,1,0.0,0,1,0,0.0,0,0
1.0,1,1.0,1,1.0,0,0,0,0.0,0,0
0.0,1,0.0,1,1.0,0,0,0,0.0,1,0
1.0,0,0.0,1,0.0,1,0,0,,0,0
1.0,1,1.0,0,0.0,0,0,0,0.0,0,0
1.0,0,0.0,1,0.0,0,0,0,0.0,0,0</code></pre>
<!-- /wp:code -->

<!-- wp:prismatic/blocks {""language"":""python""} -->
<pre class=""wp-block-prismatic-blocks""><code class=""language-python"">pg.cronbach_alpha(data=data)</code></pre>
<!-- /wp:prismatic/blocks -->

<!-- wp:code -->
<pre class=""wp-block-code""><code>(0.732661, array([0.435, 0.909]))</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2>Krippendorff's Alpha &amp; Scott's Pi</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We can use <code>nltk.agreement</code>  python package for both of these measures. I will show you an example of that.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For <code>nltk.agreement</code>, we need our formatted data (what we did in the previous example?). Once we have our formatted data, we simply need to call <code>alpha</code> function to get the Krippendorff's Alpha. Let's see the python code.</p>
<!-- /wp:paragraph -->

<!-- wp:prismatic/blocks {""language"":""python""} -->
```python
from nltk import agreement

coder1 = [1,0,2,0,1,1,2,0,1,1]
coder2 = [1,1,0,0,1,1,2,1,1,0]
coder3 = [1,2,2,1,2,1,2,1,1,0]

formatted_codes = [[1,i,coder1[i]] for i in range(len(coder1))] + [[2,i,coder2[i]] for i in range(len(coder2))]  + [[3,i,coder3[i]] for i in range(len(coder3))]


ratingtask = agreement.AnnotationTask(data=formatted_codes)

print('Krippendorff\'s alpha:',ratingtask.alpha())
print('Scott\'s pi:',ratingtask.pi())
```
<!-- /wp:prismatic/blocks -->

<!-- wp:code -->
<pre class=""wp-block-code""><code>Krippendorff's alpha: 0.30952380952380953
Scott's pi: 0.2857142857142859</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2>Inter-class correlation</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>I am using <code>Pingouin</code> package mentioned before as well. The function used is <code>intraclass_corr</code>. This function returns a <code>Pandas</code> Datafame having the following information (from R package [psych](https://www.rdocumentation.org/packages/psych/versions/1.9.12.31/topics/ICC"">documentation)). Six cases are returned (ICC1, ICC2, ICC3, ICC1k, ICCk2, ICCk3) by the function and the following are the meaning for each case.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Shrout and Fleiss (1979) consider six cases of reliability of ratings done by k raters on n targets.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
* ICC1: Each  target is rated by a different  judge and the judges are 
selected at random.  (This is a one-way ANOVA fixed effects model and is
 found by  (MSB- MSW)/(MSB+ (nr-1)*MSW))</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
* ICC2: A random sample of k judges rate each target.  The measure is 
one of absolute agreement in the ratings. Found as (MSB- MSE)/(MSB + 
(nr-1)*MSE + nr*(MSJ-MSE)/nc)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
* ICC3: A fixed set of k judges rate each target. There is no 
generalization to a larger population of judges.  (MSB - MSE)/(MSB+ 
(nr-1)*MSE)
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Then, for each of these cases, there are second variant (e.g., ICC1k). The difference between the two variants of classes is that in the first case, the 1 rating is 
equivalent to the average intercorrelation, while, the k rating case uses 
Spearman Brown adjusted reliability.)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>ICC1 is sensitive to differences in means between raters and is a measure of absolute agreement.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>ICC2 and ICC3 remove mean differences between judges, but are 
sensitive to interactions of raters by judges.  The difference between 
ICC2 and ICC3 is whether raters are seen as fixed or random effects.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>ICC1k, ICC2k, ICC3K reflect the means of k raters.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The dataset from <code>Pingouin</code> has been used in the following example.</p>
<!-- /wp:paragraph -->

```python
import pingouin as pg

data = pg.read_dataset('icc')

icc = pg.intraclass_corr(data=data, targets='Wine', raters='Judge',ratings='Scores')
icc
```
<!-- /wp:prismatic/blocks -->

<!-- wp:code -->
<pre class=""wp-block-code""><code> 	Type 	Description 	ICC 	F 	df1 	df2 	pval 	CI95%
0 	ICC1 	Single raters absolute 	0.728 	11.680 	7 	24 	0.000002 	[0.43, 0.93]
1 	ICC2 	Single random raters 	0.728 	11.788 	7 	21 	0.000005 	[0.43, 0.93]
2 	ICC3 	Single fixed raters 	0.730 	11.788 	7 	21 	0.000005 	[0.43, 0.93]
3 	ICC1k 	Average raters absolute 	0.914 	11.680 	7 	24 	0.000002 	[0.75, 0.98]
4 	ICC2k 	Average random raters 	0.914 	11.788 	7 	21 	0.000005 	[0.75, 0.98]
5 	ICC3k 	Average fixed raters 	0.915 	11.788 	7 	21 	0.000005 	[0.75, 0.98]</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2>References</h2>
<!-- /wp:heading -->

<!-- wp:list {""ordered"":true} -->
<ol><li>Hayes, A. F., &amp; Krippendorff, K. (2007). Answering the Call for a Standard Reliability Measure for Coding Data. <em>Communication Methods and Measures</em>, <em>1</em>(1), 77–89. https://doi.org/10.1080/19312450709336664</li></ol>

* <a href="https://www.freepik.com/free-photo/hands-businessman-businesswoman-discussing-agreement_2448652.htm#query=agreement&position=9&from_view=search&track=sph&uuid=5f5f3c11-39ad-4d61-b154-408962962020">Image by katemangostar</a> on Freepik
<!-- /wp:list -->"