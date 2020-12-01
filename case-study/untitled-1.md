---
description: Metrics
---

# Causes for drop

## How would you find the cause of a 15% drop in Facebook Groups usage?

* **Clarifying questions:** 
  * **Metric:** What do you mean by groups usage, and how do you measure it? 
    * Engagement?
    * Group creation?
  * If engagement: how to measure?
    * Avg time people spent on group
    * Avg number of posts
    * Avg reaction to the posts?
  * If the total count for all activities, as called the total engagement
* **What is the business goal?** 
* **What is the success metric?** 
* **High level framework:**
  * **The cause might be resulted from** 
    * **Internal:** user behaviour, as evidenced \# by posts, replies, share drop 
    * **External-Product / Competitor effect** users shift to other features or other products 
  * To diagnose which one is the cause, I will start by asking the questions to check if we really need to care about the effect.  
    * **Context: Should we care about the drop?** 
      * **Frequency:** One-time drop or consistent drop?
        * If one-time event 
          * Technical glitches 
          * system down service 
        * If consistent
          * Dive into it 
      * **Duration:**Is this drop happens only for this week or month, or seasonally? 
        * Seasonally
          * Holiday season issue
          * Post-promotion or special event 
          * Cohort analysis to check the historical data, and compare the YoY and MoM change. 
      * **Region:** Did the drop only happen in one region? 
        * If one region
          * Policy regulation 
          * Market competitor 
            * If that is the case, we need some ethnographic analysis to understand the market and develop the feature targeted at their needs. 
      * **Platform:** Did the drop only happen in particular platform, like IOS, android, or other?
        * If happen in one particular platform
          * compare the engagement between platforms
          * Potential UI/UX or the user experience in certain platform is different?
      * **Across features:** Did the drop only happen in only group or other feature as well?
        * If all other feature: Big problem of the product, and might potentially competitor effect-need to dive into it and see overall engagement across the product. 
        * If only one feature: Dive into this feature
      * **Does it happen in competitor products as well?** 
        * If we can get the data
          * If so, might be government regulation, or PR reason that is out of DS team, but for corporate management team 
  * **Deep Dive**
    * If we cross check all the previous factors, nothing happens, then we can dive into the metric. 
    * **Internal User behaviors**
      * Break down to the feature level 
        * Are all features function well?
        * Did people experience long time posting or sharing?
        * Did UI work for replying, posting, sharing for FB group?
          * If the UI happens, then people will report to Q&A or technical team, then it should be caught immediately and not causing the consistent drop. 
        * **Did users experience more spam content in the group?**
          * Spam could cause people to leave the group
          * It might cause consistent drop 
          * If the spam rate is increasing in the group, then it should be investigated further, and figure out the strategy on identifying those spams. 
    * **External: Competitor / Alternative Feature**
      * Is there a new feature similar to group?
        * If so, then we could investigate two problems
          * 1\) **Shared users proportion:** What is the proportion of users of the new feature that are also users of Facebook Groups? And, 2\) **Percentage Switch:** What is the percentage of users that switched that have exhibited a significant decline in engagement with Facebook Groups since the problem started?
          * If there is a large group of people switch, it might not necessarily a bag thing, the new feature might bring more revenue, so this needs to be checked with the stakeholders. 
      * **Is there a similar product from the competitor?**
        * If so, then there needs more market research on how the competitor did differently, figure out the features that attract the users, and borrow the ideas. 
        * Then we could use A/B test to test the new features.
  * Summary 
    * Check the possible reasons
    * Internal and external factors 
      * Key hypothesis for the consistent drop 
        * Spam posts 
        * Alternative feature 
        * Competitor product 

{% embed url="https://www.thedsinterview.com/challenges/facebook" %}

[https://www.productmanagementexercises.com/996/how-would-you-measure-the-success-of-facebook-likes](https://www.productmanagementexercises.com/996/how-would-you-measure-the-success-of-facebook-likes)

