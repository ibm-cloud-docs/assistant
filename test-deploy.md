---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Testing in Slack

You can use the test deployment tool to integrate your assistant into a Slack team as a bot user. Use this method if you want to quickly test using a Slack bot as the user interface for your assistant.

The test deployment tool uses the {{site.data.keyword.openwhisk}} service to deploy a prebuilt Slack application to your team as a bot user. This application handles communication with your assistants.

![Test deployment overview diagram](images/testdeploy_diagram.png)

Note that the test deployment tool has some limitations:

- You cannot use this tool to publish an application for other teams to use.
- If you use this method to deploy more than one assistant to the same team, all of the assistants will respond to the `@ibmwatson_bot` user name. It is recommended that you use this tool to deploy only one assistant at a time to each Slack team.
- You must have permission to install apps to your Slack team. Check with your Slack administrator if you are not sure whether you have this permission.
- The prebuilt Slack application is for testing purposes only, and might not be available at all times.
- Because of {{site.data.keyword.openwhisk_short}} restrictions, this tool is currently available only for the {{site.data.keyword.Bluemix_notm}} US South region.

To install your application as a bot user:

1. In the {{site.data.keyword.conversationshort}} tool, open the assistant you want to test in Slack.
1. Click the menu icon in the upper left corner, and then select **Deploy**. The Deploy Options page opens.

   ![Quick deploy menu option](images/deploy_menu_testdeploy.png)

1. Under **Deploy with {{site.data.keyword.openwhisk_short}}**, click **Test in Slack** and follow the instructions.

   ![Create Slack test button](images/testdeploy_testinslack.png)

## Chatting with the bot

After you complete the deployment process, you can use the `@ibmwatson_bot` user name to interact with your assistant, just as you would with any other Slack bot.

Keep in mind that the bot deployed to your team preserves state for each user within a particular channel. This means that any variables you store in the dialog context are maintained indefinitely, unless your dialog clears them.

If you need to be able to reset the conversation to a known starting state, you must do so within your dialog. Make sure that your dialog has a node that is executed at the end of the conversation or any other time you need to start over. Update the JSON for this node to reset all context variables to the appropriate starting values. (If necessary, you can use **Jump to** actions or a special "end conversation" intent to execute this node.)

For example, if your assistant uses a context variable called `drink_order` to store a user's beverage selection, you can use the `context.remove` method to delete this variable when the conversation ends:

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

For more information about modifying context variable values, see [Updating a context variable value](dialog-overview.html#updating-a-context-variable-value).

**Note:** When you have finished testing your assistant, you can delete the test deployment by going back to the test deployment tool and clicking **Delete test**. Remember that you must also separately deauthorize the bot application in your Slack team.