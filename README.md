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
 
