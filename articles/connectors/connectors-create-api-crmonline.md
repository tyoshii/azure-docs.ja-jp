---
title: "Dynamics CRM Online コネクタを Logic Apps に追加する | Microsoft Docs"
description: "Azure App Service を使用してロジック アプリを作成します。 Dynamics CRM Online 接続プロバイダーは、Dynamics CRM Online 上のエンティティを操作する API を提供します。"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 66fc8f7e1da55dbe6bb1dd8b8d6a535c498c1cf7
ms.openlocfilehash: a1ea6c09621aeeb1e98bbbf5edf1d5deb5e4b721


---
# <a name="get-started-with-the-dynamics-crm-online-connector"></a>Dynamics CRM Online コネクタの概要
Dynamics CRM Online に接続して、新しいレコードの作成、項目の更新などを行います。 CRM Online では、次の操作を実行できます。

* CRM Online から取得したデータに基づいてビジネス フローを構築できます。 
* レコードの削除、エンティティの取得などのアクションを使用できます。 また、これらのアクションで応答を取得すると、他のアクションから出力を使用できます。 たとえば、CRM で項目を更新するときに、Office 365 を使用して電子メールを送信できます。

このトピックでは、ロジック アプリ内で Dynamics CRM Online コネクタを使用する方法を説明し、トリガーとアクションの一覧を示します。

> [!NOTE]
> 本記事は、一般公開された Logic Apps の一般公開 (GA) を対象としています。
> 
> 

Logic Apps の詳細については、「[Logic Apps とは](../logic-apps/logic-apps-what-are-logic-apps.md)」と[ロジック アプリの作成](../logic-apps/logic-apps-create-a-logic-app.md)に関するページを参照してください。

## <a name="connect-to-dynamics-crm-online"></a>Dynamics CRM Online に接続する
ロジック アプリから任意のサービスにアクセスできるようにするには、まず、そのサービスへの "*接続*" を作成します。 接続により、ロジック アプリと別のサービスとの接続が実現します。 たとえば、Dynamics に接続するには、まず Dynamics CRM Online への "*接続*" が必要になります。 接続を作成するには、接続対象のサービスへのアクセスに通常使用する資格情報を入力します。 そのため、Dynamics の場合は、Dynamics CRM Online アカウントの資格情報を入力して接続を作成します。

### <a name="create-the-connection"></a>接続の作成
> [!INCLUDE [Steps to create a connection to Dynamics CRM Online Connection Provider](../../includes/connectors-create-api-crmonline.md)]
> 
> 

## <a name="use-a-trigger"></a>トリガーを使用する
トリガーとは、ロジック アプリで定義されたワークフローの開始に使用できるイベントです。 トリガーは、指定された間隔と頻度でサービスを "ポーリング" します。 トリガーの詳細については[こちら](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts)を参照してください。

1. ロジック アプリで「dynamics」と入力して、トリガーの一覧を取得します。  
   
    ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. **[Dynamics CRM Online - When a record is created (Dynamics CRM Online - レコードが作成されたとき)]** を選択します。 接続が既に存在する場合は、ドロップダウン リストから組織とエンティティを選択します。
   
    ![](./media/connectors-create-api-crmonline/select-organization.png)
   
    サインインを求められたら、サインインの詳細を入力して接続を作成します。 この手順については、このトピックの「[接続の作成](connectors-create-api-crmonline.md#create-the-connection)」を参照してください。 
   
   > [!NOTE]
   > この例では、レコードが作成されたときにロジック アプリが実行されます。 このトリガーの結果を確認するには、自身に電子メールを送信する別のアクションを追加してください。 たとえば、Office 365 の "*電子メールを送信する*" アクションを追加します。これにより、新しいレコードが追加されると電子メールが送信されます。 
   > 
   > 
3. **[編集]** を選択し、**[頻度]** と **[間隔]** の値を設定します。 たとえば、トリガーを使用して 15 分ごとにポーリングを実行するには、**[頻度]** を **[分]** に設定し、**[間隔]** を **15** に設定します。 
   
    ![](./media/connectors-create-api-crmonline/edit-properties.png)
4. ツール バーの左上隅にある **[保存]** を選択して変更を保存します。 ロジック アプリが保存され、場合によっては、自動的に有効になります。

## <a name="use-an-action"></a>アクションを使用する
アクションとは、ロジック アプリで定義されたワークフローによって実行される操作です。 アクションの詳細については[こちら](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts)を参照してください。

1. プラス記号を選択します。 **[アクションの追加]**、**[条件の追加]**、**[More (その他)]** のいずれかのオプションという複数の選択肢があります。
   
    ![](./media/connectors-create-api-crmonline/add-action.png)
2. **[アクションの追加]**を選択します。
3. テキスト ボックスに「dynamics」と入力して、使用可能なすべてのアクションの一覧を取得します。
   
    ![](./media/connectors-create-api-crmonline/dynamics-actions.png)
4. この例では、**[Dynamics CRM Online - Update a record (Dynamics CRM Online - レコードを更新する)]** を選択します。 接続が既に存在する場合は、**[組織名]**、**[エンティティ名]**、およびその他のプロパティを指定します。  
   
    ![](./media/connectors-create-api-crmonline/sample-action.png)
   
    接続情報の入力を求められたら、詳細を入力して接続を作成します。 これらのプロパティについては、このトピックの「[接続の作成](connectors-create-api-crmonline.md#create-the-connection)」を参照してください。 
   
   > [!NOTE]
   > この例では、CRM Online の既存のレコードを更新します。 別のトリガーからの出力を使用して、レコードを更新できます。 たとえば、SharePoint の "*既存の項目が変更されたとき*" トリガーを追加します。 次に、CRM Online の "*レコードを更新する*" アクションを追加します。このアクションは、SharePoint フィールドを使用して CRM Online 内の既存のレコードを更新します。 
   > 
   > 
5. ツール バーの左上隅にある **[保存]** を選択して変更を保存します。 ロジック アプリが保存され、場合によっては、自動的に有効になります。

## <a name="technical-details"></a>技術的な詳細
## <a name="triggers"></a>トリガー
| トリガー | 説明 |
| --- | --- |
| [レコードが作成されたとき](connectors-create-api-crmonline.md#when-a-record-is-created) |オブジェクトが CRM で作成されたときにフローをトリガーします。 |
| [レコードが更新されたとき](connectors-create-api-crmonline.md#when-a-record-is-updated) |オブジェクトが CRM で変更されたときにフローをトリガーします。 |
| [レコードが削除されたとき](connectors-create-api-crmonline.md#when-a-record-is-deleted) |オブジェクトが CRM で削除されたときにフローをトリガーします。 |

## <a name="actions"></a>アクション
| アクション | 説明 |
| --- | --- |
| [レコードを一覧表示する](connectors-create-api-crmonline.md#list-records) |この操作は、エンティティのレコードを取得します。 |
| [新しいレコードを作成する](connectors-create-api-crmonline.md#create-a-new-record) |この操作は、エンティティの新しいレコードを作成します。 |
| [レコードを取得する](connectors-create-api-crmonline.md#get-record) |この操作は、エンティティの指定されたレコードを取得します。 |
| [レコードを削除する](connectors-create-api-crmonline.md#delete-a-record) |この操作は、エンティティ コレクションからレコードを削除します。 |
| [レコードを更新する](connectors-create-api-crmonline.md#update-a-record) |この操作は、エンティティの既存のレコードを更新します。 |

### <a name="trigger-and-action-details"></a>トリガーとアクションの詳細
このセクションでは、必須または任意の入力プロパティ、コネクタに関連付けられた対応する出力など、各トリガーとアクションに関する具体的な詳細について説明します。

#### <a name="when-a-record-is-created"></a>レコードが作成されたとき
CRM のオブジェクトが作成されたときにフローをトリガーします。 

| プロパティ名 | 表示名 | Description |
| --- | --- | --- |
| dataset* |組織名 |Contoso などの CRM 組織の名前 |
| table* |エンティティ名 |エンティティの名前 |
| $skip |Skip Count (スキップ数) |スキップするエントリの数 (既定値 = 0) |
| $top |Maximum Get Count (最大取得数) |取得するエントリの最大数 (既定 = 256) |
| $filter |Filter Query (フィルター クエリ) |返されるエントリを制限する ODATA filter クエリ |
| $orderby |Order By (並べ替え) |エントリの順序を指定する ODATA orderBy クエリ |

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### <a name="output-details"></a>出力の詳細
ItemsList

| プロパティ名 | データ型 |
| --- | --- |
| 値 |array |

#### <a name="when-a-record-is-updated"></a>レコードが更新されたとき
CRM のオブジェクトが変更されたときにフローをトリガーします。 

| プロパティ名 | 表示名 | Description |
| --- | --- | --- |
| dataset* |組織名 |Contoso などの CRM 組織の名前 |
| table* |エンティティ名 |エンティティの名前 |
| $skip |Skip Count (スキップ数) |スキップするエントリの数 (既定値 = 0) |
| $top |Maximum Get Count (最大取得数) |取得するエントリの最大数 (既定 = 256) |
| $filter |Filter Query (フィルター クエリ) |返されるエントリを制限する ODATA filter クエリ |
| $orderby |Order By (並べ替え) |エントリの順序を指定する ODATA orderBy クエリ |

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### <a name="output-details"></a>出力の詳細
ItemsList

| プロパティ名 | データ型 |
| --- | --- |
| 値 |array |

#### <a name="when-a-record-is-deleted"></a>レコードが削除されたとき
CRM のオブジェクトが削除されたときにフローをトリガーします。 

| プロパティ名 | 表示名 | Description |
| --- | --- | --- |
| dataset* |組織名 |Contoso などの CRM 組織の名前 |
| table* |エンティティ名 |エンティティの名前 |
| $skip |Skip Count (スキップ数) |スキップするエントリの数 (既定値 = 0) |
| $top |Maximum Get Count (最大取得数) |取得するエントリの最大数 (既定 = 256) |
| $filter |Filter Query (フィルター クエリ) |返されるエントリを制限する ODATA filter クエリ |
| $orderby |Order By (並べ替え) |エントリの順序を指定する ODATA orderBy クエリ |

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### <a name="output-details"></a>出力の詳細
ItemsList

| プロパティ名 | データ型 |
| --- | --- |
| 値 |array |

#### <a name="list-records"></a>レコードを一覧表示する
この操作では、エンティティのレコードを取得します。 

| プロパティ名 | 表示名 | Description |
| --- | --- | --- |
| dataset* |組織名 |Contoso などの CRM 組織の名前 |
| table* |エンティティ名 |エンティティの名前 |
| $skip |Skip Count (スキップ数) |スキップするエントリの数 (既定値 = 0) |
| $top |Maximum Get Count (最大取得数) |取得するエントリの最大数 (既定 = 256) |
| $filter |Filter Query (フィルター クエリ) |返されるエントリを制限する ODATA filter クエリ |
| $orderby |Order By (並べ替え) |エントリの順序を指定する ODATA orderBy クエリ |

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### <a name="output-details"></a>出力の詳細
ItemsList

| プロパティ名 | データ型 |
| --- | --- |
| 値 |array |

#### <a name="create-a-new-record"></a>新しいレコードを作成する
この操作では、エンティティの新しいレコードを作成します。 

| プロパティ名 | 表示名 | Description |
| --- | --- | --- |
| dataset* |組織名 |Contoso などの CRM 組織の名前 |
| table* |エンティティ名 |エンティティの名前 |

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### <a name="output-details"></a>出力の詳細
なし。

#### <a name="get-record"></a>レコードを取得する
この操作では、エンティティの指定されたレコードを取得します。 

| プロパティ名 | 表示名 | Description |
| --- | --- | --- |
| dataset* |組織名 |Contoso などの CRM 組織の名前 |
| table* |エンティティ名 |エンティティの名前 |
| id* |項目識別子 |レコードの識別子を指定します。 |

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### <a name="output-details"></a>出力の詳細
なし。

#### <a name="delete-a-record"></a>レコードを削除する
この操作では、エンティティのコレクションからレコードを削除します。 

| プロパティ名 | 表示名 | Description |
| --- | --- | --- |
| dataset* |組織名 |Contoso などの CRM 組織の名前 |
| table* |エンティティ名 |エンティティの名前 |
| id* |項目識別子 |レコードの識別子を指定します。 |

アスタリスク (*) は、そのプロパティが必須であることを意味します。

#### <a name="update-a-record"></a>レコードを更新する
この操作では、エンティティの既存のレコードを更新します。 

| プロパティ名 | 表示名 | Description |
| --- | --- | --- |
| dataset* |組織名 |Contoso などの CRM 組織の名前 |
| table* |エンティティ名 |エンティティの名前 |
| id* |レコード識別子 |レコードの識別子を指定します。 |

アスタリスク (*) は、そのプロパティが必須であることを意味します。

##### <a name="output-details"></a>出力の詳細
なし。

## <a name="http-responses"></a>HTTP 応答
アクションとトリガーは、次の HTTP 状態コードを&1; つ以上返す場合があります。 

| Name | 説明 |
| --- | --- |
| 200 |OK |
| 202 |承認済み |
| 400 |正しくない要求 |
| 401 |権限がありません |
| 403 |許可されていません |
| 404 |見つかりません |
| 500 |内部サーバー エラー。 不明なエラーが発生しました。 |
| default |操作に失敗しました。 |

## <a name="next-steps"></a>次のステップ
[ロジック アプリを作成](../logic-apps/logic-apps-create-a-logic-app.md)します。 [API の一覧](apis-list.md)で、Logic Apps で使用できる他のコネクタを確認してください。




<!--HONumber=Feb17_HO2-->


