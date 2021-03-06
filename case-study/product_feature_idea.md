---
description: Product_feature
---

# Product\_Feature\_Idea

### FB would like to change the UI of the composer \(the one users use to write a post\) to be like INSTAGRAM \(instead of a box ,they would they will add a “+” button at the bottom of the page\), how would you test if this is a good idea?





## How do you decide between displaying Facebook’s ‘People You May Know’ feature or an Advertisement? <a id="1671"></a>



Imagine you are the product manager of Facebook News Feed and have to instruct your team on how to decide whether to display the ‘People You May Know feature’ \(PYMK\) feature or an advertisement. How do you choose? What are the trade-offs?

{% hint style="info" %}
[https://medium.com/stellarpeers/how-do-you-decide-between-displaying-facebooks-people-you-may-know-feature-or-an-advertisement-f90aaf0ba47e](https://medium.com/stellarpeers/how-do-you-decide-between-displaying-facebooks-people-you-may-know-feature-or-an-advertisement-f90aaf0ba47e)
{% endhint %}

* **Goal of the product?** 
  * **NewsFeed:** Increase the engagement of users and generate more user generated content
  * Clarify if the two features display in the regular posts which might impact user engagement. 
  * What is the business goal or impact we would like to achieve?
    * Find a common metric and test the impact against it
    * Because different products may serve for different user segment and goal 
  * **Find a common metrics for success** 
    * Feature A: 'People you may know feature' 
    * Feature B: 'advertisement'-assumption: increase the ad revenue 
    * Define the product goal for A, B and metric of success for A, B respectively 
      * Feature A
        * **General goal:** More users acquired, and more time spent on feature A while using the product, and more ads they will see--&gt; revenue
        * **Different impact for new and old users:** might only work for the new users rather than old users because old users might be very active with current friends 
      * Feature B: 
        * **Target users and goals:** 
          * **1\) advertisers:** promote products to interest users
          * **2\) End-user: help users buy what they are interested at** 
          * The business goal is to increase **ad revenue.** 
          * Breakdown the ad revenue: what generate values for FB?
            *  **Number of views per ad or clicks per ad**
      * **Common metric:** Time spent on FB ---&gt;  the number of ads a user sees---&gt; **ad revenue** \(common metrics\)
* **What is the metrics for measure ad revenue?** 
  *  \#impressions or \# of clicks ---&gt; for two features---&gt; **THE AVERAGE revenue gained from all ad in certain period of time** 
* **How to test?**
  * **Set an A/B testing** 
  * **Setting Hypothesis \(Launch A or B or not launch\)**
    * **If need to consider A B or null, then we need to design two pair of test: A vs null, and B vs null.** 
      * **Test two hypothesis**
        * H01: Avg/total revenue from feature A has no difference from the null group  \(one-tail\)
        * H11: Avg/total revenue from feature A is greater than  from the null group 
        * H02: Avg/total revenue from feature B has no difference from the null group  \(one-tail\)
        * H12: Avg/total revenue from feature B is greater than  from the null group 
  * **Experiment setting** 
    * Unit of diversion: user 
    * Unit of analysis: ads 
    * Sample: Randomized samples - Block
      * Consider the least segment for randomization given the cost and time for A/B testing 
        * Users: old / new 
        * Region
        * Age 
        * active time \(morning, afternoon, night\)
    * **\*\*Size** 
      * Calculate n: statistical power, significance level \(alpha\), practical significance level \(effect size\), confidence level
    * Duration of experiment
      * Check with the engineer and data scientist on the daily volume of the users --&gt; check how much volume we can to reach the sample size
* **Result interpretation**
  * If feature A &gt; control, feature B &gt; control
    * Launch A vs B test \(check the significance\)
    * **\(What if the difference in revenue lift is minimal between the two features? How sure are you that this small difference is not just due to randomness?\)** 
    * **If any weight between two features? By decision rules:** only feature B bring at least X more dollars than the feature A, we will decide launch it, otherwise, no bother. 
  * If feature A &gt; control, feature B &lt; control, verse versa
    * Launch A or Launch B 
  * If feature &lt; control, feature &lt; control
    * Not launch any feature or further experiment  
* **Summary**

![](../.gitbook/assets/image%20%281%29.png)



