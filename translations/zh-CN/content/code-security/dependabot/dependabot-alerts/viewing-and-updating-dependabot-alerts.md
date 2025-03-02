---
title: 查看和更新 Dependabot 警报
intro: '如果 {% data variables.product.product_name %} 发现项目中存在不安全的依赖项，您可以在仓库的 Dependabot 警报选项卡中查看详细信息。 然后，您可以更新项目以解决或忽略警报。'
redirect_from:
  - /articles/viewing-and-updating-vulnerable-dependencies-in-your-repository
  - /github/managing-security-vulnerabilities/viewing-and-updating-vulnerable-dependencies-in-your-repository
  - /code-security/supply-chain-security/viewing-and-updating-vulnerable-dependencies-in-your-repository
  - /code-security/supply-chain-security/managing-vulnerabilities-in-your-projects-dependencies/viewing-and-updating-vulnerable-dependencies-in-your-repository
permissions: 'Repository administrators and organization owners can view and update dependencies, as well as users and teams with explicit access.'
shortTitle: 查看 Dependabot 警报
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: how_to
topics:
  - Dependabot
  - Security updates
  - Alerts
  - Dependencies
  - Pull requests
  - Repositories
---

{% data reusables.dependabot.beta-security-and-version-updates %}
{% data reusables.dependabot.enterprise-enable-dependabot %}

仓库的 {% data variables.product.prodname_dependabot_alerts %} 选项卡列出所有打开和关闭的 {% data variables.product.prodname_dependabot_alerts %}{% ifversion fpt or ghec or ghes > 3.2 %} 以及对应的 {% data variables.product.prodname_dependabot_security_updates %}{% endif %}。 可以{% ifversion fpt or ghec or ghes > 3.4 or ghae-issue-5638 %} 按程序包、生态系统或清单筛选警报。 您可以{% endif %} 对警报列表进行排序，单击特定警报以获取更多详细信息。 {% ifversion dependabot-bulk-alerts %}您还可以逐个关闭或重新打开警报，也可以一次选择多个警报。{% else %}您还可以关闭或重新打开警报。 {% endif %} 更多信息请参阅“[关于 {% data variables.product.prodname_dependabot_alerts %}](/code-security/supply-chain-security/about-alerts-for-vulnerable-dependencies)”。

{% ifversion fpt or ghec or ghes > 3.2 %}
您可以为使用 {% data variables.product.prodname_dependabot_alerts %} 和依赖关系图的任何仓库启用自动安全更新。 更多信息请参阅“[关于 {% data variables.product.prodname_dependabot_security_updates %}](/code-security/supply-chain-security/managing-vulnerabilities-in-your-projects-dependencies/about-dependabot-security-updates)“。
{% endif %}

{% ifversion fpt or ghec or ghes > 3.2 %}
## 关于仓库中有漏洞的依赖项的更新

{% data variables.product.product_name %} 在检测到您的代码库正在使用具有已知安全风险的依赖项时会生成 {% data variables.product.prodname_dependabot_alerts %}。 对于启用了 {% data variables.product.prodname_dependabot_security_updates %} 的仓库，当 {% data variables.product.product_name %} 在默认分支中检测到有漏洞的依赖项时，{% data variables.product.prodname_dependabot %} 会创建拉取请求来修复它。 拉取请求会将依赖项升级到避免漏洞所需的最低安全版本。

每个 {% data variables.product.prodname_dependabot %} 警报都有一个唯一的数字标识符，{% data variables.product.prodname_dependabot_alerts %} 选项卡列出了每个检测到的漏洞的警报。 旧版 {% data variables.product.prodname_dependabot_alerts %} 按依赖项对漏洞进行分组，并为每个依赖项生成一个警报。 如果导航到旧版 {% data variables.product.prodname_dependabot %} 警报，则会将您重定向到为该包筛选的 {% data variables.product.prodname_dependabot_alerts %} 选项卡。 {% endif %}

{% ifversion fpt or ghec or ghes > 3.4 or ghae-issue-5638 %}
You can filter and sort {% data variables.product.prodname_dependabot_alerts %} using a variety of filters and sort options available on the user interface. For more information, see "[Prioritizing {% data variables.product.prodname_dependabot_alerts %}](#prioritizing-across--data-variablesproductprodname_dependabot_alerts-)" below.

## Prioritizing {% data variables.product.prodname_dependabot_alerts %}

{% data variables.product.company_short %} helps you prioritize fixing {% data variables.product.prodname_dependabot_alerts %}. {% ifversion dependabot-most-important-sort-option %} By default, {% data variables.product.prodname_dependabot_alerts %} are sorted by importance. The "Most important" sort order helps you prioritize which {% data variables.product.prodname_dependabot_alerts %} to focus on first. 警报根据其潜在影响、可操作性和相关性进行排名。 我们的优先级计算不断改进，包括 CVSS 分数、依赖范围以及是否为警报找到有漏洞的函数调用等因素。

![带有"最重要"排序”的“排序”下拉列表的屏幕截图](/assets/images/help/dependabot/dependabot-alerts-sort-dropdown.png)
{% endif %}

{% data reusables.dependabot.dependabot-alerts-filters %}

In addition to the filters available via the search bar, you can sort and filter {% data variables.product.prodname_dependabot_alerts %} using the dropdown menus at the top of the alert list. The search bar also allows for full text searching of alerts and related security advisories. You can search for part of a security advisory name or description to return the alerts in your repository that relate to that security advisory. For example, searching for `yaml.load() API could execute arbitrary code` will return {% data variables.product.prodname_dependabot_alerts %} linked to "[PyYAML insecurely deserializes YAML strings leading to arbitrary code execution](https://github.com/advisories/GHSA-rprw-h62v-c2w7)" as the search string appears in the advisory description.

{% endif %}

{% ifversion dependabot-bulk-alerts %}
  ![{% data variables.product.prodname_dependabot_alerts %} 选项卡中过滤器和排序菜单的屏幕截图](/assets/images/help/graphs/dependabot-alerts-filters-checkbox.png){% elsif ghes = 3.5 %}
You can select a filter in a dropdown menu at the top of the list, then click the filter that you would like to apply. ![Screenshot of the filter and sort menus in the {% data variables.product.prodname_dependabot_alerts %} tab](/assets/images/enterprise/3.5/dependabot/dependabot-alerts-filters.png){% endif %}

{% ifversion dependabot-alerts-development-label %}
## 支持的生态系统和依赖范围清单

{% data reusables.dependabot.dependabot-alerts-dependency-scope %}

列为开发依赖项的包的警报在 {% data variables.product.prodname_dependabot_alerts %} 页上标有 `Development` 标签，并且还可以通过 `scope` 筛选器进行筛选。

![在警报列表中显示"开发"标签的屏幕截图](/assets/images/help/repository/dependabot-alerts-development-label.png)

开发范围的包上警报的警报详细信息页显示“标记”部分，其中包含 `Development` 标签。

![在警报详细信息页面中显示"标记"部分的屏幕截图](/assets/images/help/repository/dependabot-alerts-tags-section.png)

{% endif %}

{% ifversion dependabot-alerts-vulnerable-calls %}
## 关于检测对有漏洞函数的调用

{% data reusables.dependabot.vulnerable-calls-beta %}

当 {% data variables.product.prodname_dependabot %} 告诉您存储库使用有漏洞的依赖项时，您需要确定有漏洞的功能是什么，并检查是否正在使用它们。 获得此信息后，可以确定升级到依赖项的安全版本的迫切程度。

对于支持的语言，{% data variables.product.prodname_dependabot %} 会自动检测您是否使用有漏洞的函数，并为受影响的警报添加“有漏洞的调用”标签。 您可以在 {% data variables.product.prodname_dependabot_alerts %} 视图中使用此信息来更有效地分流修正工作并确定其优先级。

{% note %}

**注意：** 在测试版期间，此功能仅适用于在 2022 年 4 月 14 日*之后*创建的新 Python 公告，以及历史 Python 公告的子集。 {% data variables.product.prodname_dotcom %} 正在努力回填其他历史 Python 公告的数据，这些公告是滚动添加的。 有漏洞的调用仅在 {% data variables.product.prodname_dependabot_alerts %} 页面上突出显示。

{% endnote %}

![显示带有"有漏洞的调用"标签的警报屏幕截图](/assets/images/help/repository/dependabot-alerts-vulnerable-call-label.png)

您可以在搜索字段中使用 `has:vulnerable-calls` 筛选视图，仅显示 {% data variables.product.prodname_dependabot %} 检测到至少一次调用有漏洞函数的警报。

对于检测到有漏洞的调用的警报，警报详细信息页面将显示其他信息：

- 一个或多个代码块，显示函数的使用位置。
- 列出函数本身以及指向调用函数的行的链接的注释。

![显示警报的并带有"有漏洞的调用"标签的警报详细信息页面屏幕截图](/assets/images/help/repository/review-calls-to-vulnerable-functions.png)

更多信息请参阅下面的“[查看和修复警报](#reviewing-and-fixing-alerts)”。

{% endif %}

## 查看 {% data variables.product.prodname_dependabot_alerts %}

{% ifversion fpt or ghec or ghes > 3.4 or ghae-issue-5638 %}
{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-security %}
{% data reusables.repositories.sidebar-dependabot-alerts %}
1. Optionally, to filter alerts, select a filter in a dropdown menu then click the filter that you would like to apply. 您还可以在搜索栏中键入过滤条件。 For more information about filtering and sorting alerts, see "[Prioritizing {% data variables.product.prodname_dependabot_alerts %}](#prioritizing-across--data-variablesproductprodname_dependabot_alerts-)."
{%- ifversion dependabot-bulk-alerts %}
  ![{% data variables.product.prodname_dependabot_alerts %} 选项卡中过滤器和排序菜单的屏幕截图](/assets/images/help/graphs/dependabot-alerts-filters-checkbox.png){% else %}
![Screenshot of the filter and sort menus in the {% data variables.product.prodname_dependabot_alerts %} tab](/assets/images/enterprise/3.5/dependabot/dependabot-alerts-filters.png){% endif %}
1. 单击要查看的警报。{% ifversion dependabot-bulk-alerts %} ![Alert selected in list of alerts](/assets/images/help/graphs/click-alert-in-alerts-list-checkbox.png){% else %}
![Alert selected in list of alerts](/assets/images/enterprise/3.5/dependabot/click-alert-in-alerts-list-ungrouped.png){% endif %}

{% else %}
{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-security %}
{% data reusables.repositories.sidebar-dependabot-alerts %}
1. 单击您想要查看的警报。 ![在警报列表中选择的警报](/assets/images/help/graphs/click-alert-in-alerts-list.png)
{% endif %}

## 查看和修复警报

请务必确保所有依赖项都没有任何安全漏洞。 当 {% data variables.product.prodname_dependabot %} 发现依赖项中的漏洞{% ifversion GH-advisory-db-supports-malware %}或恶意软件{% endif %}时，应评估项目的暴露水平，并确定要采取哪些补救措施来保护应用程序。

如果依赖项有修补的版本可用，则可以生成 {% data variables.product.prodname_dependabot %} 请求，以直接从 {% data variables.product.prodname_dependabot %} 警报更新此依赖项。 如果您启用了 {% data variables.product.prodname_dependabot_security_updates %}，则拉取请求可能会在 Dependabot 警报中链接。

如果修补的版本不可用，或者您无法更新到安全版本，{% data variables.product.prodname_dependabot %} 会共享其他信息，以帮助您确定后续步骤。 单击以查看 {% data variables.product.prodname_dependabot %} 警报时，可以看到依赖项的安全通告的完整详细信息，包括受影响的功能。 然后，可以检查代码是否调用受影响的函数。 此信息可以帮助您进一步评估风险级别，并确定解决方法或是否能够接受安全公告所代表的风险。

{% ifversion dependabot-alerts-vulnerable-calls %}

对于支持的语言，{% data variables.product.prodname_dependabot %} 为您检测对有漏洞函数的调用。 当您查看标记为“有漏洞的调用”的警报时，详细信息包括函数的名称以及指向调用该函数的代码的链接。 通常，您将能够根据此信息做出决策，而无需进一步探索。

{% endif %}

### 修复有漏洞的依赖项

1. 查看警报的详细信息。 更多信息请参阅“[查看 {% data variables.product.prodname_dependabot_alerts %}](#viewing-dependabot-alerts)”（上文）。
{% ifversion fpt or ghec or ghes > 3.2 %}
1. 如果启用了 {% data variables.product.prodname_dependabot_security_updates %} ，则可能存在指向将修复依赖项的拉取请求的链接。 或者，可以单击警报详细信息页面顶部的 **创建 {% data variables.product.prodname_dependabot %} 安全更新**以创建拉取请求。 ![创建 {% data variables.product.prodname_dependabot %} 安全更新按钮](/assets/images/help/repository/create-dependabot-security-update-button-ungrouped.png)
1. （可选）如果不使用 {% data variables.product.prodname_dependabot_security_updates %}，则可以使用页面上的信息来决定要升级到的依赖项版本，并创建拉取请求以将依赖项更新到安全版本。
{% elsif ghes < 3.3 or ghae %}
1. 可以使用页面上的信息来决定要升级到的依赖项版本，并创建对清单的拉取请求或将文件锁定到安全版本。
{% endif %}
1. 当您准备好更新依赖项并解决漏洞时，合并拉取请求。

{% ifversion fpt or ghec or ghes > 3.2 %}
   {% data variables.product.prodname_dependabot %} 提出的每个拉取请求都包含可用于控制 {% data variables.product.prodname_dependabot %} 的命令的相关信息。 更多信息请参阅“[管理依赖项更新的拉取请求](/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/managing-pull-requests-for-dependency-updates#managing-dependabot-pull-requests-with-comment-commands)”。
{% endif %}

## 忽略 {% data variables.product.prodname_dependabot_alerts %}

{% tip %}

**提示：** 您只能关闭打开的警报。
{% endtip %}

如果计划大量工作来升级依赖项，或者决定不需要修复警报，则可以忽略警报。 通过忽略已评估的警报，可以更轻松地在新警报出现时对其进行分类。

1. 查看警报的详细信息。 更多信息请参阅“[查看有漏洞的依赖项](#viewing-dependabot-alerts)”（上文）。
1. 选择“Dismiss（忽略）”下拉列表，然后单击忽略警报的原因。{% ifversion reopen-dependabot-alerts %} 未修复的已忽略警报可以稍后重新打开。{% endif %}
{% ifversion dependabot-alerts-dismissal-comment %}1. Optionally, add a dismissal comment. The dismissal comment will be added to the alert timeline and can be used as justification during auditing and reporting. You can retrieve or set a comment by using the GraphQL API. The comment is contained in the `dismissComment` field. For more information, see "[{% data variables.product.prodname_dependabot_alerts %}](/graphql/reference/objects#repositoryvulnerabilityalert)" in the GraphQL API documentation.
   ![Screenshot showing how to dismiss an alert via the "Dismiss" drop-down, with the option to add a dismissal comment](/assets/images/help/repository/dependabot-alerts-dismissal-comment.png)
1. Click **Dismiss alert**.
{% else %}
   ![选择通过 "Dismiss（忽略）"下拉菜单忽略警报的原因](/assets/images/help/repository/dependabot-alert-dismiss-drop-down-ungrouped.png){% endif %}
{% ifversion dependabot-bulk-alerts %}

### 一次忽略多个警报

1. 查看打开的 {% data variables.product.prodname_dependabot_alerts %}。 更多信息请参阅“[查看 {% data variables.product.prodname_dependabot_alerts %}](/en/code-security/dependabot/dependabot-alerts/viewing-and-updating-dependabot-alerts#viewing-dependabot-alerts)”。
2. （可选）通过选择下拉菜单，然后单击要应用的筛选器来筛选警报列表。 您还可以在搜索栏中键入过滤条件。
3. 在每个警报标题的左侧，选择要忽略的警报。 ![突出显示了复选框的打开警报的屏幕截图](/assets/images/help/graphs/select-multiple-alerts.png)
4. （可选）在警报列表的顶部，选择页面上的所有警报。 ![选择的所有已打开警报的屏幕截图](/assets/images/help/graphs/select-all-alerts.png)
5. 选择“Dismiss alerts（忽略警报）”下拉列表，然后单击忽略警报的原因。 ![突出显示了“"忽略警报"下拉列表的已打开警报页面屏幕截图](/assets/images/help/graphs/dismiss-multiple-alerts.png)

{% endif %}

{% ifversion reopen-dependabot-alerts %}

## 查看和更新已关闭的警报

You can view all open alerts, and you can reopen alerts that have been previously dismissed. 无法重新打开已修复的已关闭警报。

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-security %}
{% data reusables.repositories.sidebar-dependabot-alerts %}
1. 要仅查看已关闭的警报，请单击 **Closed（已关闭）**。

   {%- ifversion dependabot-bulk-alerts %}
   ![显示"已关闭"选项的屏幕截图](/assets/images/help/repository/dependabot-alerts-closed-checkbox.png)
   {%- else %}
   ![显示"已关闭"选项的屏幕截图](/assets/images/help/repository/dependabot-alerts-closed.png)
   {%- endif %}
1. 单击要查看或更新的警报。

   {%- ifversion dependabot-bulk-alerts %}
   ![显示突出显示的 dependabot 警报的屏幕截图](/assets/images/help/repository/dependabot-alerts-select-closed-alert-checkbox.png)
   {%- else %}
   ![显示突出显示的 dependabot 警报的屏幕截图](/assets/images/help/repository/dependabot-alerts-select-closed-alert.png)   {%- endif %}
2. （可选）如果警报已消除，并且您希望重新打开它，请单击 **Reopen（重新打开）**。 无法重新打开已修复的警报。

   {% indented_data_reference reusables.enterprise.3-5-missing-feature spaces=3 %}
   ![显示"重新打开"按钮的屏幕截图](/assets/images/help/repository/reopen-dismissed-alert.png)

{% endif %}

{% ifversion dependabot-bulk-alerts %}

### 一次重新打开多个警报

1. 查看关闭的 {% data variables.product.prodname_dependabot_alerts %}。 更多信息请参阅“[查看和更新关闭的警报](/en/code-security/dependabot/dependabot-alerts/viewing-and-updating-dependabot-alerts#viewing-and-updating-closed-alerts)”（上文）。
2. 在每个警报标题的左侧，选择要重新打开的警报。 ![突出显示了复选框的已关闭警报的屏幕截图](/assets/images/help/repository/dependabot-alerts-open-checkbox.png)
3. （可选）在警报列表的顶部，选择页面上所有已关闭的警报。 ![选中了所有警报的已关闭警报的屏幕截图](/assets/images/help/graphs/select-all-closed-alerts.png)
4. 单击 **Reopen（重新打开）**以重新打开警报。 无法重新打开已修复的警报。 ![突出显示了"重新打开"按钮的已关闭警报的屏幕截图](/assets/images/help/graphs/reopen-multiple-alerts.png)

{% endif %}
