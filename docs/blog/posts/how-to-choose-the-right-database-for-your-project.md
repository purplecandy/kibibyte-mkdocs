---
date: 2022-05-15
authors: [nadeem]
title: How to choose the right database for your project?
description: >
  Key steps involved in choosing a database when building a project from scratch
categories:
  - Databases
---

Choosing the right database is one of the most important decisions you have to make when starting a new project. It should be the first building block when deciding on the tech stack.

Nowadays, you have a lot of choices than the usual RDBMS (MySQL or Postgres) which are great and tend to solve most of the problems but you should understand the advantages and disadvantages of different technologies. So down the line, you don't regret making a wrong choice.

The article aims to give you a framework/process for choosing a database instead of exploring technologies and comparing them.

The following steps can help you identify the right database:

#### Identify the use cases

You can't think of deciding on a database without knowing the use cases.

The following criteria can help:

1. Gather the business requirement
2. List down the feature sets separately
3. Determine what features are must-haves and will be immediately implemented
4. Understand the future use cases and features that may get added in the pipeline
5. Analyse the business requirements and build user stories

#### Visualise your use cases

Try to understand and roughly visualize how you will define the data models for the given use cases. It's an important step to help identify potential problems.

It will be an exercise heavily dependent on your experience, so don't forget to brainstorm with your team members.

For example, you can try to define approaches for the most critical features or stories and build on top of it.

#### Laydown potential problems and the key factors

You have identified the use cases and roughly figured out some approaches. Now you are in a much better position to answer 'What are the things you expect from your database?' and potential problems related to scaling, performance, maintenance, etc.

It can include the technical requirements such as

- query patterns
- data patterns
- storage capacity
- consistency
- throughput
- SDKs and libraries

and business requirement

- current team skillset a
- maturity and stability
- community support
- manged or self-hosted
- cost

Something like the below image is a gist of what we wanted from the database.
![image.png](https://res.cloudinary.com/kibibyte/image/upload/v1691933599/K58c81Y5l_kktumk.png align="center")
You can define it more specifically to your needs and add criticality levels (Severe, High, Medium, Low) as well.

#### Survey of technologies

Now that we have a clear idea of what to expect from our database and the use cases. We can start exploring the database, considering the factors we have defined above.

Tip: You can take the help of [DB Engine](https://db-engines.com/en/ranking) it keeps a list of most of the databases according to their type and also has a popularity index which can be helpful in some cases.

![Key steps screenshots](https://res.cloudinary.com/kibibyte/image/upload/v1691933599/K58c81Y5l_kktumk.png)

You can create a decision matrix of the factors and databases. It can help easily distinguish the advantages and disadvantages among databases.

#### Conclusion

The decision matrix should help you outline the benefits of one over another. You can brainstorm with your team members and project members and conclude.

Some tips you can that can help

- It's critical to understand where the comfort of your team lies. You should thoroughly understand the implications of adopting a new database and the problems it solves.

- Good development practices are a must. A poor data model and query won't scale well irrespective of the database. Always make sure that the models you define and queries are optimized.

- Performance and scaling metrics are highly dependent on your use case and database design. Try to take benchmarks with a pinch of salt.
