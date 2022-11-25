# AB_Testing_Udacity_Free-trial-screener
----------------------------------------
 
**Project Instruction**: https://docs.google.com/document/u/1/d/1aCquhIqsUApgsxQ8-SQBAigFDcfWVVohLEXcV6jWbdI/pub

## Experiment Overview: Free Trial Screener

At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.
![This is an image](https://github.com/hxycorn/AB_Testing_Udacity_Free-trial-screener/blob/90e7096c7385ea0108d0202c8c6f5eae18b19b95/Final%20Project_%20Experiment%20Screenshot.png)

The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.

The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

## Experiment Design
### Hypothesis
- **Null Hypothesis**: This free-trial-screener approach will not generate a significant change and might not be effective in reducing the early Udacity course cancellation.
- **Alternative Hypothesis**: This free-trial-screener approach will reduce the number of frustrated students who left the free trial because they didn't have enough time, without significantly reducing the number of students to continue past the free trial and eventually complete the course.

### Metric Choice
**Invariant Metrics**

In the given experiment, the following matrics are chosen as the invariant metrics which we could expect a similar distribution in both control and experiment groups. 
 - Number of cookies: That is, number of unique cookies to view the course overview page. This is the unit of diversion and even distribution amongst the control and experiment groups is expected.
 - Number of clicks: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). Equal distribution amongst the experiment and control groups would be expected since at this point in the funnel the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "start free trial" button.
 - Click-through-probability: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. Till the time the user clicks the "start free trial" button the user experience is same for all the users. Hence, we expect equal distribution in both the groups.
  
**Evaluation Metrics**

Evaluation metrics are chosen since there is a possibility of different distribution between experiment and control groups as a function of experiment. Each evaluation metric is associated with a minimum difference (dmin) that must be observed for consideration in the decision to launch the experiment. The ultimate goal is to minimize student frustation and use the limited coaching resources most efficiently. With this in mind, the following conditions must be satisfied -
 - Gross conversion: That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01) **Decrease gross conversion** is expected if the screener is effective, as less students would enroll in the free trial due to the reminder and would choose to access the course materials for free instead. 
 - Retention: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01) **Increase retention** is expected if the screener is effective. Since the function is aims to filter out those students who tend to cancel the course early, which means the total number of user-ids to complete checkout would decrease. Therefore the ratio of users who remained enrolled past the 14-day boundary to the number of users to complete checkout should increase.
 - Net conversion: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin=0.0075) This matric could help to evaluate whether the new feature would reduce the number of students to continue past the free trial and eventually complete the course. It is expected to remain show similar distribution in both control and experiemnt groups if the alternative hypothesis is true. 

**Unused Metrics**
 - Number of user-ids: That is, number of users who enroll in the free trial. User-ids are tracked only after enrolling in the free trial and equal distribution between the control and experimental branches would not be expected. However, user-id count could be used to evaluate how many enrollments stayed beyond the 14 day free trial boundary. Thus, it's not chosen as the metrics. 

### Measuring Standard Deviation
For each of the evaluation metrics, the analytical estimates of standard deviation is calculated for a sample size of 5000 unique cookies visiting the course overview page. The standard deviation are calculated using the [Baseline_Values](Data/Baseline_Values.csv).

**_Analytical Estimates of Standard Deviation of Evaluation Metrics_**

|Evaluation Metric|Standard Deviation|
|:---:|:---:|
|Gross conversion|0.0202|
|Retention|0.0549|
|Net conversion|0.0156|

### Sizing
The following calculation is based on the [Baseline_Values](Data/Baseline_Values.csv).

**Number of Samples vs Power**

The alpha vaue of 0.05 and beta value of 0.2 is used in all the cases. The total number of pageviews needed to achieve enough statistical power for each evaluation metrics in the experiments is calculated separately using the [online calculator](https://www.evanmiller.org/ab-testing/sample-size.html).

**_Pageviews Required for Each Evaluation Metrics_**
| |Gross Conversion|Retention|Net Conversion|
|:---|:---:|:---:|:---:|
|Baseline conversion rate|20.625%|53.0%|10.9313%|
|Minimum Detectable Effect|1%|1%|0.75%|
|Significance level α|5%|5%|5%|
|Statistical power 1−β|80%|80%|80%|
|Sample size/group|25,835|39,115|27,413|
|Number of Group|2|2|2|
|Total sample size|51,670|78,230|54,826|
|Click or Enrollment Rate|0.08|0.0165|0.08|
|Total Pageviews|645,875|4,741,212|685,325|

The required pageviews to achieve the stastical power is the maximum total pageviws values among all the evaluation metrics. Therefore, the pageview numbers should be 4,741,212 based on the above calculation.

## Experiment Analysis
All the analysis is based on the following experiment data: [Control Group Data](Data/Project_Results_Control.csv) and [Experiment Group Data](Data/Project_Results_Experiment.csv).

### Sanity Checks
The sanity checks is primarily for the invariant metrics that discussed above, which is **Number of cookies**, **Number of clicks**, and **Click-through-probability**. For the invariant metrics, equal diversion for both control and experiment groups is expected. Therefore, all invariant metrics are tested given the 95% confidence interval. The sanity check results are listed in the table below. 

|Metric|Expected|Observed(Control)|Observed(Experiment)|CI Lower_Bound|CI Upper Bound|Result|
|:---|:---:|:---:|:---:|:---:|:---:|:---:|
|Number of cookies|0.5000|0.5006|0.4994|0.4988|0.5012|Pass|
|Number of clicks|0.5000|0.5005|0.4995|0.4959|0.5042|Pass|
|Click-through-probability|0.08213|0.08213|0.08218|0.08121|0.08304|Pass|

All invariant metrics passed the sanity checks. 

### Result Analysis
 -**Practical and Statistical Significance**
 
 The evaluation metrics of control and experiment groups were compared given a 95% confidence interval. Bonferroni correction is not used here. Calculated results are shown in the following table.
 |Metric|dmin|Difference observed|CI Lower_Bound|CI Upper Bound|Result|
 |:---|:---:|:---:|:---:|:---:|:---:|
 |Gross Conversion|0.01|-0.0205|-0.0291|-0.0120|statistically and practically significant|
 |Net Conversion|0.0075|-0.0048|-0.0116|0.0019|Neither statistically nor practically significant|
 
 -For gross conversion, the confidence interval doesn't include 0 and include the practical boundary (0.01), thus it's statistically and practically important. 
 
 -For net conversion, the confidence interval does include 0 and dosen't include the practical boundary (0.0075), thus it's neither statistically nor practically important.
 
 -**Sign Tests**
 
 A sign test is another method to validate the result obtained above. We check if the trend of change we observed (decrease of gross conversion) was evident in the daily data. We are going to compute the metric's value per day and then count on how many days the metric was lower in the experiment group and this will be the number of success for the binomial variable. We then check the proportion of days of success out of all the available days.  
 Baesd on the "success" number for each metrics, the p-value could be calculated using the [online sign and binomial test calculator](https://www.graphpad.com/quickcalcs/binomial2/).
 |Metric|Success Number|Trial Number|p-value|Statistically important @ α=0.05?|
 |:---|:---:|:---:|:---:|:---:|
 |Gross Conversion|19|23|0.0026|Yes|
 |Net Conversion|13|23|0.6776|No|

 
 ## Summary
 The purpose of conducting this experiment was to test if the free trial screener could improve learning experience and completion rates. Udacity students were diverted by cookies into experiment and control group. The students in the experiment group will be directed to ask their devoted time to the course after clicking a "start free trial button", and suggested to enroll or use the free course materials while the control group will see the regular enrollment button or use the free material in the homepage.

 We selected three invariant metrics (Number of Cookies, Number of clicks on "start free trial", and Click-Through-Probability)as our metrics for validation and sanity check. The evaluation metrics we selected are Gross Conversion (enrollment/cookies) and Net Conversion (payments/cookies). The null hypothesis is that there is no difference in the evaluation metrics between the groups, futhermore, a practical signifcance threshold was set for each metric. There are two requirements to lauch the feature: 1. The null hypothesis is rejected for both evaluation metircs, meaning there is no difference in the evaluation metrics between the groups. 2. A practical signifcance threshold was set for each metric should be met, meaning the minimun difference should exceed the practical signficance threshold.
 
 We decided not to use the Bonferonni correction, given our acceptance criteria requires statiscally signifcant differences for ALL evaluation metrics, and so the use of the Bonferonni correction is not appropriate. The Bonferonni correction is a method for controlling for type I errors (false positives) when using multiple metrics in which relevance of ANY of the metrics matches the hypothesis. In this case the risk of type I errors increases as the number of metrics increases (signifcance by random chance). In our case in which ALL metrics must be relevant to launch, the risk of type II errors (false negatives) increases as the number of metrics increases, so it stands to reason that controlling for false positives is not consistent with our acceptance criteria.

The results revealed the equal distribution of cookies into the control and experimental groups for the invariant metrics, at the 95% CI. A difference in gross conversion was found to be statistically signficant at the 95% CI and was practically significant, and thus the null hypothesis was rejected. However, the Net Conversion was found to be neither statistically nor practically signficant at the 95% CI.  

## Recommendation
We expect filtering students as a function of study time commitment would improve the overall student experience and the coaches' capacity to support students who are likely to complete the course, without significantly reducing the number of students who continue past the free trial. A statistically and practically signficant decrease in Gross Conversion was observed but with no significant differences in Net Conversion. This translates to a decrease in enrollment not coupled to an increase in students staying for the requisite 14 days to trigger payment. Given these two reason, I suggest not launching the experiment but to pursue other experiments.


## Follow-up Experiment
With the goal of reduce early cancellation, there are a variety of strategies could be taken for intervention. 

The first timepoint for intervention is pre-enrollment. We could try to take strategies to filter out students who are likely to become frustrated and quit early. The above experiment was focused only on time commiment to the course and did not consider other reasons. Actually, students may also drop the course due to other reasons, for example: lack of pre-requisite skills/experience. Therefore, we could design an experiment to add a popup checklist of pre-requisite skills after they click on the "start free trial". This experiment would be similar to the time commitment poll, but it may increase the selectivity of the pre-enrollment filter. A succesful experiment would be one in which there is a signficant decrease in Gross Conversion coupled to a significant increase in Net Conversion.

The second timepoint for interverntion is post-enrollment but pre-payment. One factor that could affect students performance is the price of the course. Some students may think the price of the course is too high and choose to drop the course before the free trial ends. In this case, I propose we design an experiment to provide students whom chose to cancel the course a discount on the course. The experiment would function in the following manner. 

**Setup**: Upon the student clicks on the "cancel" button, they wound be asked to choose a reason for their cancellation, if they chose "the price is too expensive", then they would be offered 15% off promo code for the course. 

**Null Hypothesis**: The promo code for the course will not increase the students enrolled beyond the 14 day free trial period by a significant amount. 

**Alternative Hypothesis**: The promo code will significantly increase the number of students enrolled beyond the 14 day free trial period by a significant amount and result in an increase in the total revenue. 

**Unit of Diversions**: The unit of diversion will be user-id as the change takes place after a student creates an account and enrolls in a course.

**Invariant Metrics**: The invariant metric will be user-id, since an equal distribution between experiment and control would be expected as a property of the setup.

**Evaluation Metrics**: Retention (number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout) and Total Revenue (number of user-ids paid at original price * original price + number of user-ids paid at discounted price * discounted price). If a statistically and practically signifcant positive change in Retention and Total Revenue is observed, the experiment will be launched.
