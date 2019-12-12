---
title: 【NLP】02 Basic Text Processing
date: 2018-10-25 12:01:31
---

## Regular expressions

a formal language for specifying text strings
1. [A-Z] [a-z] [0-9]
2. [^A-Z] [^e] [^!] [^A-Za-z]
3. groundhog|woodchuck a|b|c [gG]roundhog|[wW]oodchuck
4. colou?r oo*h! o+h! baa+ beg.n
5. ^[A-Z] [A-Z]$ ^[^A-Za-z] \.$ .$

#### Error
1. False positives(TypeI):<br/>
Matching strings that we should not have matched
2. False negaGves(TypeII):<br/>
Not matching things that we should have matched

#### antagonistic efforts:
1. Increasing accuracy or precision(minimizing false positives)
2. Increasing coverage or recall(minimizing false negatives)

#### Summary
Regular expressions play a suprisingly large role.<br/>
For many hard tasks, we using machine learning classifiers.<br/>
But regular expressions are used as features in the classifiers.<br/>
Can be very useful  in capturing generalizations.<br/>

## Regular expressions in practical NLP


## Word tokenization

#### text normalization（正常化，标准化）
1. Segmenting/tokenizing words in running text
2. Normlizing word formats
3. Segmenting sentences in running text
