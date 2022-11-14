---
layout: post
title:  "Data Governance"
kicker: "Data Governance"
subtitle: "Data governance is the process of defining security guidelines and policies and making sure they are followed by having authority and control over how data assets are managed."
image: assets/images/posts-cover-images/data-governance.jpg
featured_image: assets/images/featured-images/data-governance.jpg
author: senthil
date: 2022-07-26 19:00:00 +0530
last_modified_at: 2022-07-29 00:00:00 +0530
tags: ["data-engineering", "big-data", "data-goverance", "data-security"]
categories: data-security-and-compliance
featured: true
hidden: false
---

> **Writing in progress:** If you have any suggestions for improving the content or notice any inaccuracies, please email me at [hello@senthilnayagan.com](mailto:hello@senthilnayagan.com). Thanks!

What do we mean when we claim the data is secure? It means:
- Data is only *accessible to authorized users* in authorized ways.
- Data is *auditable*, which means that all accesses, including modifications, are logged.
- Data *complies with all regulations*.

# What is data governance?

Data governance is a collection of *processes*, *roles*, *policies*, *responsibilities* and *standards* that ensure data is *secure*, *accurate*, and *available as an asset* across the organization. In other words, it is the process of defining security guidelines and policies and making sure they are followed by having authority and control over how data assets are managed. 

It defines ***who** can take **what actions** based on **what data**, under **what conditions**, and using **what methods**.*

The practice of data governance also includes adhering to external standards set by industry associations, government agencies, and other stakeholders. Regulations such as the GDPR and many others impose legal accountability and severe penalties on firms ([*in the case of GDPR, the penalty may be up to 4% of global revenue*](https://gdpr.eu/fines/){:target="_blank"}) that fail to adhere to governance principles around data privacy, retention, and portability. 

No matter how large the organization is or the volume of data, the principles of data governance remain the same. However, data governance practitioners make decisions about tools and ways to implement them based on practical factors that are affected by the environment in which they work.

> It is important to note that data governance is not just about data security; it is more than that. Data governance guarantees that data is trustworthy while also maintaining its quality and integrity.

## Enhancing trust in data

The goal of data governance is to establish *trust in data*. Trustworthy data is required to enable decision making and risk assessment. To ensure data trust, a data governance strategy must address three key aspects:

- **Discoverability**
- **Security**
- **Accountability**

## Data quality

We can accomplish data quality by using data governance. Data quality is focused on making sure that the data complies to our data quality dimensions such as *accuracy*, *completeness*, *validity*, *timeliness*, *consistency*, and so on. To put it simply, data quality guarantees that we have high-quality data.

We can figure out the data quality by looking at its source, i.e., where it came from (e.g., was it entered by humans, who often make mistakes?). A *sense of ownership* is one method for improving data quality—making sure the business unit responsible for generating the data also owns the quality of that data. 

The organization can set up a *data acceptance process* that states that data cannot be used until the people who own it demonstrate that it meets the organization's quality criteria.

## Why is data governance important?

Data governance is not only about managing the rules but also making data useful. Effective data governance implementation ensures that high-quality data must be efficiently available to the right people throughout the organisation.

## Data governance vs. data management

### Data governance

- Data governance describes the general structure that need to be in place.
- It has policies, procedures, and accountability.
- It's more about what should happen and how things should happen.

### Data management

- The goal of data management is to put all of the policies into practice.
- It's a hands-on daily effort to make sure that the policies we put in place are being followed.

## Data encryption

Data encryption adds another level of protection. Only the systems or users who have the keys can make sense of the data. There are several data encryption implementations available. However, the *envelope encryption* technique offers the best security while also performing well.

## Identity and access management (IAM)

Access control is based on who the user is (called "authentication") and whether or not the user is allowed to access certain data (called "authorization"). 

Authorization is based on a set of permissions and roles that are tied to a user's or service's identity. Authorization decides whether or not the user is allowed to access or perform any activity on the data in question.

Authentication is typically handled by providing a password associated with the individual seeking access. The obvious weakness of this approach is that anybody who has obtained access to the password may access whatever that person has access to. So we must add extra layers of protection to the authentication process by making it more difficult for attackers to acquire access-we can enhance it even further by adding *two-factor authentication*. If possible, we may include *biometric data in the authentication request*.

## Roles

The first thing we need to consider when it comes to data governance is who is engaged and what their roles are. There are usually several roles, but the most important one is *data owner* or *data sponsor*. These are the individuals who have ultimate decision-making authority over the data and are solely responsible for ensuring that the data is accurate and up to date. These individuals have a deeper understanding of the groundwork that's being done with the data. There could be many data owners or data sponsors. For instance, a sales data owner, an inventory data owner, etc.