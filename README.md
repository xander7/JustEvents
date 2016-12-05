# JustEvents
Events form the functional fabric of our daily lives, and are the crucial building blocks of all forms of news media. Since a large amount of online space is consumed in describing and discussing events, they are embedded within news articles, forums, blogs and different forms of online social media. Inspecting documents to assess whether a set of events occur in them is a laborious task, which becomes unfeasible when events are detected on a large-scale. Therefore, the task of automatically determining whether a given event occurs in a given document or corpus, named as *Event Validation*, has recently been studied.

*JustEvents* is a dataset for evaluating the performances of event validation methods, with the main goal of fostering research advances and comparisons in this field. The dataset consists of 250 events and 6,457 candidate documents used as a base for assessing event occurrence. Events and documents are coupled in pairs and are associated with validity
judgments, indicating the number of event participants acting together within the event timespan in the document. These validity judgments were assigned by human evaluators via crowdsourcing.

## Citing JustEvents
If you are using JustEvents for any purpose, please cite the following paper:

A. Ceroni, U. Gadiraju, and M. Fisichella. 2017. JustEvents: A Crowdsourced Corpus for Event Validation with Strict Temporal Constraints. In Proceedings of the 39th European Conference on IR Research, ECIR 2017.

If you are dealing with event validation, you are also encouraged to cite the following papers:
- A. Ceroni, U. Gadiraju, and M. Fisichella. 2015. Improving Event Detection by Automatically Assessing Validity of Event Occurrence in Text. In Proceedings of the 24th ACM International Conference on Information and Knowledge Management, CIKM 2015, pp. 1815-1818.
- A. Ceroni and M. Fisichella. 2014. Towards an Entity-Based Automatic Event Validation. In Proceedings of the 36th European Conference on IR Research, ECIR 2014, pp. 605-611.

## Events
Each of the 250 events is made of (i) a set of participants, and (ii) a start and end date, indicating the timespan within which the event occurred. Events have been detected by applying the algorithm introduced by Tran et al. (2014) to detect
events, working on the Wikipedia Edit History of more than 1.8 million Wikipedia pages representing persons, locations, artifacts, and groups. Titles of Wikipedia pages are considered as event participants in the dataset. The considered time period spans from 18th January 2011 to 7th February 2011.

NOTE. Since events have been detected automatically, the event set also contains false events (due to either unrelated participants or wrong time span) as well as partially true events, where one or more intruders are present along with the true event participants. These events have been retained in the set and were also subject to manual evaluation since event validation has to deal with not only true and clean events, but also with false or ambiguous ones. Our comprehensive
dataset thus supports evaluation of event validation methods for all potential cases, and contains a corresponding ground
truth for them.

Events are contained in the *events.txt* file. Its tab-separated fields are the following:
- **event ID**: the unique integer associated to events.
- **start date**: the starting date of the period in which the event was supposed to occur. Format date: YYYY-MM-DD.
- **end date**: the ending date of the period in which the event was supposed to occur. Format date: YYYY-MM-DD.
- **participants**: the participants who were supposed to take part to the same event. They are separated by semicolons.

## Documents
The documents in the dataset consist of web pages that have been subject to scrutiny in order to assess the validity of events. The Web has been chosen as source for documents due to its easy accessibility and wide event coverage. For each event, queries have been constructed by concatenating the name of event participants along with the months and year covered by the dataset (one distinct query for January 2011 and another for February 2011). The Bing Search API has been used to perform queries and to retrieve the top-20 web pages for each query in terms of URLs. Plain text has been extracted by using BoilerPipe, while the Stanford CoreNLP parser has been used for POS tagging, named entity recognition, and temporal expression extraction. After removing duplicates and discarding non-crawlable webpages, the number of documents sums up to 6,457.

Documents are contained in the *documents.txt* file. Its tab-separated fields are the following:
- **document ID**: the unique integer associated to the document.
- **url**: the url of the web page the document comes from.
- **title**: the title of the web page the document comes from.
- **text**: the plain text extracted from the web page. Note that this field can contain tabs that were present in the extracted text.

## Pairs
Events and retrieved candidate documents have been logically coupled within (event, document) pairs, which in turn have been subject to validity assessment by human evaluators.

Pairs are contained in the *pairs.txt* file. Its tab-separated fields are the following:
- **event ID**: the id of the event in the pair.
- **document ID**: the id of the document in the pair.

## Validity Judgments
In order to manually evaluate the validity of the 6,457 pairs in the dataset, the task of assessing whether or not a document contains evidence of the occurrence of an event into has been decomposed into atomic units deployed on CrowdFlower. For each pair, workers were presented with a description of the event participants and timespan as well as the document URL. The event timespan, specified by a start and end day, was strictly considered during the tasks. The workers were
then asked to report the number of event participants conforming to the same event in the document and within the event timespan. Workers also had the possibility to specify whether the web page was not available and whether the temporal bearings of the web page were not clear. A preview of the crowdsourcing job is available and can be previewed [here](http://www.l3s.de/~gadiraju/SIGIR2016/cs_job/). Task design guidelines has been followed to engage the crowd workers and gold standard questions have been asked to detect untrustworthy workers. A monetary reward of 20 USD cents for each set of 10 pairs has been offered to workers on successful task completion. For each pair, at least 5 independent validity judgments have been gathered, resulting in over 32,285 responses in total.

Validity judgments can assume the following values:
- **>= 0:** the number of event participants conforming to the same event in the document AND within the event timespan.
- **-1**: the web page was not available at the time of the evaluation.
- **-2**: the temporal bearings of the web page were not clear.

Validity judgments are contained in the *judgments.txt* file. Its tab-separated fields are the following:
- **document ID**: the id of the document in the pair.
- **event ID**: the id of the event in the pair.
- **# judgments**: the number of independent validity judgments gathered for the pair.	
- **agreed judgment**: the most frequent judgment within the judgments pool for the pair.	
- **max frequency**: the frequency of appearance of the *agreed judgment* within the judgments pool for the pair.
- **tie**: whether there was more than one judgment with the same frequency of appearance. *0* means no tie, *-1* means tie.
- **alternate judgment**: the value of the other judgment with same appearance frequency as the one of the *agreed judgment*. This field is set to *null* if there was no tie.
- **closest judgment to avg**: the judgment that is closest to the average of all judgments for the pair. This can be used as tie breaker.
- **avg judgment**: the average of the judgments for the pair.	
- **pairwise agreement**: pairwise agreement over the judgments for the pair.
- **judgments**: individual judgments given for the pair. There are at least 5 judgments for each pair and they are separated by tabs.
