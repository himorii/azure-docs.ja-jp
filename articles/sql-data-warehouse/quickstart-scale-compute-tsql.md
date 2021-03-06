---
title: 'クイックスタート: Azure SQL Data Warehouse のコンピューティングのスケールアウト - T-SQL | Microsoft Docs'
description: DWU を調整してコンピューティング リソースをスケーリングする T-SQL コマンド。
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: ''
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/16/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 1591192c72f5bf201dbbef80acc5895c8324fca4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-using-t-sql"></a>クイックスタート: T-SQL を使用して Azure SQL Data Warehouse のコンピューティングをスケーリングする

T-SQL と SQL Server Management Studio (SSMS) を使用して、Azure SQL Data Warehouse のコンピューティングをスケーリングします。 [コンピューティングをスケールアウト](sql-data-warehouse-manage-compute-overview.md)してパフォーマンスを向上させます。または、コンピューティングをスケールバックしてコストを削減します。 

Azure サブスクリプションをお持ちでない場合は、開始する前に[無料](https://azure.microsoft.com/free/)アカウントを作成してください。

## <a name="before-you-begin"></a>開始する前に

最新バージョンの [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) をダウンロードしてインストールします。

ここでは、[ポータルでの作成と接続に関するクイックスタート](create-data-warehouse-portal.md)を完了していることを前提としています。 作成と接続に関するクイックスタートを完了すると、接続方法がわかります。**mySampleDataWarehouse** という名前のデータ ウェアハウスの作成、クライアントによるサーバーへのアクセスを可能にするファイアウォール規則の作成、インストールが完了します。
 
## <a name="create-a-data-warehouse"></a>データ ウェアハウスの作成

[ポータルでの作成と接続に関するクイックスタート](create-data-warehouse-portal.md)を使用して、**mySampleDataWarehouse** という名前のデータ ウェアハウスを作成します。 クイックスタートを完了して、ファイアウォール規則があり、SQL Server Management Studio 内からデータ ウェアハウスに接続できることを確認します。

## <a name="connect-to-the-server-as-server-admin"></a>サーバー管理者としてサーバーに接続する

このセクションでは、[SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) を使って、Azure SQL Server に対する接続を確立します。

1. SQL Server Management Studio を開きます。

2. **[サーバーへの接続]** ダイアログ ボックスで、次の情報を入力します。

   | Setting       | 推奨値 | [説明] | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | サーバーの種類 | データベース エンジン | この値は必須です |
   | サーバー名 | 完全修飾サーバー名 | たとえば、**mynewserver-20171113.database.windows.net** です。 |
   | 認証 | パブリック | このチュートリアルで構成した認証の種類は "SQL 認証" のみです。 |
   | ログイン | サーバー管理者アカウント | これは、サーバーの作成時に指定したアカウントです。 |
   | パスワード | サーバー管理者アカウントのパスワード | これは、サーバーの作成時に指定したパスワードです。 |

    ![[サーバーへの接続]](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. **[接続]**をクリックします。 SSMS でオブジェクト エクスプローラー ウィンドウが開きます。 

5. オブジェクト エクスプローラーで、**[データベース]** を展開します。 **mySampleDatabase** を展開して、新しいデータベースのオブジェクトを表示します。

    ![データベース オブジェクト](media/create-data-warehouse-portal/connected.png) 

## <a name="view-service-objective"></a>サービス目標の表示
サービス目標設定には、データ ウェアハウスの Data Warehouse ユニットの数が含まれています。 

データ ウェアハウスの現在の Data Warehouse ユニットを表示するには:

1. **mynewserver-20171113.database.windows.net** への接続で、**System Databases** を展開します。
2. **master** を右クリックし、**[新しいクエリ]** を選択します。 新しいクエリ ウィンドウが開きます。
3. 次のクエリを実行して、sys.database_service_objectives 動的管理ビューから選択します。 

    ```sql
    SELECT
        db.name [Database]
    ,   ds.edition [Edition]
    ,   ds.service_objective [Service Objective]
    FROM
        sys.database_service_objectives ds
    JOIN
        sys.databases db ON ds.database_id = db.database_id
    WHERE 
        db.name = 'mySampleDataWarehouse'
    ```

4. 次の結果は、**mySampleDataWarehouse** のサービス目標が DW400 であることを示しています。 

    ![現在の DWU の表示](media/quickstart-scale-compute-tsql/view-current-dwu.png)


## <a name="scale-compute"></a>コンピューティングのスケーリング
SQL Data Warehouse では、Data Warehouse ユニットを調整してコンピューティング リソースを増減させることができます。 [ポータルでの作成と接続](create-data-warehouse-portal.md)では、**mySampleDataWarehouse** を作成し、それを 400 DWU で初期化しました。 次の手順では、**mySampleDataWarehouse** の DWU を調整します。

Data Warehouse ユニットを変更するには:

1. **master** を右クリックし、**[新しいクエリ]** を選択します。
2. [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) T-SQL ステートメントを使用して、サービス目標を変更します。 次のクエリを実行して、サービス目標を DW300 に変更します。 

```Sql
ALTER DATABASE mySampleDataWarehouse
MODIFY (SERVICE_OBJECTIVE = 'DW300')
;
```

## <a name="check-data-warehouse-state"></a>データ ウェアハウスの状態の確認

データ ウェアハウスが一時停止すると、T-SQL で接続することはできません。 データ ウェアハウスの現在の状態を確認する場合は、PowerShell コマンドレットを使用できます。 例については、[データ ウェアハウスの状態の確認 - Powershell](quickstart-scale-compute-powershell.md#check-data-warehouse-state) に関するページを参照してください。 

## <a name="check-operation-status"></a>操作の状態の確認

SQL Data Warehouse に対するさまざまな管理操作に関する情報を返すには、[sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) DMV で次のクエリを実行します。 たとえば、操作と操作の状態 (IN_PROGRESS または COMPLETED) が返されます。

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```


## <a name="next-steps"></a>次の手順
ここでは、データ ウェアハウスのコンピューティングをスケーリングする方法について学習しました。 Azure SQL Data Warehouse の詳細については、データの読み込みに関するチュートリアルを参照してください。

> [!div class="nextstepaction"]
>[SQL Data Warehouse にデータを読み込む](load-data-from-azure-blob-storage-using-polybase.md)
