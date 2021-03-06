---
title: Azure Automation のスケジュール
description: Automation のスケジュールは、Azure Automation の Runbook を自動的に開始するためのスケジュール設定に使用します。 特定の時刻または定期的なスケジュールで自動的に Runbook を開始できるように スケジュールを作成および管理する方法について説明します。
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: ae7378c6d0f85e71c9522a6678adcb1f2e4b01f1
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Azure Automation の Runbook をスケジュール設定する
指定の時刻に開始するように Azure Automation の Runbook をスケジュール設定するには、Runbook を 1 つ以上のスケジュールにリンクします。 Azure Portal の Runbook では、1 回だけ実行するようにスケジュールを構成することも、時間または日単位で繰り返すスケジュールを指定することもできます。 さらに、週単位、月単位、特定の曜日や日にち、または月の特定の日のスケジュールも指定できます。  1 つの Runbook を複数のスケジュールにリンクし、1 つのスケジュールを複数の Runbook にリンクすることができます。

> [!NOTE]
> スケジュールは、現時点では Azure Automation DSC 構成をサポートしていません。
> 
> 

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell コマンドレット
Azure Automation から Windows PowerShell を使用してスケジュールを作成したり管理したりするには、以下の表に示したコマンドレットを使用します。 これらは、 [Azure PowerShell モジュール](/powershell/azure/overview)の一部として出荷されます。

| コマンドレット | [説明] |
|:--- |:--- |
| **Azure Resource Manager コマンドレット** | |
| [Get AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |スケジュールを取得します。 |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |新しいスケジュールを作成します。 |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |スケジュールを削除します。 |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |既存のスケジュールのプロパティを設定します。 |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |スケジュール済みの Runbook を取得します。 |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Runbook をスケジュールに関連付けます。 |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Runbook とスケジュールの関連付けを解除します。 |
| **Azure Service Management のコマンドレット** | |
| [Get-AzureAutomationSchedule](/powershell/module/azure/get-azureautomationschedule?view=azuresmps-3.7.0) |スケジュールを取得します。 |
| [New-AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) |新しいスケジュールを作成します。 |
| [Remove-AzureAutomationSchedule](/powershell/module/azure/remove-azureautomationschedule?view=azuresmps-3.7.0) |スケジュールを削除します。 |
| [Set-AzureAutomationSchedule](/powershell/module/azure/set-azureautomationschedule?view=azuresmps-3.7.0) |既存のスケジュールのプロパティを設定します。 |
| [Get-AzureAutomationScheduledRunbook](/powershell/module/azure/get-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |スケジュール済みの Runbook を取得します。 |
| [Register-AzureAutomationScheduledRunbook](/powershell/module/azure/register-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Runbook をスケジュールに関連付けます。 |
| [Unregister-AzureAutomationScheduledRunbook](/powershell/module/azure/unregister-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Runbook とスケジュールの関連付けを解除します。 |

## <a name="creating-a-schedule"></a>スケジュールを作成する
Runbook の新しいスケジュールは、Azure Portal または Windows PowerShell で作成できます。 Azure クラシック ポータルまたは Azure ポータルを使用して Runbook をスケジュールにリンクするときに新しいスケジュールを作成するというオプションもあります。

> [!NOTE]
> Azure Automation は、スケジュール済みの新しいジョブの実行時に Automation アカウントの最新のモジュールを使用します。  Runbook およびそれらが自動化するプロセスに影響が及ばないようにするには、テスト専用の Automation アカウントを使用して、スケジュールがリンクされている Runbook を最初にテストする必要があります。  これにより、スケジュール済みの Runbook が引き続き正しく動作するかが検証されます。動作しない場合は、更新された Runbook バージョンを運用環境に移行する前に、トラブルシューティングを実行して必要な変更を適用できます。  
>  **[モジュール]** の [[Azure モジュールの更新]](automation-update-azure-modules.md) オプションを選択してモジュールを手動で更新しない限り、Automation アカウントで新しいバージョンのモジュールが自動的に取得されることはありません。 
>  

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Azure ポータルで新しいスケジュールを作成するには
1. Azure Portal の Automation アカウントから、左側の **[共有リソース]** セクションで **[スケジュール]** を選択します。 
2. ページ上部の **[スケジュールの追加]** をクリックします。
4. **[新しいスケジュール]** ウィンドウで、新しいスケジュールの**名前**と、必要に応じて**説明**を入力します。
5. スケジュールを 1 回だけ実行するか、繰り返し実行するかを、**[1 回]** または **[繰り返し]** から選択します。  **[1 回]** を選択した場合は、**[開始時刻]** を指定し、**[作成]** をクリックします。  **[繰り返し]** を選択した場合は、**[開始時刻]** を指定し、Runbook を繰り返す頻度を **[時間]**、**[日]**、**[週]**、または **[月]** から選択します。  ドロップダウン リストで **[週]** または **[月]** を選択した場合は、ウィンドウに **[Recurrence option]\(繰り返しのオプション\)** が表示されます。選択すると **[Recurrence option]\(繰り返しのオプション\)** ウィンドウが表示され、**[週]** を選択した場合は曜日を選択できます。  **[月]** を選択した場合は、**[平日]** を選択するか、カレンダーで月の特定の日を選択できます。最後に、月の最終日に実行するかどうかを選択し、**[OK]** をクリックします。   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Windows PowerShell で新しいスケジュールを作成するには
クラシック Runbook 用に Azure Automation の新しいスケジュールを作成するには、[New-AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) コマンドレットを使用します。Azure ポータルの Runbook 用には、[New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) コマンドレットを使用します。 スケジュールの開始時刻と実行の頻度を指定する必要があります。

次のサンプル コマンドは、毎月 15 日と 30 日に実行されるスケジュールを Azure Resource Manager コマンドレットで作成する方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

次のサンプル コマンドは、2015 年 1 月 20 日から毎日午後 3 時 30 分に実行される新しいスケジュールを Azure サービス管理コマンドレットで作成する方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>スケジュールを Runbook にリンクする
1 つの Runbook を複数のスケジュールにリンクし、1 つのスケジュールを複数の Runbook にリンクすることができます。 Runbook にパラメーターがある場合は、その値を指定できます。 必須のパラメーターについては必ず値を指定してください。オプションのパラメーターについては値の指定は任意です。  このスケジュールで Runbook が開始されるたびに、これらの値が使用されます。  同じ Runbook を別のスケジュールに関連付けて、異なるパラメーター値を指定することができます。

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Azure ポータルで Runbook にスケジュールをリンクするには
1. Azure Portal の Automation アカウントから、左側の **[プロセス オートメーション]** セクションで **[Runbook]** を選択します。
2. Runbook の名前をクリックして、スケジュールを設定します。
3. 現在、Runbook がスケジュールにリンクされていない場合は、新しいスケジュールを作成するオプション、または既存のスケジュールにリンクするオプションが表示されます。  
4. Runbook にパラメーターがある場合は、**[実行設定を変更する (既定: Azure)]** オプションを選択すると **[パラメーター]** ウィンドウが表示されるので、必要に応じて情報を入力することができます。  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Windows PowerShell で Runbook にスケジュールをリンクするには
クラシック Runbook にスケジュールをリンクするには [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) を使用します。Azure ポータルの Runbook の場合は、[Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) コマンドレットを使用します。  Parameters パラメーターを使用して、Runbook のパラメーターに値を指定できます。 パラメーター値を指定する方法の詳細については、「[Azure Automation での Runbook を開始する](automation-starting-a-runbook.md)」をご覧ください。

次のサンプル コマンドは、パラメーターを使用して Azure Resource Manager コマンドレットでスケジュールを Runbook にリンクする方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
次のサンプル コマンドは、パラメーターを使用して Azure サービス管理コマンドレットでスケジュールをリンクする方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>スケジュールを無効にする
スケジュールを無効にすると、そのスケジュールにリンクされた Runbook はいずれもそのスケジュールに基づいて実行できなくなります。 スケジュールは手動で無効にしたり、頻度が指定されているスケジュールの有効期限を、作成時に設定したりすることができます。 有効期限の時刻になると、スケジュールは無効になります。

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure ポータルからスケジュールを無効にするには
1. Azure Portal の Automation アカウントから、左側の **[共有リソース]** セクションで **[スケジュール]** を選択します。
2. スケジュールの名前をクリックして、その詳細ウィンドウを開きます。
3. **有効** を **いいえ** に変更します

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Windows PowerShell でスケジュールを無効にするには
クラシック Runbook 用の既存のスケジュールのプロパティを変更するには、[Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) コマンドレットを使用します。Azure ポータルの Runbook 用には、[Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) コマンドレットを使用します。 To disable the schedule, specify **false** for the **IsEnabled** parameter.

次のサンプル コマンドは、Azure Resource Manager コマンドレットで Runbook のスケジュールを無効にする方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

次のサンプル コマンドは、Azure サービス管理コマンドレットでスケジュールを無効にする方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>次の手順
* Azure Automation の Runbook の使用を開始するには、「 [Azure Automation での Runbook の開始](automation-starting-a-runbook.md) 

