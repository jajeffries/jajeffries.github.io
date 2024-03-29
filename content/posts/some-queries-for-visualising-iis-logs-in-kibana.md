---
title: 'Some queries for visualising IIS logs in Kibana'
date: Tue, 14 Mar 2017 08:49:36 +0000
draft: false
tags: []
---

We pump all our IIS logs into an an ELK stack, partly as a centralised log store and partly so we can easily analyse them. 

## A few useful queries
### Get all errors
```
scstatus: ([500 TO 599] OR [400 TO 403] OR [405 TO 499])
```

### Requests that took 200ms or more 
```
timetaken: [200 TO *]
```

### Requests that took 200ms or more that aren't POST requests. 
```
timetaken: [300 TO *] AND NOT method: POST
```

While these queries are interesting on their own, using aggregations in the visualisation section can give us further insite. For example, here is the number of slow requests per minute. ![slowrequests](https://jajeffries.files.wordpress.com/2017/03/slowrequests.png) Using and becoming familiar with these aggregations can quickly help you build useful dashboards based around what's valuable and what you're working on at the moment. Right now, I'm doing some profiling so response time matters, but know when slow requests happen doesn't really help much. If I change to use the significant terms aggregation I can find which pages are most common in my search. ![topslowpages](https://jajeffries.files.wordpress.com/2017/03/topslowpages1.png) The blacked out parts are the URLs of the slowest pages. Now I can start profiling those pages specifically and see if I can track down any bottle necks. As you can see the pages on the left of the chart are where I need to focus my attention. Hopefully this shows you how easy it is to do some basic analysis to point you in the right direction.