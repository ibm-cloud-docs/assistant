---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-30"

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

# 定义实体
{: #entities}

***实体***表示对象类或与用户目的相关的数据类型。通过识别用户输入中提及的实体，{{site.data.keyword.conversationshort}} 服务可以选择执行特定操作来实现意向。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/kAZ9m-oCKxM" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 实体限制
{: #entity-limits}

可以创建的实体、实体值和同义词的数量取决于 {{site.data.keyword.conversationshort}} 服务套餐：

| 服务套餐          | 每个工作空间的实体数   | 每个工作空间的实体值数      | 每个工作空间的实体同义词数      |
|-------------------|-----------------------:|----------------------------:|--------------------------------:|
| 标准/高级         |                          1000 |                            100,000 |                              100,000 |
| Lite              |                            25 |                            100,000 |                              100,000 |

启用供使用的系统实体数会计入套餐使用量总计。

## 创建实体
{: #creating-entities}

使用 {{site.data.keyword.conversationshort}} 工具来创建实体。

1.  在 {{site.data.keyword.conversationshort}} 工具中，打开工作空间，然后单击**实体**选项卡。如果**实体**选项卡未显示，请使用 ![菜单](images/Menu_16.png) 菜单来打开该页面。

1.  单击**添加实体**。

        还可以单击**使用系统实体**以从 {{site.data.keyword.IBM_notm}} 提供的常见实体列表中选择可应用于任何用例的实体。有关更多详细信息，请参阅[启用系统实体](#enable_system_entities)。


1.  在**实体名称**字段中，输入实体的描述性名称。

    实体名称可以包含字母（Unicode 格式）、数字、下划线和连字符。例如：
    - `@location`
    - `@menu_item`
    - `@product`

    在 {{site.data.keyword.conversationshort}} 工具中创建实体名称时，不要在实体名称中包含 `@` 字符。实体名称不能包含空格，长度也不能超过 64 个字符。实体名称不能以字符串 `sys-` 开头，该字符串保留用于系统实体。
    {: tip}

1.  选择**创建实体**。

    ![创建实体的截屏](images/create_entity.png)

1.  在**值名称**字段中，输入实体的可能值的文本，然后敲击 `Enter` 键。实体值可以是最大长度为 64 个字符的任何字符串。

    > **重要信息：**不要在实体名称或值中包含敏感或个人信息。名称和值可能会通过 URL 在应用程序中公开。

1.  对于**模糊匹配**，单击按钮以选择是启用还是禁用；缺省情况下，模糊匹配处于禁用状态。此功能可用于[支持的语言](lang-support.html)主题中注明的语言。
 {: #fuzzy-matching}

    如果用户输入项使用的语法类似于实体，那么通过启用模糊匹配，无需完全匹配，服务就可以更好地识别此类用户输入项。有三个组件可用于模糊匹配 - 词干提取、拼写错误检查和部分匹配：
    - *词干提取* - 此功能识别具有多种语法形式的实体值的词干形式。例如，“bananana”的词干为“banana”，“running”的词干为“run”。
    - *拼写错误检查* - 此功能能够将用户输入映射到对应的正确实体，就算存在拼写错误或语法方面略有差异也能正确映射。例如，如果将 *giraffe* 定义为某个动物实体的同义词，而用户输入包含词汇 *giraffes* 或 *girafe*，那么模糊匹配能够正确地将该词汇映射到该动物实体。
    - *部分匹配* - 部分匹配功能会自动建议在用户定义的实体中提供的基于子字符串的同义词，并为其分配低于完全实体匹配的置信度分数。

    **注** - 对于英语，模糊匹配会阻止捕获某些常用、有效的英文单词作为给定实体的模糊匹配。此功能使用标准英语字典中的单词。您还可以定义英语实体值/同义词，并且模糊匹配将仅匹配您定义的实体值/同义词。例如，模糊匹配可能会将词汇 `unsure` 与 `insurance` 匹配；但如果您将 `unsure` 定义为 `@option` 之类的实体的值/同义词，那么 `unsure` 将始终与 `@option` 匹配，而不会与 `insurance` 匹配。

1.  一旦输入了值名称，就可以通过从*类型*下拉菜单中选择`同义词`或`模式`，为该实体值添加任何同义词或定义特定模式。

    ![值的类型选择器](images/value_type.png)

    > **注：**可以为单个实体值添加同义词*或*模式，但不能同时添加这两者。

    - 在**同义词**字段中，输入实体值的任何同义词。同义词可以是最大长度为 64 个字符的任何字符串。

      ![定义实体的截屏](images/define_entity.png)

    - **模式**字段允许您为实体值定义特定模式。模式**必须**在字段中作为正则表达式输入。
  

      ![定义模式实体的截屏](images/patternents1.png)
      {: #pattern-entities}

      如此示例中所示，对于实体 *ContactInfo*，可以按如下所示定义电话、电子邮件和 Web 站点值的模式：
      - 电话
        - `localPhone`：`(\d{3})-(\d{4})`，例如 426-4968
        - `fullUSphone`：`(\d{3})-(\d{3})-(\d{4})`，例如 800-426-4968
        - `internationalPhone`：`^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`，例如 +44 1962 815000
      - `email`：`\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b`，例如 name@ibm.com
      - `website`：`(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`，例如 https://www.ibm.com

      通常在使用模式实体时，必须从对话树中将与模式匹配的文本存储在上下文变量（或操作变量）中。

      假设您要请求用户输入其电子邮件地址。对话节点条件将包含类似于 `@contactInfo:email` 的条件。为了将用户输入的电子邮件分配为上下文变量，可以使用以下语法来捕获对话节点的响应部分中的模式匹配：

      ```json
      {
          "context" : {
            "email": "<? @contactInfo.literal ?>"
          }
      }
      ```
      {: screen}
      {: #capture-group}

      *捕获组* - 对于正则表达式，模式中用一对标准圆括号括起的任何部分都会作为组进行捕获。例如，实体值 `fullUSphone` 包含三个捕获的组：

        - `(\d{3})` - 美国区号
        - `(\d{3})` - 前缀
        - `(\d{4})` - 线路号码

      例如，如果您希望 {{site.data.keyword.conversationshort}} 服务询问用户的电话号码，然后在响应中仅使用其提供的号码中的区号，那么分组会非常有用。

      为了将用户输入的区号分配为上下文变量，可以使用以下语法来捕获对话节点的响应部分中的组匹配：

        ```json
        {
            "context" : {
            "area_code": "<? @fullUSphone.groups[1] ?>"
            }
        }
        ```
       {: screen}

      有关在对话运行时使用捕获组的其他信息，请参阅[在上下文变量中存储模式实体值](dialog-overview-context-groups.html)。

      {{site.data.keyword.conversationshort}} 服务使用的模式匹配引擎具有一些必需的语法限制，目的是避免使用其他正则表达式引擎时可能发生的性能问题。
        - 实体模式不能包含：
          - 肯定重复（例如，`x*+`）
          - 反向引用（例如，`\g1`）
          - 条件分支（例如，`(?(cond)true)`)
        - 模式实体以 Unicode 字符开头或结尾，并且包含字边界（例如 `\bš\b`）时，模式匹配与字边界无法正确匹配。在此示例中，对于输入 `š zkouška`，匹配会返回 `Group 0: 6-7 š` (`š zkou`_**`š`**_`ka`)，而不是返回正确的 `Group 0: 0-1 š` (_**`š`**_ `zkouška`)。

      正则表达式引擎以松散方式基于 Java 正则表达式引擎。如果尝试通过 API 或 {{site.data.keyword.conversationshort}} 服务工具 UI 上传不支持的模式，那么 {{site.data.keyword.conversationshort}} 服务会生成错误。

1.  单击**添加值**并重复该过程以添加更多实体值。

1.  添加实体值完成后，选择 ![关闭箭头](images/close_arrow.png) 以完成创建实体的操作。

### 结果

您创建的实体将添加到**实体**选项卡，然后系统开始对新数据进行自我培训。

## 编辑实体

可以单击列表中的任何实体以将其打开进行编辑。可以重命名或删除实体，也可以添加、编辑或删除值、同义词或模式。

> **注**：如果将实体类型从`同义词`更改为`模式`，或者从“模式”更改为“同义词”，那么将转换现有值，但这些值可能不会像原来那样有用。

## 搜索实体

使用“搜索”功能查找实体名称、值和同义词。

1.  选择导航栏中的**实体**选项卡，然后选择*我的实体*。

    ![“实体”选项卡概览图](images/entity_oview.png)

    **注**：无法搜索系统实体。

1.  选择“搜索”图标：![“搜索”图标](images/search_icon.png)

1.  输入搜索项或搜索短语。

    ![实体搜索项](images/searchent_1.png)

    **注**：第一次搜索时，系统会创建索引；您可能会看到一条消息，要求您稍候，系统正在对内容建立索引。

### 结果

这将显示包含搜索项的实体以及相应的示例。选择任一结果以将其打开进行编辑。

  ![实体搜索返回内容](images/searchent_2.png)

## 导入实体

如果有大量实体，那么您可能会发现通过逗号分隔值 (CSV) 文件导入这些实体比通过 {{site.data.keyword.conversationshort}} 工具逐个对其定义更容易。

1.  将实体收集到 CSV 文件中，或将其从电子表格导出为 CSV 文件。文件中每一行的必需格式如下所示：

    ```
    <entity>,<value>,<synonyms>
    ```
    {: screen}

    其中，&lt;entity&gt; 为实体的名称，&lt;value&gt; 为实体的值，&lt;synonyms&gt; 为该值的同义词的逗号分隔列表。

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {: screen}

    导入 CSV 文件也支持模式。使用 `/` 包装的任何字符串都会视为模式（而不是同义词）。

    ```
    ContactInfo,localPhone,/(\d{3})-(\d{4})/
    ContactInfo,fullUSphone,/(\d{3})-(\d{3})-(\d{4})/
    ContactInfo,internationalPhone,/^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$/
    ContactInfo,email,/\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b/
    ContactInfo,website,/(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
    ```
    {: screen}

        保存的 CSV 文件为 UTF-8 编码且无字节顺序标记 (BOM)。最大 CSV 文件大小为 10 MB。如果 CSV 文件大于此值，请考虑将其拆分成多个文件，然后分别导入这些文件。在 {{site.data.keyword.conversationshort}} 工具中，打开工作空间，然后单击**实体**选项卡。
    {: tip}

1.  单击 ![导入](images/importGA.png)，然后拖动文件，或进行浏览以从计算机中选择文件。文件经验证并导入后，系统会开始针对新数据进行自我培训。

### 结果

可以在“实体”选项卡上查看导入的实体。您可能需要刷新页面才能看到新实体。

## 导出实体
{: #export_entities}

可以将多个实体导出到一个 CSV 文件中，这样随后可以将这些实体导入并复用于其他 {{site.data.keyword.conversationshort}} 应用程序。

导出 CSV 文件支持模式。使用 `/` 包装的任何字符串都会视为模式（而不是同义词）。
{: tip}

1.  选择所需的实体，然后选择**导出**。

    ![“导出实体”按钮](images/ExportEntity.png)

## 删除实体
{: #delete_entities}

可以选择多个实体进行删除。

**重要信息**：通过删除实体，还将删除所有关联的值、同义词或模式，并且以后无法再检索这些项。所有引用这些实体或值的对话节点都必须手动更新为不再引用已删除的内容。

1.  选择所需的实体，然后选择**删除**。

    ![“删除实体”按钮](images/DeleteEntity.png)

## 启用系统实体
{: #enable_system_entities}

{{site.data.keyword.conversationshort}} 服务提供了许多*系统实体*，这些是可用于任何应用程序的常见实体。通过启用系统实体，可以使用在许多用例中通用的培训数据来快速填充工作空间。

可以使用系统实体来识别其所表示的对象类型的大范围值。例如，`@sys-number` 系统实体与任何数字值匹配，包括整数、小数部分，甚至包括写出为字的数字。

系统实体是集中维护的，因此任何更新都会自动可用。您无法修改系统实体。

1.  在“实体”选项卡上，单击**系统实体**。

    ![“系统实体”选项卡的截屏 ](images/system_entities_1.png)

1.  浏览系统实体的列表以选择对您的应用程序有用的系统实体。
    - 要查看有关系统实体的更多信息（包括匹配输入的示例），请单击列表中的实体。
    - 有关可用系统实体的详细信息，请参阅[系统实体](system-entities.html)。

1.  单击要启用或禁用的系统实体旁边的切换开关。

### 结果

启用系统实体后，{{site.data.keyword.conversationshort}} 服务就会开始重新培训。培训完成后，即可以使用实体。
