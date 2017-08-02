---
layout: post_extend
title: "Extend Salesforce with Node.js"
description: "Introducing Auth0 Extend for Salesforce! Extend your Salesforce using node.js in just a few clicks."
date: 2017-08-01 11:13
is_extend: true
category: Extend, Webtask
author: 
  name: "Bobby Johnson"
  url: "https://twitter.com/NotMyself"
  mail: "bobby.johnson@auth0.com"
  avatar: "https://cdn.auth0.com/website/blog/profiles/bobbyjohnson.png"
design: 
  bg_color: "#3445dc"
  image_size: "95%"
  image_bg_color: "#D8E6FF"
  image: "https://cdn.auth0.com/website/blog/extend/auth0-extend-salesforce-with-node/sfdc_logo.png"
tags: 
  - extend
  - Auth0 Webtasks
  - Webtasks
related:
  - 2017-05-16-introducing-auth0-extend-the-new-way-to-extend-your-saas
  - 2017-05-19-serverless-webhooks-with-auth0-extend
  - 2016-06-28-building-serverless-apps-with-webtask
---

[Salesforce](https://www.salesforce.com) is a platform that allows developers to customize applications in multiple ways to meet any business need. The developer ecosystem is mature and requires some specialized knowledge and techniques. 


Coming from other platforms are, at times, overwhelmed at the steep learning curve and unfamiliar tooling. This is a familiar problem when switching to new platforms, you have to learn idiomatic approaches of doing things, and that can be painful. 

We on the [Auth0 Extend](https://www.auth0.com/extend/) team want to help developers discover the power of the Salesforce platform and give them access to languages that allow them to be productive quickly as well as offer new tools for the tool belt of existing developers.

Triggers can be a common source of friction when working with Salesforce. A developer can attach a trigger to fire based on several predefined events on any Salesforce object. Salesforce then presents an editor that allows them to write handler code in [Apex](https://developer.salesforce.com/page/Apex), a strongly typed object oriented programming language developed by Salesforce.

---

### Extend for Salesforce

Today we're announcing the public beta of [Extend for Salesforce](https://auth0.com/extend/salesforce). You can now implement Salesforce triggers in Node.js leveraging any of the available NPM modules. With Extend for Salesforce, your team can quickly and easily extend your Salesforce apps.

Want to notify your employees every time a big deal closes? Create an after update trigger on the Opportunity object and in your Webtask code filter based on contracts worth more than $1500 that have changed to a closed state. Then send it Slack.

Wouldn't it be nice if every time you added a lead in Salesforce, it also showed up in your Marketo marketing campaigns? Add an after insert trigger on the Lead object and send it along.

---

### Getting Started in Three Clicks

Getting started could not be simpler. Head over to [Extend for Salesforce](https://auth0.com/extend/salesforce) and click the login button. Enter your Salesforce username and password and click the login button. A list of previously created triggers will be displayed, click the Create New Trigger button to add a new one.

![1, 2, 3 Edit](https://cdn.auth0.com/website/blog/extend/auth0-extend-salesforce-with-node/extend-sfdc-authenticate2.gif)

--- 

### Working with Triggers Is Simple as Well

Creating a trigger is as simple as giving it a name, selecting the object to trigger, selecting one or more events and clicking the Create button. The Extend editor will be displayed, and you can start editing your logic in javascript.

Head over to the Salesforce Object Manager and select the Triggers tab for the object you extended, and you will see the trigger you just created with just enough Apex code to call out to Extend for Salesforce.

Don't like the trigger you just created? Hit the Delete button in the Triggers list display in Extend for Salesforce, and we will clean it all up for you.

![it's like magic](https://cdn.auth0.com/website/blog/extend/auth0-extend-salesforce-with-node/extend-sfdc-working-with-triggers.gif)

### Tell Us What You Think

We hope you are as excited as we are about Extend for Salesforce. We can't wait to see what you will create with it. And over the coming months, we will be working diligently to add new features based on your feedback.

If your organization is interested in using Extend for Salesforce or you have a tip/suggestion, just use the Talk to Sales link and let us know.