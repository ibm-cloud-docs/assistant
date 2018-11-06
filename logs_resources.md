---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-05"

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

# Other analytics resources
{: #logs_resources}

Use other tools to help you gain insight from user conversation logs.
{: shortdesc}

## Jupyter notebooks
{: jupyter-notebooks}

IBM created Jupyter notebooks that you can use to analyze your log data in more detail. A Jupyter notebook is a web-based environment for interactive computing. You can run small pieces of code that process your data, and you can immediately view the results of your computation.

There is a set of notebooks that you can use with standard Python tools and a set that is designed for optimal use with IBM Watson Studio. IBM Watson Studio is a product that provides an environment in which you can pick and choose the tools you need to analyze and visualize data, to cleanse and shape data, to ingest streaming data, or to create, train, and deploy machine learning models. See the [product documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window} for more details.

The notebooks focus on the following metrics in particular:

- Effectiveness: Measures whether the assistant is satisying the user requests it is processing, and includes metrics such as:

  - Portion of conversations not handed off or escalated to a human agent for quality reasons.
  - Whether the assistant fulfills the customer's request or answers the customer's question during a conversation.
  - Customer satisfaction scores on a scale of 1-10.

- Coverage: Measures how often the assistant is confident enough to respond to users.

Coverage is about how often the assistant responds. Effectiveness is about how useful the responses are. Focus on effectiveness first. Effectiveness should be high before you expand the assistant's coverage.

### How it works
{: #notebook-steps}

If you choose to use the notebooks that are designed for use with IBM Watson Studio, at a high level, the steps are:

1.  Create a Studio account, [create a project ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window}, and add a Cloud Object Storage account to it.
1.  Give Object Storage access to your service instance logs from conversations that users have had with your assistant (by providing workspace id and credentials).
1.  Go to the [IBM Watson GitHub repository](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook) to find the notebooks.
1   Follow the step-by-step instructions provided in the `Measure Notebook.ipynb` to analyze a subset of the dialog exchanges from the logs.

    The insights are visualized in ways that make it easier to understand the assistant's coverage and effectiveness.
1.  Export a sample set of the logs underlying visualizations from ineffective conversations, and then analyze and annotate them.

    For example, indicate whether a response is correct. If correct, mark whether it is helpful. If a response is incorrect, then identify the root cause, the wrong intent or entity was detected, for example, or the wrong dialog node was triggered. After identifying the root cause, indicate what the correct choice would have been.
1.  Feed the annotated spreadsheet to the `Effectiveness Notebook.ipynb`.

This process helps you to understand the steps you can take to improve your assistant's training data. However, there is no way to automatically apply what you learn back to your service instance. You must track changes that improved your system and apply them to your instance yourself.