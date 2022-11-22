# AB_Testing_Udacity_Free-trial-screener
 
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
 - Net conversion: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin=0.01) This matric could help to evaluate whether the new feature would reduce the number of students to continue past the free trial and eventually complete the course. It is expected to remain show similar distribution in both control and experiemnt groups if the alternative hypothesis is true. 

**Unused Metrics**
 - Number of user-ids: That is, number of users who enroll in the free trial. User-ids are tracked only after enrolling in the free trial and equal distribution between the control and experimental branches would not be expected. However, user-id count could be used to evaluate how many enrollments stayed beyond the 14 day free trial boundary. Thus, it's not chosen as the metrics. 


