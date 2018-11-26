---
title: "Understanding Observables And Iterables"
catalog: true
date: 2017-03-18 10:51:24
subtitle: "Two very important data structures explained in layman terms"
header-img: "Demo.png"
tags:
- Observables
- RxJS
- Arrays
catagories:
- JavaScript
- TypeScript

---
 In the software world, we are so used to a lot of data structures and algorithms for handling collections of data. Arrays, Lists, Queues, Stacks, Dictionaries and more seem to be the usual suspects. But these structures have kept us in thinking, for a long time, that when you need data, you will have to go for it.

 Because of this we have a lot of loop constructs in almost every programming language namely ‘for’, ‘while’, ‘do while’, ‘for in’, ‘for of’ and more. We just love iterating over data because that makes sense for us most of the time.


# Iterables
---
With Iterables, it is all about pulling data. Think of when you need water and you decide to pull from a well. There is a lot of effort involved in doing that. You have to find a:

1. good bucket or tool (choice of loop construct),
2. throw the bucket into the well and fetch some water
3. then use human effort or a lever to pull the bucket to you. Oh! and you have to do that anytime you need water.

Below is your well of data with your looping iterator (bucket).

![Your well of data with your loop iterator(bucket)](iterable1.jpeg)

And if you are pulling too much at a time, you need more hands, that is, more wait times for this synchronous operation (All hands on deck). From the picture below, I hope the lever does not break.

![Lever to pull water from well](iterable2.jpeg)

Worst! Sometimes the fetched data needs to be moved again to its consumer. And we may not even know how many rounds we have to go.

![Go Fetch More Data](iterable4.jpeg)

# Observables to the rescue!!!
---
With Observable data structures, think of standing in the middle of rain. The water comes to you. There is a lot of joy in having data being pushed to you.

![Joy of Observables](observable1.jpeg)

And even better, you can place as many buckets as you want in the rain (subscribe) and go do something else. On subscribing. In the picture below, there are 7 subscriptions to rain water.

![Subscriptions to rain](observable2.jpeg)


# Using Operators:
---
As the rain comes to you, you can `decide when` to take the rain water. You can wait for sometime before taking any of the water `(delay operator)`. You can filter out stuff from the water `(filter operator)`. You can also transform the water you fetched into drinkable water by processing `(map operator)`. You can drink just one glass of water and be done with the whole thing `(take operator)`. And so on.

![Girl In Control](observable3.jpeg)

From the picture above, you can see that the girl is really in control of how much rain water to take. That is what operators do. They help you to take charge of the stream.

#  Caution:
---
Observables brings a lot of power, enabling us to build applications that react to data in ways we could only dream of. However, having a lot of subscriptions without remembering to unsubscribe to streams that are still opened can lead to some unexpected results in your application.