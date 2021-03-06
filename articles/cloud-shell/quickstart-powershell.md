﻿---
title: Azure Cloud Shell (プレビュー) の PowerShell のクイック スタート | Microsoft Docs
description: Cloud Shell の PowerShell のクイックスタート
services: Azure
documentationcenter: ''
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: damaerte
ms.openlocfilehash: efee0842a2fca2afac28f179bba07c3b6682ee57
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell-preview"></a>Azure Cloud Shell (プレビュー) の PowerShell のクイックスタート

このドキュメントでは、[Azure Portal](https://aka.ms/PSCloudPreview) で Cloud Shell の PowerShell を使う方法について詳しく説明します。

> [!NOTE]
> [Azure Cloud Shell の Bash](quickstart.md) のクイックスタートもあります。

## <a name="start-cloud-shell"></a>Cloud Shell の起動

1. Azure Portal 上部のナビゲーションの **[Cloud Shell]** ボタンをクリックします

  ![](media/quickstart-powershell/shell-icon.png)

2. ドロップダウンで PowerShell 環境を選ぶと、Azure ドライブ `(Azure:)` になります

  ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>PowerShell コマンドを実行する

次のように、通常の PowerShell コマンドを Cloud Shell で実行します。

```PowerShell
PS Azure:\> Get-Date
Monday, September 25, 2017 08:55:09 AM

PS Azure:\> Get-AzureRmVM -Status

ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Azure リソース間を移動する	

 1. サブスクリプションを一覧表示します

    ``` PowerShell
    PS Azure:\> dir
    ```

 2. `cd` を実行して目的のサブスクリプションに移動します

    ``` PowerShell
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. 現在のサブスクリプションに含まれるすべての Azure リソースを表示します
 
    「`dir`」と入力して、Azure リソースの複数のビューを一覧表示します。
 
    ``` PowerShell
    PS Azure:\MySubscriptionName> dir

        Directory: azure:\MySubscriptionName

    Mode Name
    ---- ----
    +    AllResources
    +    ResourceGroups
    +    StorageAccounts
    +    VirtualMachines
    +    WebApps
     ```

### <a name="allresources-view"></a>AllResources ビュー 
`AllResources` ディレクトリで「`dir`」と入力して、Azure リソースを表示します。	
    
    PS Azure:\MySubscriptionName> dir AllResources

### <a name="explore-resource-groups"></a>リソース グループを探索する

 `ResourceGroups` ディレクトリに移動し、特定のリソース グループ内で仮想マシンを検索できます。	

``` PowerShell
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271

```
> [!NOTE]
> 2 回目に入力した「`dir`」の方が、はるかに速く表示される場合があります。
> これは、ユーザー エクスペリエンス向上のため、子項目がメモリ内にキャッシュされるためです。
ただし、`dir -Force` を使うと常に最新のデータを取得できます。

### <a name="navigate-storage-resources"></a>ストレージ リソース間を移動する	
    
`StorageAccounts` ディレクトリに入ることで、ストレージ リソース間を簡単に移動することができます
    
``` PowerShell 
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files


Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>


```

次のコマンドを接続文字列（ConnectionString）と共に使用することで、Azure ファイル共有をマウントできます。
        
``` PowerShell
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>


```

詳しくは、「[Windows で Azure ファイル共有をマウントして共有にアクセスする][azmount]」をご覧ください。

次のようにして、Azure ファイル共有の下のディレクトリ間を移動することもできます。

            
``` PowerShell
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1

    
```

### <a name="interact-with-virtual-machines"></a>仮想マシンを操作する

`VirtualMachines` ディレクトリを使うと、現在のサブスクリプションの下にあるすべての仮想マシンを確認できます。
    
``` PowerShell
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running


```

#### <a name="invoke-powershell-script-across-remote-vms"></a>リモート VM 間で PowerShell スクリプトを呼び出す

 > [!WARNING]
 > [Azure VM のリモート管理のトラブルシューティング](troubleshooting.md#troubleshooting-remote-management-of-azure-vms)に関するページを参照してください。

  MyVM1 という VM があるとすると、`Invoke-AzureRmVMCommand` を使ってリモート マシンの PowerShell スクリプト ブロックを呼び出すことができます。

  ``` Powershell
  Invoke-AzureRmVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```
  また、最初に VirtualMachines ディレクトリに移動して、次のように `Invoke-AzureRmVMCommand` を実行することもできます。

  ``` PowerShell
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzureRmVMCommand -Scriptblock{Get-ComputerInfo}
  ```
  次のような出力が表示されます。

  ``` PowerShell
  PSComputerName                                          : 65.52.28.207
  RunspaceId                                              : 2c2b60da-f9b9-4f42-a282-93316cb06fe1
  WindowsBuildLabEx                                       : 14393.1066.amd64fre.rs1_release_sec.170327-1835
  WindowsCurrentVersion                                   : 6.3
  WindowsEditionId                                        : ServerDatacenter
  WindowsInstallationType                                 : Server
  WindowsInstallDateFromRegistry                          : 5/18/2017 11:26:08 PM
  WindowsProductId                                        : 00376-40000-00000-AA947
  WindowsProductName                                      : Windows Server 2016 Datacenter
  WindowsRegisteredOrganization                           :
   ...
  ```

#### <a name="interactively-log-on-to-a-remote-vm"></a>リモート VM に対話形式でログオンする

`Enter-AzureRmVM` を使うと、Azure で実行されている VM に対話形式でログインできます。

  ``` PowerShell
  Enter-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

また、最初に `VirtualMachines` ディレクトリに移動して、次のように `Enter-AzureRmVM` を実行することもできます

  ``` PowerShell
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzureRmVM
 ```

### <a name="discover-webapps"></a>WebApps を検出する	

`WebApps` ディレクトリに入ることで、Web アプリ リソース間を簡単に移動することができます

``` PowerShell
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US



# You can use Azure cmdlets to Start/Stop your web apps
PS Azure:\MySubscriptionName\WebApps> Start-AzureRmWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

Name           State    ResourceGroup        EnabledHostNames                   Location
----           -----    -------------        ----------------                   --------
mywebapp1      Running  MyResourceGroup1     {mywebapp1.azurewebsites.net ...   West US

# Refresh the current state with -Force
PS Azure:\MySubscriptionName\WebApps> dir -Force

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Running  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US

```

## <a name="ssh"></a>SSH

PowerShell Cloud Shell 上で [Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH) を利用できます。
SSH を使ってサーバーまたは VM に対する認証を行うには、Cloud Shell で公開キーと秘密キーの組を生成して、リモート マシン上の `authorized_keys` (`/home/user/.ssh/authorized_keys` など) に公開キーを発行します。

> [!NOTE]
> `ssh-keygen` を使って SSH の公開/秘密キーを作成し、Cloud Shell でそれらを `$env:USERPROFILE\.ssh` に発行できます。	

### <a name="using-a-custom-profile-to-persist-git-and-ssh-settings"></a>カスタム プロファイルを使って Git と SSH の設定を保持する

サインアウトするとセッションは保持されないので、`$env:USERPROFILE\.ssh` ディレクトリを `CloudDrive` に保存するか、Cloud Shell の起動時にシンボリック リンクを作成します。
CloudDrive へのシンボリック リンクを作成するには、profile.ps1 に次のコード スニペットを追加します。

``` PowerShell
# Check if the .ssh directory exists
if( -not (Test-Path $home\CloudDrive\.ssh)){
    mkdir $home\CloudDrive\.ssh
}

# .ssh path relative to this script
$script:sshFolderPath = Join-Path $PSScriptRoot .ssh

# Create a symlink to .ssh in user's $home
if(Test-Path $script:sshFolderPath){
   if(-not (Test-Path (Join-Path $HOME .ssh ))){
        New-Item -ItemType SymbolicLink -Path $HOME -Name .ssh -Value $script:sshFolderPath
   }
}

```

### <a name="using-ssh"></a>SSH の使用

[こちら](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-powershell)の説明に従い、AzureRM コマンドレットを使って新しい VM 構成を作成します。
`New-AzureRmVM` を呼び出してデプロイを始める前に、SSH 公開キーを VM の構成に追加します。
新しく作成される VM には `~\.ssh\authorized_keys` にある公開キーが含まれ、それにより VM への資格情報不要の SSH セッションが有効になります。

``` PowerShell

# Create VM config object - $vmConfig using instructions on linked page above

# Generate SSH keys in Cloud Shell
ssh-keygen -t rsa -b 2048 -f $HOME\.ssh\id_rsa 

# Ensure VM config is updated with SSH keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmConfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

# Create a virtual machine
New-AzureRmVM -ResourceGroupName <yourResourceGroup> -Location <vmLocation> -VM $vmConfig

# SSH to the VM
ssh azureuser@MyVM.Domain.Com

```


## <a name="list-available-commands"></a>使用可能なコマンドを一覧表示する

`Azure` ドライブで、「`Get-AzureRmCommand`」と入力してコンテキスト固有の Azure コマンドを取得します。

または、`Get-Command *azurerm* -Module AzureRM.*` を使うと、使用できる Azure コマンドをいつでも検索できます。

## <a name="install-custom-modules"></a>カスタム モジュールをインストールする

`Install-Module` を実行して、[PowerShell ギャラリー][gallery]からモジュールをインストールできます。

## <a name="get-help"></a>Get-Help

Azure Cloud Shell の PowerShell についての情報を取得するには、「`Get-Help`」と入力します。

``` PowerShell
PS Azure:\> Get-Help
```

特定のコマンドの場合は、`Get-Help` の後にコマンドレットを指定します。

``` PowerShell
PS Azure:\> Get-Help Get-AzureRmVM
```

## <a name="use-azure-files-to-store-your-data"></a>Azure Files を使ってデータを保存する

たとえば `helloworld.ps1` といったスクリプトを作成して `CloudDrive` に保存し、それを異なるシェル セッションで使うことができます。

``` PowerShell
cd C:\users\ContainerAdministrator\CloudDrive
PS C:\users\ContainerAdministrator\CloudDrive> vim .\helloworld.ps1
# Add the content, such as 'Hello World!'
PS C:\users\ContainerAdministrator\CloudDrive> .\helloworld.ps1
Hello World!
```

Cloud Shell で PowerShell を次に使用するときは、`helloworld.ps1` ファイルが Azure Files 共有をマウントした `CloudDrive` ディレクトリにあります。

## <a name="use-custom-profile"></a>カスタム プロファイルを使う

PowerShell プロファイル `profile.ps1` または `Microsoft.PowerShell_profile.ps1` を作成することで、PowerShell 環境をカスタマイズできます。 それを `CloudDrive` に保存し、Cloud Shell の起動時にすべての PowerShell セッションで読み込むことができるようにします。

プロファイルの作成方法については、「[About Profiles][profile]」(プロファイルについて) を参照してください。

## <a name="use-git"></a>Git を使う

Cloud Shell で Git リポジトリを複製するには、[個人用アクセス トークン][githubtoken]を作成し、それをユーザー名として使う必要があります。 トークンを作成した後は、次のようにしてリポジトリを複製します。

 ``` PowerShell
  git clone https://<your-access-token>@github.com/username/repo.git

```
ユーザーがサインアウトするか、セッションがタイムアウトすると、Cloud Shell のセッションは保持されないので、次のログオン時に Git 構成ファイルは存在しません。 Git の構成を保持するには、.gitconfig を `CloudDrive` に保存してそれをコピーするか、または Cloud Shell が起動されるときにシンボリック リンクを作成します。 `CloudDrive` へのシンボリック リンクを作成するには、profile.ps1 で次のコード スニペットを使います。

 ``` PowerShell
 
# .gitconfig path relative to this script
$script:gitconfigPath = Join-Path $PSScriptRoot .gitconfig

# Create a symlink to .gitconfig in user's $home
if(Test-Path $script:gitconfigPath){

    if(-not (Test-Path (Join-Path $home .gitconfig ))){
         New-Item -ItemType SymbolicLink -Path $home -Name .gitconfig -Value $script:gitconfigPath
    }
}

```
## <a name="exit-the-shell"></a>シェルを終了します。

「`exit`」と入力してセッションを終了します。

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
