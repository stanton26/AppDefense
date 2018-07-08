---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2018-06-01
tags: workshop
toc: true
classes: wide
author_profile: false
---
<!--
source file date: 1 June 2018
source file commit: 8e0b439057548686383171cbd426d83acf80bdcf
-->

<!--
# Introduction
-->
# はじめに

<!--
## Add a Host to your SDDC

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-11-Image-5.png)

1. On your "Student Workshop #" SDDC, click on the "View Details" button.
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-11-Image-6.png)
2. Click on the "Actions" button
3. Click on "Add Hosts"
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-12-Image-7.png)
4. As you will only be adding only one host, review the field highlighted
5. Click the "Add Hosts" button

Congratulations! You have completed this step. The adding of an additional host to an existing SDDC should take approximately 10 minutes to complete.
-->
## SDDC へのホストの追加

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-11-Image-5.png)

1. 受講生に割り当てられた SDDC "Student Workshop #" の "View Details" ボタンをクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-11-Image-6.png)
2. "Actions" をクリックします。
3. "Add Hosts" をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-12-Image-7.png)
4. ここでは 1 ホストのみしか追加できませんが、ハイライトされたフィールドを確認してください。
5. "Add Hosts" ボタンをクリックします。

おめでとうございます！これでこのステップは完了です。既存の SDDC へのホストの追加は、完了まで 10 分程度掛かります。

----------------
<!--
## Configuring SDDC Firewall Rules
-->

## SDDC のファイアウォール ルールの構成

<!--
### Management Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-8.png)

By default, the firewall for the management gateway is set to deny all inbound and outbound traffic. In this exercise, you will add a firewall rule to allow vCenter traffic. In order to access vCenter Server in your SDDC, you must set a firewall rule to allow traffic to the vCenter Server.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-9.png)

1. Under **Management Gateway** click the arrow to expand **Firewall Rules**
2. Click **Add Rule**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-10.png)
3. Enter a name for your rule under **Rule Name**
4. Type **Any** for Source
5. Make sure **vCenter** is selected as Destination
6. Select **HTTPS (TCP 443)** from the drop down box for Service
7. Click the **SAVE** button, your rule should look like the below image

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-11.png)
-->
### マネージメント ゲートウェイのファイアウォール ルール

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-8.png)

デフォルトでは、マネージメント ゲートウェイのファイアウォールはインバウンド、アウトバウンドの全てのトラフィックが拒否 (Deny) に設定されています。この演習では、vCenter へのトラフィックを許可するファイアウォール ルールを追加します。SDDC の vCenter Server へアクセスするためには、vCenter Server へのトラフィックを許可するファイアウォール ルールを設定しなければなりません。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-9.png)

1. **Management Gateway** の下にある下矢印をクリックして **Firewall Rules** を展開します。
2. **Add Rule** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-13-Image-10.png)
3. **Rule Name** の下にルールの名前を入力します。
4. Source には **Any** と入力します。
5. Destination として **vCenter** が選択されていることを確認します。
6. **HTTPS (TCP 443)** を Service のドロップ ダウン ボックスから選択します。   
7. **SAVE** ボタンをクリックすると、以下のイメージのようにルールが作成されます。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-11.png)

<!--
### Compute Gateway Firewall Rules

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-12.png)

By default, the Compute NSX Edge Services Gateway is also set to deny all inbound and outbound traffic. You need to add additional firewall rules to allow traffic as needed.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-13.png)
-->

### コンピュート ゲートウェイのファイアウォール

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-12.png)

デフォルトでは、コンピュート ゲートウェイも同様に、全てのインバウンド、アウトバウンド トラフィックが拒否 (Deny) に設定されています。必要に応じてファイアウォール ルールを追加していく必要があります。

<!--
#### Create Firewall Rule under Compute Gateway for Inbound Native AWS Services access

1. Under **Network** tab, navigate to **Compute Gateway**
2. Expand **Firewall Rules**
3. Click **ADD RULE**
-->

#### ネイティブ AWS サービスへのアクセスのためにコンピュート ゲートウェイにファイアウォール ルールを作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-14-Image-13.png)

1. **Network** タブ配下の、**Compute Gateway** を表示させます。
2. **Firewall Rules** を展開します。
3. **ADD RULE** をクリックします。

<!--
#### AWS Inbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-15-Image-14.png)

1. **Name** - AWS Inbound
2. **Action** - Allow
3. **Source** - All connected Amazon VPC
4. **Destination** - 192.168.#.0/24 (Where # is your student number)
5. **Service** - ANY
6. Click **SAVE** button.
-->

#### AWS インバウンド ファイアウォール ルールの作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-15-Image-14.png)

1. **Name** - AWS Inbound
2. **Action** - Allow
3. **Source** - All connected Amazon VPC
4. **Destination** - 192.168.#.0/24 (# は受講者番号)
5. **Service** - ANY
6. **SAVE** ボタンをクリックします。

<!--
#### Create AWS Outbound Firewall Rule

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-15-Image-15.png)

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:

1. **Name** - AWS Outbound
2. **Action** - Allow
3. **Source** - 192.168.#.0/24 (Where # is your student number)
4. **Destination** - All connected Amazon VPC
5. **Service** - ANY
6. Click **SAVE** button.
-->

#### AWS アウトバウンド ファイアウォール ルールの作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-15-Image-15.png)

Follow the same process as in the previous step and create AWS Outbound Firewall Rule following these instructions:
前のステップと同じ手順をなぞり、AWS アウトバウンド ファイアウォール ルールを以下の手順で作成します。

1. **Name** - AWS Outbound
2. **Action** - Allow
3. **Source** - 192.168.#.0/24 (Where # is your student number)
4. **Destination** - All connected Amazon VPC
5. **Service** - ANY
6. **SAVE** ボタンをクリックします。

----------------

<!--
## Log into VMware Cloud on AWS vCenter

Connection Info Tab

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-16-Image-16.png)

Login to your Student # SDDC and click on the **Connection Info** tab. This displays different connection information for your VMware Cloud on AWS environment like:

- URL to vSphere Client HTML5 client for your vCenter server
- URL to vCenter Server API Explorer
- Local Username for access to vCenter Server on VMware Cloud on AWS **cloudadmin@vmc.local**
- Ability to copy Username to your computer's clipboard
- Password to utilize for the cloudadmin user to access vCenter
- Ability to view cloudadmin's password
- Ability to copy cloudadmin's password to your computer's clipboard
- PowerCLI Connect string to be used if you desire to use PowerCLI to connect to your VMware Cloud on AWS vCenter Server
- Ability to copy PowerCLI string to your computer's clipboard

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-17-Image-17.png)

Click on the vSphere Client's HTML5 URL, and login with **cloudadmin@vmc.local** User Name and copy the password to your computer's clipboard and paste it in the Password Field.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-17-Image-18.png)

You are now logged in to your VMware Cloud on AWS vCenter Server as **cloudadmin@vmc.local** user.
-->

## VMware Cloud on AWS の vCenter へのログイン

Connection Info タブ

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-16-Image-16.png)

受講者番号の SDDC にログインし、**Connection Info** をクリックします。このタブは、受講者の VMware Cloud on AWS 環境への以下の接続情報を表示します。この接続情報は受講者毎に異なります。

- 受講者の vCenter Server の vSphere Client HTML5 クライアントの URL
- vCenter Server API エクスプローラーの URL
- VMware Cloud on AWS の vCenter Server にアクセスするためのローカル ユーザー名 **cloudadmin@vmc.local**
- ユーザー名をコンピューターのクリップボードにコピーする機能。
- vCenter へアクセスするための cloudadmin ユーザーで用いられるパスワード。
- cloudadmin のパスワードを表示する機能。
- cloudadmin のパスワードをコンピューターのクリップボードにコピーする機能。
- VMware Cloud on AWS の vCenter Server へ接続するために PowerCLI を用いる場合の PowerCLI の接続文字列
- PowerCLI の接続文字列をコンピューターのクリップボードにコピーする機能。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-17-Image-17.png)

Click on the vSphere Client's HTML5 URL, and login with **cloudadmin@vmc.local** User Name and copy the password to your computer's clipboard and paste it in the Password Field.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-17-Image-18.png)

You are now logged in to your VMware Cloud on AWS vCenter Server as **cloudadmin@vmc.local** user.

<!--
## Create Content Library

Content libraries are container objects for VM templates, vApp templates, and other types of files like ISO images.

You can create a content library in the vSphere Web Client, and populate it with templates, which you can use to deploy virtual machines or vApps in your VMware Cloud on AWS environment or if you already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

You can create two types of libraries: local or subscribed libraries.
-->

## コンテンツ ライブラリの作成

コンテンツ ライブラリは、仮想マシン テンプレート、vApp テンプレート および ISO ファイルなどのその他のタイプのファイルのためのコンテナ オブジェクトです。

vSphere Web クライアントでコンテンツ ライブラリを作成でき、コンテンツ ライブラリは仮想マシンや vApp を VMware Cloud on AWS の環境にデプロイするのに使うことができます。また、オンプレミスのデータセンターにすでにコンテンツ ライブラリを持っている場合は、そのコンテンツ ライブラリを使って SDDC にコンテンツをインポートすることができます。

ライブラリは、ローカル ライブラリおよび購読済みライブラリの 2 種類作成することができます。

<!--
### Local Libraries

You use a local library to store items in a single vCenter Server instance. You can publish the local library so that users from other vCenter Server systems can subscribe to it. When you publish a content library externally, you can configure a password for authentication.

VM templates and vApp templates are stored as an OVF file format in the content library. You can also upload other file types, such as ISO images, text files, and so on, in a content library.
-->

### ローカル ライブラリ

ローカル ライブラリは、一つの vCenter Server インスタンスにアイテムを保存するのに利用できます。他の vCenter Server システムのユーザーが購読 (Subscribe) するために、ローカル ライブラリを公開することができます。外部にコンテンツ ライブラリを公開する際は、認証のためのパスワードを設定することができます。

コンテンツ ライブラリに、VM テンプレートと vApp テンプレートは OVF ファイル フォーマットで保存で保存されます。また、ISO イメージ、テキスト ファイルなど その他のファイル タイプも、コンテンツ ライブラリにアップロードできます。

<!--
### Subscribed Libraries

You subscribe to a published library by creating a subscribed library. You can create the subscribed library in the same vCenter Server instance where the published library is, or in a different vCenter Server system. In the Create Library wizard you have the option to download all the contents of the published library immediately after the subscribed library is created, or to download only metadata for the items from the published library and later to download the full content of only the items you intend to use.

To ensure the contents of a subscribed library are up-to-date, the subscribed library automatically synchronizes to the source published library on regular intervals.

You can also manually synchronize subscribed libraries. You can use the option to download content from the source published library immediately or only when needed to manage your storage space.

Synchronization of a subscribed library that is set with the option to download all the contents of the published library immediately, synchronizes both the item metadata and the item contents. During the synchronisation the library items that are new for the subscribed library are fully downloaded to the storage location of the subscribed library.

Synchronization of a subscribed library that is set with the option to download contents only when needed synchronizes only the metadata for the library items from the published library, and does not download the contents of the items. This saves storage space. If you need to use a library item you need to synchronize that item. After you are done using the item, you can delete the item contents to free space on the storage. For subscribed libraries that are set with the option to download contents only when needed, synchronizing the subscribed library downloads only the metadata of all the items in the source published library, while synchronizing a library item downloads the full content of that item to your storage. If you use a subscribed library, you can only utilize the content, but cannot contribute with content. Only the administrator of the published library can manage the templates and files.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-19-Image-19.png)

1. Click on **Menu**
2. Click on **Content Libraries**

-->

### 購読済みライブラリ

購読済みライブラリを作成することで、公開されたライブラリを購読 (Subscribe) することができます。購読済みライブラリは、公開されたライブラリがある同じ vCenter Server インスタンスにも作成でき、また異なる vCenter Server システムにも作成することができます。「ライブラリの作成」ウィザードでは、2 つの選択肢があります。購読済みライブラリが作成された後、公開されたライブラリの全てのコンテンツを直ぐにダウンロードするか、公開ライブラリのアイテムのメタデータのみをダウンロードし、アイテムを利用した際に後からそのアイテムの全てのコンテンツをダウンロードするかを選択できます。

購読済みライブラリのコンテンツが最新であることを保障するために、購読済みライブラリは、一定時間毎にソースとなる公開されたライブラリと自動的に同期します。

また、購読済みライブラリは手動でも同期できます。ソースの公開ライブラリから直ぐにコンテンツをダウンロードするオプションと、ストレージ容量を管理する場合には必要に応じてダウンロードするオプションを利用できます。

公開ライブラリのコンテンツを全て直ぐにダウンロードするオプションに設定された購読済みライブラリの同期は、アイテムのメタデータとアイテムのコンテンツの両方を同期させます。同期中、購読済みライブラリにとって新しいライブラリアイテムは、購読済みライブラリのストレージ ロケーションに完全にダウンロードされます。

必要に応じてコンテンツをダウンロードするオプションに設定された購読済みライブラリの同期は、公開ライブラリからライブラリ アイテムのメタデータのみを同期し、アイテムのコンテンツはダウンロードしません。これはストレージ容量を節約できます。ライブラリ アイテムを利用する必要がある際は、そのアイテムを同期させる必要があります。そのアイテムを利用した後は、ストレージのスペースを開放するためにそのアイテムのコンテンツを削除できます。<!-- 次の文は繰返しとなるのと、While 以降の意味が通じない --> 購読済みライブラリを用いる場合、コンテンツを利用するのみで、コンテントを追加することはできません。公開ライブラリの管理者のみが、テンプレートやファイルを管理できます。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-19-Image-19.png)

1. **Menu** をクリックします。
2. **Content Libraries** をクリックします。

<!--
### Subscribe to an existing Content Library

You may already have a Content Library in your on-premises data center, you can use the Content Library to import content into your SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-20.png)

1. In your Content Library window, click the **+** sign to add a new Content Library.
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-21.png)
2. Name your Content Library **Student#** where **#** is the number assigned to you
3. (Optional) Enter some notes for your Content Library
4. Click **Next** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-21-Image-22.png)
5. Select **Subscribed content library**
6. Under **Subscription URL** enter the following: <https://vcenter.sddc-34-216-241-49.vmc.vmware.com/cls/vcsp/lib/8d658764-2e89-44ff-https://vcenter.sddc-34-216-241-49.vmc.vmware.com/cls/vcsp/lib/8d658764-2e89-44ff-a7c1-ee777f0dfc8f/lib.jsona7c1-ee777f0dfc8f/lib.json>

    PLEASE NOTE THAT THERE MAY BE AN ISSUE WITH DROPPING/ADDITION OF CHARACTERS FOR THE URL WHEN COPYING AND PASTING FROM THE MANUAL. THE ACTUAL URL IS ALSO AVAILABLE IN YOUR STUDENT DESKTOP ON THE Z:\ DRIVE IN A TEXT FILE, OPEN THIS TEXT FILE AND COPY THE URL FROM THERE. ASK YOUR INSTRUCTOR IN THE EVENT YOU CANNOT LOCATE IT.
7. Click the checkbox for **Enable Authentication**
8. For **Password** enter: **VMware1!**
9. Ensure Download content is set to **Immediately**
10. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-23.png)
11. Highlight the **WorkloadDatastore** as the storage location
12. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-24.png)
13. Click **Finish**. Your content library should take about ~20 minutes to complete syncing.
-->

### 既存のコンテンツ ライブラリの購読

オンプレミスのデータセンターに既にコンテンツ ライブラリがあれば、SDDC にコンテンツをインポートするためにそのコンテンツ ライブラリを利用できます。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-20.png)

1. コンテンツ ライブラリ ウィンドウにて、**+** マークをクリックし新しいコンテンツ ライブラリを追加します。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-20-Image-21.png)
2. コンテンツ ライブラリの名前を **Student#** とします。ここで **#** は受講者に割り当てられた番号です。
3. (オプション) コンテンツ ライブラリのノートを適当に入力します。
4. **Next** ボタンをクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-21-Image-22.png)
5. **Subscribed content library** を選択します。
6. Under **Subscription URL** の下に以下を入力します。<https://vcenter.sddc-34-216-241-49.vmc.vmware.com/cls/vcsp/lib/8d658764-2e89-44ff-https://vcenter.sddc-34-216-241-49.vmc.vmware.com/cls/vcsp/lib/8d658764-2e89-44ff-a7c1-ee777f0dfc8f/lib.jsona7c1-ee777f0dfc8f/lib.json>

    マニュアルからコピー アンド ペーストした場合、URL の文字の不意に欠損/追加させてしまう問題があるかもしれません。実際の URL は、受講者のデスクトップの Z:\ ドライブにテキスト ファイルとして利用できますので、そこから URL をコピーしてください。見付からない場合はインストラクターに質問して下さい。
7. **Enable Authentication** チェック ボックスをクリックします。
8. **Password** は **VMware1!** と入力します。
9. ダウンロード コンテントが **Immediately** に設定されていることを確認します。
10. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-23.png)
11. ストレージ ロケーションとして **WorkloadDatastore** をハイライトします。
12. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-22-Image-24.png)
13. **Finish** をクリックします。コンテンツ ライブラリが同期を完了するまで 20 分ほど掛かります。

<!--
### Create a Local Content Library

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-23-Image-25.png)

1. Click the **+** sign to create a new Content Library
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-23-Image-26.png)
2. Name your new Content Library: **LocalContentLibrary#** (where # is your student #)
3. (Optional) Enter some notes about your Content Library
4. Click **Next** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-24-Image-27.png)
5. Make sure **Local content library** is selected
6. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-24-Image-28.png)
7. Highlight the **WorkloadDatastore** as the storage location
8. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-25-Image-29.png)
9. Review your information and click **Finish**

Congratulations, you have created your Local Content Library.
-->

### ローカル コンテンツ ライブラリの作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-23-Image-25.png)

1. **+** マークをクリックし、新しいコンテンツ ライブラリを追加します。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-23-Image-26.png)
2. コンテンツ ライブラリの名前を **LocalContentLibrary#** とします。(# は受講者番号になります。)
3. (オプション) コンテンツ ライブラリのノートを適当に入力します。
4. **Next** ボタンをクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-24-Image-27.png)
5. **Local content library** が選択されていることを確認します。
6. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-24-Image-28.png)
7. ストレージ ロケーションとして **WorkloadDatastore** をハイライトさせます。
8. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-25-Image-29.png)
9. 入力した情報を確認し、**Finish** をクリックします。

おめでとうございます。ローカル コンテンツ ライブラリが作成されました。

<!--
## Create Logical Network

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-26-Image-30.png)

1. Once you are logged in to your vCenter Server Click on **Menu**
2. Select **Global Inventory Lists** from the drop down menu
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-26-Image-31.png)
3. Click on **Logical Networks** in the left pane
4. Click on the **Add** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-27-Image-32.png)
5. Name your New Logical Network **Student#-LN** (where # is your student number)
6. Make sure to select **Routed Network**
7. For CIDR Block enter **192.168.###.0/24** (where # is your student #)
    If your designated student number is between 1 and 9, your CIDR block should look like this: **192.168.1.0/24** - This example represents student number 1. For students 10 thru 20 it should look like this: **192.168.10.0/24** - This example represents student number 10
8. Enter **192.168.###.1** for the Default Gateway IP - Example: 192.168.1.1
9. Make sure DHCP is Enabled by clicking on the **checkbox**
10. Enter **192.168.###.100-192.168.###.200** for IP Range
11. Type **corp.local** as your DNS Domain Name
12. Click **OK** to create your new logical network
-->

## 論理ネットワークの作成

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-26-Image-30.png)

1. vCenter Server にログインし、**メニュー** をクリックします。
2. ドロップ ダウン メニューから **グローバル インベントリ リスト** を選択します。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-26-Image-31.png)
3. 左ペインから **論理ネットワーク** をクリックします。
4. **追加** ボタンをクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-27-Image-32.png)
5. 新しい論理ネットワークの名前を **Student#-LN** とします。(# は受講者番号となります。)
6. **Routed Network** が選択されていることを確認します。
7. CIDR ブロックに **192.168.###.0/24** を入力します。(# は受講者番号となります。)
    受講者番号が 1 から 9 の場合、CIDR ブロックは次のようになります。**192.168.1.0/24** - この例は受講者番号 1 の場合です。受講者番号が 10 から 20 の場合、CIDR ブロックは次のようになります。**192.168.10.0/24** - この例は受講者番号 10 の場合です。
8. デフォルト ゲートウェイ IP に **192.168.###.1** を入力します。- 例: 192.168.1.1
9. **チェック ボックス** をクリックして、DHCP が 有効であることを確認します。
10. IP レンジに **192.168.###.100-192.168.###.200** を入力します。
11. DNS ドメイン名として **corp.local** を入力します。
12. **OK** をクリックし、新しい論理ネットワークを作成します。

<!--
## Create Linux Customization Spec

When you clone a virtual machine or deploy a virtual machine from a template, you can customize the guest operating system of the virtual machine to change properties such as the computer name, network settings, and license settings.

Customizing guest operating systems can help prevent conflicts that can result if virtual machines with identical settings are deployed, such as conflicts due to duplicate computer names.

You can specify the customization settings by launching the Guest Customization wizard during the cloning or deployment process. Alternatively, you can create customization specifications, which are customization settings stored in the vCenter Server database. During the cloning or deployment process, you can select a customization specification to apply to the new virtual machine.

Use the Customization Specification Manager to manage customization specifications you create with the Guest Customization wizard.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-28-Image-33.png)

1. Click **Menu**
2. Click **Policies and Profiles**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-29-Image-34.png)
3. Click on **+ New** to add a new Linux Customization Specification
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-29-Image-35.png)
4. Give your VM Customization Spec a Name
5. Enter a description for it (Optional)
6. Make sure to select **Linux**
7. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-30-Image-36.png)
8. Click on the **Enter a name** button
9. Enter a name for your linux VMs
10. Click on the **Append a numeric value** checkbox
11. Enter **corp.local** for the Domain Name
12. Click the **Next** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-31-Image-37.png)
13. Select **US** for Area
14. Select **Eastern** for Location
15. Select **Local time**
16. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-31-Image-38.png)
17. Leave the defaults on the **Network** screen and click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-32-Image-39.png)
18. Under Primary DNS Server enter **10.46.159.10**
19. Type **corp.local** for DNS Search Paths
20. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-32-Image-40.png)
21. Review your entries and click **Finish**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-33-Image-41.png)

Congratulations! You have successfully created your VM Customization Spec for your Linux VMs. You can also export (Duplicate), Edit, Import, and Export a VM Customization Spec.
-->

## Linux 向けカスタマイズ仕様の作成

仮想マシンをクローンする際、あるいは、テンプレートから仮想マシンをデプロイする際、コンピューター名、ネットワーク設定やライセンス設定を変更することで仮想マシンのゲスト オペレーティング システムをカスタマイズすることができます。

ゲスト オペレーティング システムをカスタマイズすることは、重複したコンピューター名により名前が衝突するといった、同一の設定を持つ仮想マシンがデプロイされることによる衝突を防ぐ助けとなります。

デプロイやクローンのの途中でゲスト カスタマイゼーション ウィザードを起動することによって、カスタマイズ設定を行うことができます。あるいは、vCenter Server のデータベースにカスタマイズ設定が保存されるカスタマイズ仕様を作成することもできます。クローンやデプロイの途中で、カスタマイズ仕様を選択し新しい仮想マシンに適用することができます。

ゲスト カスタマイゼーション ウィザードで作成したカスタマイズ仕様を管理するために、カスタマイズ仕様マネージャを利用して下さい。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-28-Image-33.png)

1. **メニュー** をクリックします。
2. **ポリシーおよびプロファイル** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-29-Image-34.png)
3. Click on **+ 新規...** をクリックし、新しい Linu 向けカスタマイズ仕様を追加します。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-29-Image-35.png)
4. カスタマイズ仕様に名前を付けます。
5. カスタマイズ仕様に説明を付け加えます。(オプショナル)
6. **Linux** を選択します。
7. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-30-Image-36.png)
8. **名前を入力** ボタンをクリックします。
9. Linux 仮想マシンの名前を入力します。
10. **数値を付加します** チェック ボックスをクリックします。
11. ドメイン名に **corp.local** を入力します。
12. **Next** ボタンをクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-31-Image-37.png)
13. エリアで **US** を選択します
14. 保存場所で **Eastern** を選択します
15. **ローカル時間** を選択します。
16. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-31-Image-38.png)
17. **ネットワーク** スクリーンはデフォルトのままとし、**Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-32-Image-39.png)
18. プライマリ DNS サーバーに **10.46.159.10** と入力します。
19. DNS 検索パスに **corp.local** と入力します。
20. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-32-Image-40.png)
21. 入力した項目を確認し、**Finish** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-33-Image-41.png)

おめでとうございます。Linux 仮想マシンのためのカスタマイズ仕様が作成されました。カスタマイズ仕様は、複製、編集、インポート、エクスポートすることができます。

<!--
## Deploy Virtual Machine

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-34-Image-42.png)

1. On your Content Libraries (Menu -> Content Libraries)**, select **Student#** and select the **Templates** tab.
2. Right click on the **centos01-web** template and select **New VM from This Template**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-34-Image-43.png)
3. Name your Virtual Machine **StudentVM#** (where # is your student number)
4. Expand the location area until you see **Workloads** and highlight it
5. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-35-Image-44.png)
6. Expand the destination compute resources until you find **Compute-ResourcePool**, select it
7. Click **Next** button
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-35-Image-45.png)
8. Click **Next** button on the Review details screen
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-36-Image-46.png)
9. In the **Select storage** step, highlight **WorkloadDatastore**
10. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-36-Image-47.png)
11. In the **Select networks** step, click the drop down box to select the Destination Network (you may need to click Browse to see other networks and select your "Student#-LNStudent#-LN" network you created previously
12. Click **Next**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-37-Image-48.png)
13. In the **Ready to complete** section, review to ensure all your selections are correct and click **Finish**
-->

## 仮想マシンのデプロイ

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-34-Image-42.png)

1. コンテンツ ライブラリ (メニュー -> コンテンツ ライブラリ) にて、**Student#** を選択し、**Templates** タブを選択します。
2. **centos01-web** テンプレートを右クリックし、**New VM from This Template** を選択します。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-34-Image-43.png)
3. 仮想マシンの名前を **StudentVM#** とします。(# は受講者番号となります。)
4. **Workloads** が見付かるまでロケーション エリアを展開し、これをハイライトします。
5. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-35-Image-44.png)
6. **Compute-ResourcePool** が見付かるまで Destination コンピュート リソースを展開し、これを選択します。
7. **Next** ボタンをクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-35-Image-45.png)
8. 設定の確認スクリーンで **Next** ボタンをクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-36-Image-46.png)
9. **Select storage** ステップにて、**WorkloadDatastore** をハイライトします。
10. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-36-Image-47.png)
11. **Select networks** ステップで、ドロップ ダウン リストをクリックして Destination Network を選択します。(他のネットワークを参照したり、事前に作成した "Student#-LNStudent#-LN" ネットワークを選択するために、Browse をクリックする必要があるかも知れません)
12. **Next** をクリックします。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-37-Image-48.png)
13. **Ready to complete** セクションにて、全てのセクションが正しいことを確認し、**Finish** をクリックします。

<!--
## Convert Virtual Machine to Template

In this step you will be cloning your newly created Virtual Machine into a Template for later use in vRealize Automation section.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-38-Image-49.png)

1. Ensure your VM deployment completed from your previous step
2. Click on **Menu**
3. Select **VMs and Templates**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-50.png)
4. Select your newly created VM **Student#** (where # is your student number)
5. Click on **Template**
6. Select **Convert to Template**
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-51.png)
7. Click **Yes**  in the Convert to Template prompt

You have completed this step.
-->

## 仮想マシンをテンプレートにコンバート

このステップでは、後続の vRealize Automation セクションのために、新しく作成された仮想マシンを仮想マシンにクローンします。

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-38-Image-49.png)

1. 前のステップの仮想マシンのデプロイが完了していることを確認します。
2. **メニュー** をクリックします。
3. **仮想マシンおよびテンプレート** を選択します。
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-50.png)
4. 新しく作成された仮想マシン **Student#** を選択します。(# は受講者番号になります)
5. **テンプレート** をクリックします。
6. Select **テンプレートに変換** を
    ![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/Page-39-Image-51.png)
7. 変換の確認 ダイアログで **はい** をクリックします。

このステップを完了しました。