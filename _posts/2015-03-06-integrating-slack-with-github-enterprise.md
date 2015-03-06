---
layout: post
title: "Integrating Slack With Github Enterprise"
description: ""
category: git
tags: [slack, github, git]
---
{% include JB/setup %}

If you are using **Slack**(Yes it's awesome) and **Github Enterprise**(Awesome too), then you might have played around with the integration of both the tools.

The [Slack documentation](https://slack.zendesk.com/hc/en-us/articles/201710957-Using-the-GitHub-integration-with-GitHub-Enterprise) helps a bit but to have a complete integration with `issues`, `issues comments` etc, you'll have to jump through some hoops. So let me put it all below.

### Adding Webhook in **Github Enterprise** for your **Slack** group/channel

First step is to obviously add a hook in your company's **Github Enterprise** repo for **Slack**. Skip to [next section](#update-webhook) if you've already addded the **Github Webhook**.

1. Click on the **Add Service Integration** menu link for your group


2. Select your **Slack** group or channel where you want the **Github** notifications:

   ![Github Channel](/assets/media/github-select-group.png)


3. Now click on the **switch to unauthed mode**

   ![Github Channel](/assets/media/github-unauth-mode.png)


4. Copy the **Webhook URL** you see on the next screen. Now goto the **Settings** > **Hooks** page of your repo in Github Enterprise. Add a **Webhook** and paste the Webhook URL there.

After adding the **Webhook** you should receive notifications when anyone pushes commits to your repo.


<a name="update-webhook"></a>

### Updating Webhook in **Github Enterprise** to support more notifications


Now you may have noticed that even after adding a **Webhook**, you can only see notifications for new `commits`. The integration is still missing notifications for events such a new `issue` or `issue comment` etc.


The reason is that Webhook only publishes commit events by default. You can change this behaviour by altering the hook via [Github Webhook API](https://developer.github.com/webhooks/)


Assuming your **Github Enterprise** link as `http://githuben.mycompany.com/`, your organization as `myorg` and repo as `myrepo`


1 . Find your hook url (containing the hook id) by retrieving a list all the `hooks` for a `repo`:

    curl --user "<User>:<Password>" http://githuben.mycompany.com/api/v3/repos/myorg/myrepo/hooks

  You should see a big `JSON` output. All you need is the `url` of the hook matching your **Slack Webhook** integration url.


     [
        {
          "url": "http://githuben.mycompany.com/api/v3/repos/myorg/myrepo/hooks/1121",
          "test_url": "http://githuben.mycompany.com/api/v3/repos/myorg/myrepo/hooks/1121/test",
          "id": 1121,
          "name": "web",
          "active": true,
          "events": [
            "push"
          ],
          ....
          "config": {
                "url": "https://hooks.slack.com/services/SOMEHASH/SOMEHASH/SOMEHASHBUTLONGER",
                "content_type": "form",
                "insecure_ssl": "1"
              },
          ....



  You can see in the `JSON` aboce, that `events` in the hook is only `push`. The url for our **Slack** hook is - `http://githuben.mycompany.com/api/v3/repos/myorg/myrepo/hooks/1121`


2 . Now add all the extra notifications to the `events` for the **Webhook** using the `hook url` from above:


    curl -X PATCH --user "<User>:<Password>"  -H "Content-Type: application/json"
    http://githuben.mycompany.com/api/v3/repos/myorg/myrepo/hooks/1121
    -d '{"add_events": ["commit_comment", "create", "issue_comment", "issues", "pull_request_review_comment"]}'

That's all. You should now be able to receive notifications for all these events apart from push. You can call the `Get Hooks` api again to confirm.