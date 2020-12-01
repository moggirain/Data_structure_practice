---
description: Health
---

# Invalid High School

###  **How many high schools that people have listed on their profiles are real? How do we find out, and deploy at scale, a way of finding invalid schools?**

**Clarifying questions:** 

**1\) What is the goal for finding the unreal high school profiles?** 

**2\) Why high school?** 

**3\) What is the success metrics?**

**4\)What is the current infrastructure or system to identify those fake accounts?** 

* **Question: translate to if people list fake high schools**
  * If yes, what is the purpose of listing the fake high schools?
    * Possibility1: Protect the privacy information 
    * **Possibility2: Bad actors want to create multiple fake accounts for scam or fraud purpose** 
  * **Possibility2 should be the main concern here for community health** 
  * **Identify the fake high school name. E.g., “Garbish High School”.** 
    * **Hypothesis1: Meaningful name but does not exist e.g., MountainEagle High School** 
      * High school name does not list on the public education data system 
      * How to detect?
        * Utilize the current public government education data to get the high school names \(trustworthy database\)
        * If it can not be retrieved from the database, then it is probably a potential fake high school 
      * Edge case: The database is limited, and cover only a small amount of high school names \(See hypothesis 2\)
    * **Hypothesis2: My friends or similar users should go to the same school.** 
      * If similar users or my friends \(I followed or added\) have no one go the school I listed, or not one listed a high school name, it is likely a fake high school
      * How to detect?
        * Data needed: Friends profile information: born place, address, tags, pictures...
        * Use the features to identify my possible high school friends / community and get a list of their high school names \(Community is to supplement the rare friends case, might add the weight to the feature\). 
        * Calculate a matching ratio = The number of matching schools / The total number of friends \(Problem: How to deal with the ratio range difference? ½ = 100/ 200 \)
    * **Hypothesis3: Users will report the fake high school listed in those who want to add them as friends** 
      * * If a user reported a high school name as spam, mark the target users, and record the number of being reported. If reaching a threshold, flag the user and take actions for authentication. 
        * Then the fake high school names will be recorded in the system and could be used to identify other suspicious accounts. 
    * **Hypothesis4: Gibberish high school names e.g., “xgbixiyh high school”**
      * High school names does not match the pattern of naming norm
        * Sequence of letters can form a word
        * How to detect?
          * Detect if a string is a good string by measuring the probability of generating the next character
          * A classification model to predict the probability of transition from character-to- character and word-to-word   More: [https://stackoverflow.com/questions/6297991/is-there-any-way-to-detect-strings-like-putjbtghguhjjjanika/6298040\#comment-7360747](https://stackoverflow.com/questions/6297991/is-there-any-way-to-detect-strings-like-putjbtghguhjjjanika/6298040#comment-7360747)
          * Binary classification: Set the threshold, if less than certain threshold, then it will be counted as a gibberish name 
* **Deploy the model/solution - Scalability** 
  * Solution1 
    * Yes -- pass
    * No → Solution 2, 3, 4 
  * Solution2, Solution3, Solution 4 can output probability and add the weight for decision making for a linear regression model 
  * Further, Solution2, 3, 4 output could be utilized as features, once having sufficient training data, build a classification model to predict what kind of account pattern will have fake high school names. 



