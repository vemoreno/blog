---
layout: post_extend
title: "Extend Salesforce with Node.js"
description: "Introducing Auth0 Extend for Salesforce! Extend your Salesforce using Node.js in just a few clicks."
date: 2017-08-01 11:13
is_extend: true
category: Extend, Technical, Salesforce
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

Salesforce is a SaaS CRM platform that can be customized by developers in multiple ways to meet business needs. The developer ecosystem is mature but requires some specialized knowledge and techniques.

For instance, developers can attach triggers to business objects, including custom objects. Trigger logic implemented in [Apex](https://developer.salesforce.com/page/Apex), a strongly typed object oriented programming language developed by Salesforce, is a common source of friction for new developers.

We on the [Auth0 Extend](https://www.auth0.com/extend/) team want to help developers discover the power of the Salesforce platform and allow them to be productive quickly as well as offer new tools for the tool belt of existing developers.

---

### Extend for Salesforce

Today we are announcing the public beta of [Extend for Salesforce](https://auth0.com/extend/salesforce). You can now implement Salesforce triggers in Node.js leveraging any NPM module. With Extend for Salesforce, your team can quickly and easily customize your Salesforce logic using a language most modern programmers already use.

Want to notify your employees every time a big deal closes? Create an after-update trigger on the Opportunity object and in your Node.js handler filter based on contracts worth more than $10,000 that have changed to a closed state. Then send it to Slack.

Wouldn't it be nice if every time you added a lead in Salesforce, it also was added to your Marketo campaigns? Add an after-insert trigger on the Lead object and send it along.

---

### Working with Triggers Is Simple

Creating a trigger is as simple as giving it a name, selecting the object to trigger, choosing one or more events, and clicking the Create button. The Extend editor is displayed, and you can start editing your logic in JavaScript.

In the Salesforce Object Manager, view triggers for the object you extended, and you see the logic Extend for Salesforce created on your behalf with just enough Apex code to call out to your Node.js handler.

Don't like the trigger you just created? Hit the Delete button in the Triggers list display in Extend for Salesforce, and it cleans it all up for you.

![it's like magic](https://cdn.auth0.com/website/blog/extend/auth0-extend-salesforce-with-node/extend-sfdc-working-with-triggers.gif)

---

### Now It Is Your Turn

How about we walk through setting up that first Slack notification for you? First, you need to create an [Incoming Webhook](https://api.slack.com/incoming-webhooks) in Slack.  Copy the **Webhook URL**, then head over to [Extend for Salesforce](https://auth0.com/extend/salesforce/app) and log in, if you have not done so already. Follow these steps.

- Click the Create New Trigger button
- Name it "SendToSlack"
- Select the Lead Salesforce object
- Select the After Insert event
- Finally, click the Create button

You should now see the Extend Editor. Click the double headed arrow icon in the lower left-hand corner of the editor to give you some room to work. 

We have added some helpful comments to get you started. However, for now, add the following code.

```javascript
module.exports = (ctx, cb) => {

  var slack = require('slack-notify')(ctx.secrets.slack_url);
  var item = ctx.body.new[0];

  var message = {
    channel: 'general',
    username: 'Salesforce',
    icon_emoji: ':moneybag: ',
    text: 'New Lead: ' + item.Name,
    attachments:[ { text: 'running on node version: ' + process.version } ]
  };

  slack.send(message);
  cb();
};
```

This code uses the slack-notify NPM module to send a message to the #general channel in Slack with the username Salesforce. I have also added an attachment to the message that displays the executing version of Node.js used. Extend for Salesforce runs on Node 8, giving you access to the most recent language features.

As mentioned earlier, you have full access to all Node.js modules hosted on NPM. To use slack-notify, you need to add it as a dependency.  

- Click the wrench icon in the upper left-hand corner of the editor
- Select NPM Modules
- Click the Add Module button
- Enter "slack-notify" 
- Hit Enter
- Close NPM Modules

Finally, the code uses `ctx.secrets.slack_url` to initialize the Slack object. Secrets are a secure storage feature of Extend for Salesforce. The values are encrypted while stored and decrypted only for execution. Let's hide your Slack Webhook URL there.

- Click the wrench icon in the upper left-hand corner of the editor
- Select Secrets
- Click the Add Secret button
- Enter "slack_url" as the key
- Enter the Slack Webhook URL as the value
- Click the Save button
- Close Secrets

That is all there is to it. Add a new lead in Salesforce and watch it magically show up as a message in Slack. If you would like to fancy up that message, take a look [here](https://api.slack.com/docs/messages) for details on customizing messages for Slack.

--- 
### Tell Us What You Think

We hope you are as excited as we are about Extend for Salesforce. We cannot wait to see what you will create with it. Over the coming months, we will be working diligently to add new features based on your feedback.

If your organization is interested in using Extend for Salesforce or you have a tip/suggestion, [let us know]().