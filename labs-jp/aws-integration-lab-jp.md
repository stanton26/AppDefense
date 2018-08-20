---
layout: single
title: "AWS Integration Lab Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
<!--
# Introduction

One of the most compelling reasons to adopt VMware Cloud on AWS is to integrate your existing systems which sit in your VMware cloud environment, with application platforms which reside in your AWS Virtual Private Cloud (VPC) environment. The intergration which VMware and AWS have created allows for these services to communicate, for free, across a private network address space for services such as EC2 instances, which connect into subnets within a native AWS VPC, or with platform services which have the ability to connect to a VPC Endpoint, such as S3 Storage.

In this lab we will work through some common basic integrations which you can utilise in your VMware Cloud on AWS platform.

Note: There is a requirement in this lab to have completed the steps in the [Working with your SDDC Lab](https://vmc-field-team.github.io/labs/working-with-sddc-lab/) concerning Content Library creation and Network creation and firewall rule creation.
-->

# はじめに

VMware Cloud on AWS を採用する時の最も切実な理由の一つに、VMware Cloud on AWS 環境にある既存のシステムを、AWS Virtual Private Cloud (VPC) にあるアプリケーション プラットフォームと統合することが挙げられます。VMware と AWS によるインテグレーションは、既存のシステムと、EC2 のようなサービスのためのプライベート ネットワーク アドレス空間の通信を可能とします。また S3 ストレージのような VPC エンドポイントによる接続が可能なプラットフォーム サービスとも通信が可能となります。そしてこれらの通信は無料です。

このラボでは、VMware Cloud on AWS のプラットフォームでも利用できるいくつかの良くある基本的なインテグレーションに取り組みます。

注意: このラボの前提として [SDDC Lab を使ってみる](https://vmc-field-team.github.io/labs-jp/working-with-sddc-lab-jp/) の、コンテント ライブラリの作成、ネットワークの作成、およびファイアウォール ルールの作成を完了して下さい。


<!--
## AWS Relational Database Service (RDS) Integration

### Deploy Photo VM

As a first step in setting up our integration between the VMware vSphere platform in VMware Cloud on AWS and native AWS services, we are going deploy a VM which we will use for this demo. This VM will be referred to as "Photo VM". Please follow the instructions below.

1. If not already opened, open your VMware Cloud on AWS vCenter and click on the **Menu** drop down
2. Select **Content Libraries**
3. Click on your previously created Content Library named **Student#** (where # is your student number)
4. Make sure you click on the **Template** tab
5. Right-click on the **photo app** Template
6. Select **New VM from This Template**
7. Name the virtual machine **PhotoApp#** (where # is your student #)
8. Expand the location and select **Workloads**
9. Click **Next**
10. Expand the destination to select **Compute-ResourcePool** as the compute resource
11. Click **Next**
12. In the "Review details" step click **Next**
13. In the **Select storage** step, highlight the "WorkloadDatastore"
14. Click **Next**
15. Select the network you created in a previous step for your VM
16. Click **Next**
17. Click **Finish**
18. Monitor the deployment of your VM until it's completed
19. Check for completion of the deployment of your VM
20. Click **Menu**
21. Select **VMs and Templates**
22. Check to make sure your VM is powered on. If not, right-click on your VM
23. Select **Power** -> **Power On**
24. Make sure your VM is assigned an IP addresses (may need to a few minutes for this information to be populated). Make a note of this IP address for a future step.
-->

## AWS リレーショナル データベース サービス (RDS) インテグレーション

### Photo VM のデプロイ

VMware Cloud on AWS の VMware vSphere プラットフォームとネイティブの AWS サービスのインテグレーションをセットアップする最初のステップとして、このデモで使う仮想マシンをデプロイします。この VM は Photo VM として参照されます。以下の手順を進めて下さい。

1. もし開いていないのであれば、VMware Cloud on AWS の vCenter を開き、**メニュー** ドロップ ダウンをクリックしてください
2. **コンテント ライブラリ** を選択してください
3. **Student#** と名前を付けて前に作成したコンテント ライブラリをクリックします (# は受講者番号)
4. **テンプレート** タブがクリックされていることを確認してください
5. **photo app** テンプレートを右クリックします
6. **このテンプレートから仮想マシンを新規作成** を選択します
7. 仮想マシンの名前を **PhotoApp#** とします (# は受講者番号)
8. ツリーを展開し、**Workloads** を選択します
9. **NEXT** をクリックします
10. ツリーを展開し **Compute-ResourcePool** をコンピュート リソースとして選択します
11. **Next** をクリックします
12. "詳細の確認" ステップで **NEXT** をクリックします
13. **ストレージの選択** ステップで、"WorkloadDatastore" をハイライトします
14. **NEXT** をクリックします
15. 仮想マシンのために前に作成したネットワークを選択します
16. **NEXT** をクリックします
17. **FINISH** をクリックします
18. デプロイが完了するまで、デプロイメント タスクを監視します
19. 仮想マシンのデプロイが完了したことを確認します
20. **メニュー** をクリックします
21. **仮想マシンおよびテンプレート** を選択します
22. デプロイされた仮想マシンで右クリックし、仮想マシンをパワーオンします
23. **電源** -> **パワーオン** を選択します
24. 仮想マシンに IP アドレスが付与されていることを確認します (この情報が反映されるまで数分かかるかもしれません)。この後のステップのために、この IP アドレスをメモしておいてください

<!--
### Firewall Rules for RDS Integration

In this step we will ensure that we have the correct firewall rules in place in order for our Photo App VM in VMware Cloud on AWS to talk across to the RDS service in our AWS VPC.

1. Go back your VMware Cloud on AWS portal and click on the **Network** tab in order to request a **Public IP address**
2. Under the **Compute Gateway** click and expand **Public IPs**
3. Click on **REQUEST PUBLIC IP***
4. (Optional) Enter Notes for this public IP, such as the name of the VM we are linking this too.
5. Click on **Request**
6. Take note of your newly acquired Public IP address
7. Next you will create a **NAT rule** from the newly acquired Public IP address you noted in your last step to the internal IP address of the VM you created. Click on **NAT** under the "Compute Gateway" section to expand the NAT Rules
8. Click **ADD NAT RULE**
9. Give your **NAT rule** a name
10. Your new Public IP address should be pre-filled for you, if not, select it now
11. Under **Service** select **Any (All Traffic)**
12. Type your VM's internal IP address
13. Click the **SAVE** button
14. You should get a **NAT rule successfully created** notification
15. Expand **Firewall Rules** under the Compute Gateway section
16. Click **ADD RULE**
17. Give your Firewall Rule a name
18. Select **All Internet and VPN** for **Source**
19. Type the Public IP Address you noted under **Destination**
20. Select **Any (All Traffic)** for **Service**
21. Click **SAVE** button
22. You should get a **Firewall rule successfully created** notification
-->

### RDS インテグレーションのためのファイアウォール ルール

このステップでは、VMware Cloud on AWS にある Photo App VM が AWS VPC にある RDS サービスにアクセスできるよう正しくファイアウォール ルールを設定します

1. VMware Cloud on AWS のポータルに戻り、**Public IP Address** をリクエストするために **Network** タブをクリックします
2. **Compute Gateway** セクションの **Public IP** をクリックし展開します
3. **REQUEST PUBLIC IP** をクリックします
4. (オプション) この Public IP にリンクさせる仮想マシンの名前など、この Public IP のための注記を入力します
5. **Request** をクリックします
6. 新しく取得された Public IP アドレスをメモします
7. 次に、前のステップでメモした新しく取得された Public IP アドレスから作成された仮想マシンの内部 IP アドレスへの **NAT Rule** を作成します。**Compute Gateway** セクションの **NAT** をクリックし NAT ルールを展開します
8. **ADD NAT RULE** をクリックします
9. **NAT rule** に名前を入力します
10. 新しい Public IP が既に入力済みのはずですが、そうでない場合は ここで選択します
11. **Service** の下で **Any (All Traffic)** を選択します
12. 仮想マシンの内部 IP アドレスを入力します
13. **SAVE** ボタンをクリックします
14. **NAT rule successfully created** という通知を確認します
15. **Compute Gateway** セクションの **Firewall Rules** を展開します
16. **ADD RULE** をクリックします
17. ファイアウォール ルールの名前を入力します
18. **Source** では **All Internet and VPN** を選択します
19. **Destination** にメモした Public IP アドレスを入力します
20. **Any (All Traffic)** for **Service** では **Any (All Traffic)** を選択します
21. **SAVE** ボタンをクリックします
22. **Firewall rule successfully created** という通知を確認します

<!--
### AWS Relational Database Service (RDS Configuration)

On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>

1. For Account ID or alias ensure "vmcworkshop" is specified
2. IAM user name - Student# (where # is the number assigned to you)
3. Password - VMCworkshop1211
4. Click **Sign In**
5. You are now signed in to the AWS console. Make sure the region selected is **Oregon** in the top right hand corner of the AWS Console
6. Click on the **RDS** service under the "Database" section
7. In the left pane click on **Instances**
8. Click on the RDS instance that corresponds to your Student number
9. Scroll down to the **Details** area and under **Security and network** notice that the RDS instance is not publicly accessible, meaning this instance can only be accessed from within AWS
    ![RDS Public Access](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/Screenshot+at+Jul+20+12-38-34.png)
10. Go back to the main Services page in the AWS console by clicking the **Services** link in the top left corner of the console
11. Scroll down to **Networking & Content Delivery** and click **VPC**
12. Click on **Security Groups** in the left pane under the "Security" section
13. Choose the **rds-launch-wizard-#** RDS Security group corresponding to your student number (where # is the number assigned to you)
14. After highlighting the appropriate security group click on the **Inbound Rules** tab below. VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own
15. Notice that the CIDR block range of your Student#-LN Logical Network you created in VMware Cloud on AWS is authorized for MySQL on port 3306. This was done for you ahead of time
16. AWS Relational Database Service (RDS), also creates its own Elastic Network Interface (ENI) for access which is separate from the ENI created by VMware Cloud on AWS.
17. Click on **Services** in the top left of the console to go back to the Main Console
18. Click on **EC2** under the Compute section
19. In the EC2 Dashboard click **Network Interfaces** in the left pane under the "Network & Security" section
20. All Student environments belong to the same AWS account, therefore, hundreds of ENI's may exist. In order to minimize the view, type "RDS" in the filter area and press **Enter** to add a filter
21. Highlight your **rds-launch-wizard-#** corresponding to your student number (where # is the number assigned to you)
22. Make note of the **Primary private IPv4** IP address for the next step
23. Open an additional browser tab and type your public IP address you requested in the VMware Cloud on AWS portal in the browser address bar followed by /Lychee (case sensitive) ie: x.x.x.x/Lychee
24. Enter the database connection information below (case sensitive), using the IP address you noted in the previous step from the RDS ENI:
    **Database Host**: x.x.x.x:3306
    **Database Username**: student# (where # is the number assigned to you)
    **Database Password**:VMware1!
25. Click **Connect**

You have now successfully created a Hybrid Application utilises components across both your VMware on AWS SDDC environment and your AWS services.

This functionality provides customers now with choices around how applications are migrated to the cloud. You can now split your application across platforms and consume services in vSphere and AWS as it makes sense. This level of choice can really help those who are looking to migrate to the cloud, accelerate that process by not getting "bogged down" in refactoring parts of an application which are difficult to move to hyperscale cloud.
-->

### AWS リレーショナル データベース サービス (RDS の構成)

ブラウザで 新しいタブを開き、<https://vmcworkshop.signin.aws.amazon.com/console> を開きます

1. アカウント ID またはエイリアスに、"vmcworkshop" と入力されていることを確認します
2. IAM ユーザー名 - Student# (# は受講者番号)
3. パスワード - VMCworkshop1211
4. **サインイン** をクリック
5. これで AWS コンソールにログインしました。AWS コンソールの右上角で、リージョンが **オレゴン** と選択されていることを確認します
6. AWS コンソール上部の **サービス** をクリックし、"データベース" セクションの **RDS** をクリックします
7. 左ペインの **インスタンス** をクリックします
8. 受講者番号に対応する RDS インスタンスをクリックします
9. **詳細** エリアまでスクロールさせ、**セキュリティとネットワーク** の下で RDS インスタンスがパブリックアクセス可能ではないことを確認します。これは このインスタンスが AWS からのみアクセスできることを意味します
    ![RDS パブリック アクセス](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/Screenshot+at+Jul+20+12-38-34.png)
10. AWS コンソールの左上角の **サービス** をクリックし、AWS コンソールのメイン サービス ページに戻ります
11. **ネットワーキング & コンテンツ配信** へスクロールし **VPC** をクリックします
12. 左ペインの "セキュリティ" セクションの下の **セキュリティ グループ** をクリックします
13. 受講者番号に対応した RDS のセキュリティ グループ **rds-launch-wizard-#** を選択します (# は受講者番号)
14. 適切なセキュリティ グループをハイライトした後、下部の **インバウンド ルール** をクリックします。VMware Cloud on AWS は、デフォルトの VPC のセキュリティ グループにルーティングを作成し、RDS のみが
<!-- 文意が汲み取れず -->
After highlighting the appropriate security group click on the **Inbound Rules** tab below. VMware Cloud on AWS establishes routing in the default VPC Security Group, only RDS can leverage this or create its own
15. VMware Cloud on AWS で作成した Student#-LN 論理スイッチの CIDR ブロック レンジが MySQL のポート 3306 へ許可されていることを確認します。これは事前に設定されています
16. リレーショナル データベース サービス (RDS) は、VMware Cloud on AWS によって作成される ENI とは別に、アクセスのための自身の Elastic Network Interface (ENI) を作成します。
17. コンソールの左上の **サービス** をクリックし、メイン コンソールに戻ります
18. コンピューティング セクションの下の **EC2** をクリックします
19. EC2 のダッシュボードにて、"ネットワーク & セキュリティ" セクションの下の **ネットワーク インターフェイス** をクリックします
20. 全ての受講者の環境は同じ AWS アカウントに属しているため、数百の ENI があるかもしれません。ビューを小さくするために、フィルター エリアに "RDS" と入力し、そのフィルターを追加するために **Enter** キーを押します
21. 受講者番号に対応した **rds-launch-wizard-#** セキュリティ グループを持つ ENI をハイライトします。(# は受講者番号)
<!-- ENI をハイライトさせる -->
22. 次のステップのために **プライマリ プライベート IPv4** の IP アドレスをメモします
23. 新しいブラウザ タブを開き、ブラウザのアドレスバーに VMware Cloud on AWS でリクエストした IP アドレスを入力し、続いて /Lychee (大文字と小文字を区別) と入力します。例) x.x.x.x/Lychee
24. 以下のデータベース接続情報を入力します。先ほどメモした RDS の ENI の IP アドレスを利用します:
    **Database Host**: x.x.x.x:3306
    **Database Username**: student# (# は受講者番号)
    **Database Password**:VMware1!
25. **Connect** をクリックします

これで、VMware Cloud on AWS の SDDC 環境と AWS のサービスの両方に跨がったコンポーネントを利用するハイブリッド アプリケーションの構築に成功しました。

この機能により、顧客はアプリケーションをクラウドに移行する方法を選択できるようになります。アプリケーションをプラットフォームを跨いで分割し、vSphere と AWS のサービスを利用することができます。このレベルの選択肢はクラウドへ移行しようとしている人々の助けとなります。なぜならば、クラウド レベルのハイパースケールに対応させるのが困難なアプリケーションのリファクタリングに行き詰まることなく移行できるからです。

<!--
## AWS Elastic File System (EFS) Integration

### EFS VM Creation

In our next section on integrating AWS services with VMware Cloud on AWS. We will utilise the AWS Elastic File System (EFS) which we can use to mount NFS shares to our VMware Cloud on AWS hosted virtual machines.

1. Navigate to your Content Library, click **Menu** on your VMware Cloud on AWS vCenter Server
2. Select **Content Libraries**
3. Click on your **Student#** Content Library
4. Make sure the **Templates** tab is selected
5. Right-click on the **efs** template
6. Select **New VM from This Template**
7. Name your VM **EFSVM#** (where # is your student number)
8. Select **Workloads** for the location of your VM
9. Click **Next**
10. Select **Compute-ResourcePool** as the destination for your VM
11. Click **Next**
12. Click **Next**
13. Select **WorkloadDatastore** for storage
14. Click **Next**
15. Select your Destination Network
16. Click **Next**
17. Click **Finish**
18. Make sure to Power on your VM and ensure it has an assigned private IP address
-->

## AWS Elastic File System (EFS) インテグレーション

### EFS VM の作成

このセクションの AWS と VMware Cloud on AWS のインテグレーションでは、VMware Cloud on AWS にホストされた仮想マシンから NFS 共有を使うことができる AWS Elastic File System (EFS) を利用します
<!-- Typo, period/comma, utilize-->

1. コンテンツ ライブラリにナビゲートするために、VMware Cloud on AWS の vCenter で **メニュー** をクリックします
2. **コンテンツ ライブラリ** を選択します
3. コンテンツ ライブラリ **Student#** をクリックします
4. **テンプレート** タブが選択されていることを確認します
5. **efs** テンプレートの上で右クリックします
6. **このテンプレートから仮想マシンを新規作成** を選択します
7. 仮想マシンの名前を **EFSVM#** とします (# は受講者番号)
8. ツリーを展開し **Workloads** を選択します
9. **NEXT** をクリックします
10. ツリーを展開し **Compute-ResourcePool** をコンピュート リソースとして選択します
11. **NEXT** をクリックします
12. **NEXT** をクリックします
13. ストレージは **WorkloadDatastore** を選択します
14. **NEXT** をクリックします
15. ネットワークを選択します <!-- どのネットワーク? -->
16. **NEXT** をクリックします
17. **FINISH** をクリックします
18. 仮想マシンがパワーオンされるのを確認し、プライベート IP アドレスが割り当てられるのを確認します

<!--
### AWS Elastic File System (EFS)

On your browser, open a new tab and go to: <https://vmcworkshop.signin.aws.amazon.com/console>

1. Account ID or alias - vmcworkshop
2. IAM user name - Student# (where # is the number assigned to you)
3. Password - **VMCworkshop1211**
4. Click **Sign In**
5. You are now signed in to the AWS console. Make sure the region selected is **Oregon**
6. Click on the **EFS** service under the storage section
7. Select your Student# NFS (where # is the number assigned to you)
8. Note the IP address
9. Back on your vCenter Server tab, click on **Launch Web Console**  for your EFS VM (Might need to allow pop ups in browser). Log in using the following credentials:
 a. **User**: root
 b. **Password**: VMware1!

Enter the following commands at the prompt:

``` bash
cd /
mkdir efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2
MOUNT_TARGET_IP:/ efs Where MOUNT_TARGET_IP is the IP you noted for your EFS
cd efs
touch hello.world
ls
```

You have now integrated AWS EFS with a VM running in VMware Cloud on AWS.
-->

### AWS Elastic File System (EFS)

ブラウザで新しいタブを開き、<https://vmcworkshop.signin.aws.amazon.com/console> を開きます

1. アカウント ID またはエイリアスに、"vmcworkshop" と入力されていることを確認します
2. IAM ユーザー名 - Student# (# は受講者番号)
3. パスワード - VMCworkshop1211
4. **サインイン** をクリック
5. これで AWS コンソールにログインしました。AWS コンソールの右上角で、リージョンが **オレゴン** と選択されていることを確認します
6. ストレージ セクションの下の **EFS** をクリックします
7. Student# NFS を選択します (# は受講者番号)
8. IP アドレスをメモします
9. vCenter Server のタブに戻り、EFS VM の **Web コンソールの起動** をクリックします (ブラウザのポップアップ許可が必要かも知れません)。以下のクレデンシャルを使いログインします:
 a. **User**: root
 b. **Password**: VMware1!

以下のコマンドをプロンプトで実行します:

``` bash
cd /
mkdir efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2
MOUNT_TARGET_IP:/ efs Where MOUNT_TARGET_IP is the IP you noted for your EFS
cd efs
touch hello.world
ls
```

これで AWS EFS と VMware Cloud on AWS で稼働している仮想マシンのインテグレーションが完了しました。