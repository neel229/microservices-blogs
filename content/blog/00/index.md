---
title: "50,000 feet overview of Microservices"
date: 2021-03-21
template: post
---

I am assuming that you have landed here for one of the following two reasons:

- To learn how to create applications with microservice architecture
- To sharpen your microservices skills a bit further

So let's cut chase right into the subject domain.

### The Crux of the Problem

So far, you might have been developing monolithic applications in which everything is tightly-coupled. By tightly-coupled, what I am trying to say is that either everything will work or it won't work at all. Even though some parts or most parts of your application might be working properly, the application as a whole won't work because of some silly error or a bug.

So what is our goal?
Our goal is to make such a system whose various working parts are decoupled(separated) from one another and error in one part should not cause the whole application to go down.

### The Solution

The term "**Microservices**" was first used at a workshop for software architects back in 2011 but some companies were already working on something similar to it known as "SOA". Though SOA is not 100% similar to Microservices but we can think of it as a subset of Microservices.

Microservice Architecture is a word you might be hearing on a day-to-day basis so let's understand what it really means.

...First of all, there's no concrete definition of what can be called a microservice architecture. But what we do know from examining the patterns followed at firms both at large and small scale is that in microservice architecture, we create services which are small and are deployed independently. It is an approach to develop a suite of small services modeled around a business domain which can be deployed independently and are good at one thing in particular. Each of these services are running their own processes, have their own data storage, and communicate with other services via mechanisms like HTTP APIs and RPCs. One more thing to note about them is that they are automatically deployed using some CI/CD toolchain.

For example, one service might be used for keeping the items list, another for processing payments, one for shipping but as a whole, they form an e-commerce system. Take a look at the following diagram.

![Project Architecture](./example.png)

You can see that we have 5 different services which as a whole constitute an e-commerce website.
First we have users(on the right) where we are using CockroachDB to store their information. This is further connected to our OAuth service whose sole purpose is to verify the user with a different type of database. Don't worry, we will be talking about the e-commerce application in detail in further blogs and we will be also making one in Go but feel free to replicate it in your language of choice.

Microservices embrace the concept of information hiding. Information hiding is described as hiding as much information as possible inside a component and exposing as little as possible via external interfaces. This allows for clear separation between what can change easily and what is more difficult to change. And since from the outside, a service is seen as a black box, it can be written in any language of your choice. So one service might be written in Go and the other in Java and yet they can still communicate with each other via network endpoints. This also means that for a particular service, we can use a specific DB type at our advantage. In the example shown above, for Users service, we are using CockroachDB since it will help us with managing the relations. On the other hand, in the OAuth service, we are using a NoSQL database in Cassandra because it would be more beneficial for that particular business domain.

So now that we know what to expect when we are talking about microservices architecture, let's go over some pros and cons of using microservices architecture.

### Pros

1. Technology Agnostic - In microservices architecture, you are not confined to using a single technology or a single programming language per se. You can write an ordering service in Java and payments system in Golang. You can use graph database like Neo4j or Dgraph when it comes to user relations and use a NoSQL database for storing auth keys.

2. Robustness - Earlier, we read that in microservice architecture, even though some services are not working, the application as a whole wont' go down. You may have seen sometimes that all of the Instagram features are working correctly except the stories feature or chats feature. This happens when a single service is down because of some bugs or system crash. This way, we make our systems more robust in nature.

3. Scaling - In monolithic architecture, we need to deploy several instances of our whole application to make it scale. Therefore, it ends up being more costlier than it needs to be. On the other hand, in microservcies architecture, we can scale a particular service to much more extent than the other services. For example, let's say stories feature on Instagram is used more often than the chats feature. This way, to handle the traffic, we can run more instances of the stories feature and comparitively less instances of chats feature.

4. Ease of Deployment - As we talked before, the main goal of ours should be faster and independent deployments of services. Thus, if Team A is ready to deploy their feature and Team B is still working on theirs, Team A can go ahead, deploy their feature and start working on something new. This is prohibited in a monolithic environment.

Of course this are not all the pros of using microservice architecture but these are the main ones.

Let's talk about some of the cons next.

### Cons

1. Developer Experience - So far, in monolithic architecture, the whole application was running on your computer and you can start developing or debugging a particular part of it with ease. But in microeservices architecture, you have to run different services at the same time and your desktop is computationally constrained. You can at max run 5-10 different services. But what if your firm like Uber or Netflix is running 100-200 microservices and upwards. Then you need to switch from local development to cloud-based development which again comes with it's own set of problems.

2. Technology Scrambling - You are given an option to write services in different languages, run different runtimes or use different types of databases. But remember that this is an option, not a requirement. You have to take into account the cost overhead using different tech-stacks comes with.

3. Latency - With a microservice architecture, processing that might previously have been done locally on one processor can now end up being split across multiple separate microservices. Information that previously flowed within only a single process now needs to be serialized, transmitted, and deserialized over networks that you might be exercising more than ever before. All of this can result in worsening latency of your system.

There are more cons to add on top of these three and we will talk about them as we go through the series.

In the upcoming blogs, we will talk about different technologies which will help us in creating microservices and quite possibly, we will be applying them to create a pizza-ordering platform.
