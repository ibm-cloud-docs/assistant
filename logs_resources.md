---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-13"

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

# More resources
{: #logs_resources}

Use other tools to help you gain insight from user conversation logs.
{: shortdesc}

## Jupyter notebooks
{: jupyter-notebooks}

IBM created Jupyter notebooks that you can use to analyze your log data in more detail. A Jupyter notebook is a web-based environment for interactive computing. You can run small pieces of code that process your data, and you can immediately view the results of your computation.

There is a set of notebooks that you can use with standard Python tools and a set that is designed for optimal use with {{site.data.keyword.DSX_full}}. {{site.data.keyword.DSX_short}} is a product that provides an environment in which you can pick and choose the tools you need to analyze and visualize data, to cleanse and shape data, to ingest streaming data, or to create, train, and deploy machine learning models. See the [product documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window} for more details.

The following notebooks are available:

- **Measure**: Gathers metrics that focus on coverage (how often the assistant is confident enough to respond to users) and effectiveness (when the assistant does respond, whether the the responses are satisying user needs).

- **Effectiveness**: Performs a deeper analysis of your logs to help you understand the steps you can take to improve your assistant.

The [Watson Assistant Continuous Improvement Best Practices Guide ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) describes how to get the most out of the notebooks.

### Using the notebooks with {{site.data.keyword.DSX}}
{: #notebook-steps}

If you choose to use the notebooks that are designed for use with {{site.data.keyword.DSX}}, at a high level, the steps are:

1.  Create a {{site.data.keyword.DSX}} account, [create a project ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window}, and add a Cloud Object Storage account to it.
1.  Give Object Storage access to your service instance logs from conversations that users have had with your assistant (by providing the workspace id and credentials).
1.  From the {{site.data.keyword.DSX}} community, get the [Measure Watson Assistant Performance notebook ![External link icon](../../icons/launch-glyph.svg "External link icon")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568).
1   Follow the step-by-step instructions provided with the notebook to analyze a subset of the dialog exchanges from the logs.

    The insights are visualized in ways that make it easier to understand the assistant's coverage and effectiveness.
1.  Export a sample set of the logs underlying visualizations from ineffective conversations, and then analyze and annotate them.

    For example, indicate whether a response is correct. If correct, mark whether it is helpful. If a response is incorrect, then identify the root cause, the wrong intent or entity was detected, for example, or the wrong dialog node was triggered. After identifying the root cause, indicate what the correct choice would have been.
1.  Feed the annotated spreadsheet to the [Analyze Watson Assistant Effectiveness notebook](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c).

This process helps you to understand the steps you can take to improve your assistant. There is no way to automatically apply what you learn back to your service instance. Keep track of any changes you make to improve the system, so you can subsequently apply them to the training data of your dialog skill directly.

### Using the notebooks with standard Python tools

If you choose to use standard Python tools to run the notebooks, you can get the notebooks from the [IBM GitHub repository](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook). Be sure to run them in the following order:

1.  Measure Notebook.jpynb
1.  Effectiveness Notebook.jpynb
