+++
date = '2025-03-01T15:07:13+05:30'
draft = false
title = 'Dazhboard contributions'
pin = true
+++

## Dazhboard contributions
<!--more-->
I have gone through a lot of code in the dazhboard and implemented real functionality as well as removed bugs from it.



my notable additions in the dazhboard are Survey Review API -> complete end to end implementation with closely working with the front end team designed, developed & tested the complete functionality according to the clients requirement

1. support for multiple types of questions ,
2. support for conditional question with potential of unlimited condition and unlimited nested condition functionality
3. complete crud( create, read, update, and delete ) works flawlessly


git commit hash for survey API

```
bd05608: Survey API
```
```
ee901ea: Ready for test the req res api/surveys put & post
```
```
16fd0b7: Multi Condition and Nested Condition support
```
```
395aee5: JOI validations used for survey api
```
```
2369daf: endpoints renamed /surveys instead of /survey
```
```
d23ffc1: include minRating and maxRating in question
```
```
796b48a: survey soft delete
```


similarly made and developed the survey response model along with the analytics from end to end and have complete flow and knowledge of how the module works an how to extend whatever needed

```
7294aec: Review Survey Response Initial Model
```
```
93fa6c6: Get survey resoponse & stats
```
```
e2a00db: SurveyQuestionModel add more fields
```
Review Invite email API - removed several bugs [ a lot of them ] from the review invite email API.

SMTP was completely broken debugged it read through 5k lines of code, the code base is quite overwhelming to understand and grasp but took the challenge and fixed the smtp which was the bottleneck for actual review invite email API now i understand the complete flow for this.



list of bugs & features that i completed with the git commit hash

```
152e83e: Average Rating field avgRating
```
```
e1f08d4: Type added in review widget model
```
```
6658730: new service collectDeviceStats(req)
```
```
387c5e6: send id in body for the put
```
```
f815b46: new field for the survey isActive
```
```
cfc5545: Proper check for the active flag
```
```
6fa5ea5: response with success only
```
```
4bc30cf: pdf-lib dependency
```
```
7d189a3: Better validation in edit review survey
```
```
abd6d16: Remove posId and smtpId check
```
```
d9b22e4: smtp working now
```
```
c40d065: Sender name fallback values
```
```
43991f5: send dummy mail every hour
```
```
40cef5f: Dont save empty posID
```
```
b9f156d: Review Link posId error removed
```
