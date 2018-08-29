---
layout: single
title: "Hybrid Cloud Extension (HCX) ラボ マニュアル"
categories: labs
date: 2018-08-29
tags: workshop
toc: true
classes: wide
author_profile: false
---

<!--
# Introduction

In this lab exercise you will learn about Hybr Cloud Extension (HCX), Primarily this is a tool, bundled with VMware Cloud on AWS, which will allow you to bulk migrate workloads to VMware Cloud on AWS and significantly reduce the time and complexity of moving workloads into the public cloud sphere.
-->

# はじめに

このラボでは Hybrid Cloud Extension (HCX) について学びます。このツールは VMware Cloud on AWS にバンドルされています。VMware Cloud on AWS へのワークロードをバルク マイグレーションをサポートし、パブリック クラウドへのワークロードの移行にまつわる複雑さと移行に掛かる時間を著しく削減することができます。

<!--
## What is Hybrid Cloud Extension (HCX)

Hybrid Cloud Extension abstracts on-premises and cloud resources and presents them to the apps as one continuous hybrid cloud. On this, Hybrid Cloud Extension provides high-performance, secure and optimized multisite interconnects. The abstraction and interconnects create infrastructure hybridity. Over this hybridity, Hybrid Cloud Extension facilitates secure and seamless app mobility and disaster recovery across on-premises vSphere platforms and VMware Clouds. Hybrid Cloud Extension is a multi-site, multi cloud service, facilitating true hybrid cloud.
-->

## Hybrid Cloud Extension (HCX) とは何か

Hybrid Cloud Extension は、オンプレミスとクラウドのリソースを抽象化し、一つの連続したハイブリッド クラウドとしてアプリケーションに提供されます。Hybrid Cloud Extension は、ハイパフォーマンスで、安全で、最適化されたマルチサイトのインターコネクトを提供します。この抽象化とインターコネクトにより、インフラストラクチャのハイブリッド性が成されます。Hybrid Cloud Extension は、安全でシームレスなアプリケーションのモビリティと、オンプレミスの vSphere プラットフォームと VMware Cloud 間の災害対策を容易にします。Hybrid Cloud Extension は、マルチ サイトのサービスであり、マルチ クラウドのサービスでもあり、真のハイブリッド クラウドを実現します。

<!--
### Hybrid Cloud Extension Features

Any-to-Any vSphere Cloud App Mobility

Eliminate the need for cloud readiness and app dependency assessment

- Rapidly move existing workloads from a vSphere platform to the latest SDDC
- Reduce upfront planning time for cost and resource analysis
- Accelerate cloud adoption and avoid retrofitting on-premises environment Business Continuity with Lower TCOBusiness Continuity with Lower TCO
- IP and MAC address remapping is not required
- No need to retrofit existing VM environment
- Hybrid Cloud Extension provides warm and cold bulk migration, and bidirectional migration
- Hybrid Cloud Extension simplifies your operational model Architected for SecurityArchitected for Security
- Ensure highly secure tethering of private and public clouds
- Protect resources with resilient disaster recovery capabilities
- Hybrid Cloud Extension hybrid DMZ enables portability of enterprise network and security practices to the cloud
- Security policies migrate with applications High-Performance Infrastructure HybridityHigh-Performance Infrastructure Hybridity
- In-built WAN optimization is tuned for the needs of hybrid use cases
- Hybrid Cloud Extension provides agile, intelligent routing
- Traffic load balancing overlay is policy-enforced
- Multiple VM migration models (including vMotion, warm, cold) make it easy to migrate to and from the cloud without any changes
-->

### Hybrid Cloud Extension の機能

- Any-to-Any の可搬性
  - クラウド化の準備とアプリケーションの依存関係のアセスメントを不要にします
  - vSphere プラットフォームから最新の SDDC へ既存のワークロードを迅速に移動させます
  - コストやリソースの分析のための事前の計画時間を削減します
  - クラウドへの適用を加速し、オンプレミス環境の改修を避けることができます
- 低い TCO でのビジネス継続性
  - IP アドレス、MAC アドレスの再設定が不要
  - 既存の仮想マシン環境の改修が不要
  - Hybrid Cloud Extension は、ウォーム バルク マイグレーションとコールド バルク マイグレーションを提供し、さらには双方向のマイグレーションも提供します
  - Hybrid Cloud Extension は運用モデルをシンプルにします
- セキュリティを踏まえた設計
  - プライベート クラウドとパブリック クラウドの接続をよりセキュアに実現
  - 回復性のある災害対策機能によるリソースの保護
  - Hybrid Cloud Extension の Hybrid DMZ は、エンタープライズのネットワークの可搬性とクラウドへのセキュリティのプラクティスを可能にします。
  - アプリケーションと共にセキュリティ ポリシーの移行
- ハイパフォーマンスなインフラストラクチャのハイブリッド性
  - ハイブリッドなユースケースのニーズのためのビルトインされた WAN 最適化
  - Hybrid Cloud Extension は俊敏性のあるインテリジェントなルーティングを提供
  - トラフィックがロードバランシングされたオーバーレイはポリシーが適用されます
  - 複数の仮想マシンの移行モデル (vMotion、ウォーム、コールド) は変更無しでクラウドから、そしてクラウドへの移行を容易に。

<!--
## HCX - vMotion Migration

1. Open your Chrome browser and click on the **HCX-vMotion** bookmark
2. Click the **X** on the right pane to enlarge the main screen The first tab in the browser demonstrate an on-premises vCenter server.

3. Click on the second tab, this represents a second data center (This can also represent a VMware Cloud on AWS vCenter)
4. Click the first tab in the browser to go back to the on-premises vCenter.
5. Click on the VM named **Mission Critical Workload 1**
6. Click on the **Console screen**

    A console window is now open for the **Mission Critical Workload 1** VM, it will try to ping IP Address 10.159.137.212 which corresponds to a VM in the second site.
7. Click in the tab corresponding to the second site
8. Click on the VM named **TargetSite-TestVM**
9. Note the IP address corresponding to this VM is the IP address that the **Mission Critical Workload 1** VM on the source site is trying to ping
10. Click on the **Mission Critical Workload 1** tab
    - Press enter after the ping command
    - Click **Control-C** to stop ping
    - Type ping 172.16..4.2 which is this VM's own IP address
11. Click the **X** on this tab to close this tab on your browser
12. Click on the first tab to go back to the on-premises vCenter
13. Click on the **Actions** button
14. Click on **Hybridity Actions**
15. Click on **Migrate to the Cloud**
16. Click on **(Specify Destination Container)**
17. Select **RedwoodCluster**
18. Click on **Select Destination** button
19. Click on **(Select Storage)** button
20. Select **cloudStorage**
21. Click **(Select Virtual Disk Format)**
22. Select **Same format as source**
23. Click on **(Select Migration Type)**
24. Select **vMotion**
25. Click **Next**
26. Look for the **Validation is Successful** message
27. Click **Finish** button
28. Click on the **Home** button
29. Click **HCX** in the left pane
30. Click the **Migration** tab
31. Notice the progress of the vMotion migration
32. Click on the **Refresh** tab to update the progress
33. Once progress has completed, click on the second tab to open the target site's vCenter
34. You now see that **Mission Critical Workload 1** has successfully migrated to the Target Site, click on its name
35. Click on the Console window
    - You can see that the ping you left running in a previous step never stopped, successfully vMotioning the VM with zero downtime
    - Press **Control-C** to stop the ping
36. Click on the **X** on the browser tab to close the console tab
-->

## HCX - vMotion マイグレーション

1. Chrome ブラウザを起動し、 **HCX-vMotion** ブックマークをクリックします。
2. メイン スクリーンを最大化するために、右上の **-** をクリックします。ブラウザの最初のタブはオンプレミスの vCenter になります
3. 2 つ目のタブをクリックします。これは 2 つ目のデータセンター (VMware Cloud on AWS の vCenter) になります。
4. ブラウザの最初のタブをクリックし、オンプレミスの vCenter を表示します
5. **Mission Critical Workload 1** という名前の仮想マシンをクリックします。
6. **Console screen** をクリックします。仮想マシン **Mission Critical Workload 1** のコンソール ウィンドウが開きました。2 つ目のサイトの VM に対応する IP アドレス  10.159.137.212 へ ping を打ちます
7. 2 つ目のサイトに対応するタブをクリックします
8. **TargetSite-TestVM** という名前の仮想マシンをクリックします
9. この仮想マシンの IP アドレスが、ソースサイトの仮想マシン **Mission Critical Workload 1** が ping を行っている先の IP アドレスであることを確認します
10. **Mission Critical Workload 1** タブをクリックします
    - ping コマンドの後に Enter を押下します
    - **Control-C** で ping を停止します
    - ping 172.16.4.2 とタイプします。IP アドレスはこの仮想マシンの IP アドレスになります
11. このタブを閉じるために **X** をクリックします
12. 最初のタブをクリックし、オンプレミスの vCenter に戻ります
13. **Actions** ボタンをクリックします
14. **Hybridity Actions** をクリックします
15. **Migrate to the Cloud** をクリックします
16. **(Specify Destination Container)** をクリックします
17. **RedwoodCluster** を選択します
18. **Select Destination** ボタンをクリックします
19. **(Select Storage)** ボタンをクリックします
20.  **cloudStorage** を選択します
21. **(Select Virtual Disk Format)** をクリックします
22. **Same format as source** を選択します
23. **(Select Migration Type)** をクリックします
24. **vMotion** を選択します
25. **Next** をクリックします
26. **Validation is Successful** メッセージを確認します
27. **Finish** ボタンをクリックします
28. **Home**  ボタンをクリックします
29. 左ペインにて **HCX** クリックします
30. **Migration** タブをクリックします
31. vMotion による移行の進捗を確認します
32. **Refresh** タブをクリックし進捗をアップデートします
33. vMotion が完了した後、2 つ目のタブでターゲット サイトの vCenter を開きます
34. **Mission Critical Workload 1** がターゲット サイトに無事移行されたことを確認できます。**Mission Critical Workload 1** をクリックします
35. コンソール タブを開きます
    - 前の手順で実行した ping がそのまま実行されていることを確認します。仮想マシンが vMotion による無停止での移行に成功しました。
    - **Control-C** を押し ping を停止させます
36. ブラウザの **X** をクリックしコンソール タブを閉じます

<!--
### Reverse Migration

1. Click on the first tab in the browser
2. Click the **Migrate Virtual Machines** button
3. Click the **Reverse Migration** checkbox
4. Click the checkbox for **Mission Critical Workload 1**
5. Click on **(Specify Destination Container)**
6. Select **Tier 0 Workloads**
7. Click **Select Destination** button
8. Click on **(Select Storage)**
9. Select **onpremStorage**
10. Click **(Select Virtual Disk Format)**
11. Select **Same format as source**
12. Click on **(Select Migration Type)**
13. Select **vMotion**
14. Click **Next**
15. Once **Validation is Successful** message appears
16. Click **Finish** button
17. Check the progress of the migration
18. Click **Refresh** button
19. Click the **Home** button
20. Click **Hosts and Clusters** button
21. Click on the **Mission Critical Workload 1** VM, you can see that the Reverse migration to the on-premises vCenter was successful
-->

### 逆方向のマイグレーション
 
1. ブラウザの最初のタブをクリックします
2. **Migrate Virtual Machines** ボタンをクリックします
3. **Reverse Migration** チェック ボックスをクリックします
4. **Mission Critical Workload 1** のチェック ボックスをクリックします
5. **(Specify Destination Container)** をクリックします
6. **Tier 0 Workloads** を選択します
7. **Select Destination** ボタンをクリックします
8. **(Select Storage)** をクリックします
9. **onpremStorage** を選択します
10. **(Select Virtual Disk Format)** をクリックします
11. **Same format as source** を選択します
12. **(Select Migration Type)** をクリックします
13. **vMotion** を選択します
14. **Next** をクリックします
15. **Validation is Successful** メッセージが表示されます
16. **Finish** ボタンをクリックします
17. マイグレーションの進捗を確認します
18. **Refresh** ボタンをクリックします
19. **ホーム** ボタンをクリックします
20. **ホストおよびクラスタ** ボタンをクリックします
21. **Mission Critical Workload 1** 仮想マシンをクリックし、オンプレミスの vCenter へ逆方向のマイグレーションが成功していることを確認します

<!--
## HCX - Bulk Migration

1. In your Chrome browser click the **HCX-Bulk** bookmark
2. Click on the **X** to enlarge the main screen
3. As with the vMotion module, in this example we also have a source site (on-premises) vCenter
4. Click on the second tab in the browser to view the Target site vCenter
5. Click on the tab for the on-premises vCenter
6. Click on the **Home** button
7. Click **HCX**
8. Click on the **Migration** tab
9. Click the **Migrate Virtual Machines** button
10. Select **Tier 1 Workloads** in the left pane
11. Click the **checkbox** to select all VM's
12. Click **(Specify Destination Container)**
13. Select **RedwoodCluster** as the Destination Container
14. Click the **Select Destination** button
15. Click **(Select Storage)**
16. Select **cloudStorage**
17. Click **(Select Virtual Disk Format)**
18. Select **Same format as source**
19. Click **(Select Migration Type)**
20. Select **Bulk Migration**
21. Click on **HelpDesk Workload 1** to see the options for this workload
22. Click on **HelpDesk Workload 2** to see the options for this workload
23. Click on the **sidebar** to view all the options
24. Click the **Next** button
25. Wait for **Validation is Successful** message
26. Click **Finish** button
27. Check Progress of the migrations
28. Click **Refresh** button
29. Once migrations complete, click the **Hosts and Clusters** button
30. Click on the second tab to view the cloud vCenter
31. You can see that both workloads have successfully migrated to the Target vCenter
-->

## HCX - バルク マイグレーション

1. Chrome ブラウザで **HCX-Bulk** ブックマークをクリックします
2. **-** をクリックしてメイン スクリーンを最大化します
3. vMotion のモジュールのように、この例でもソース サイト (オンプレミス) の vCenter があります
4. ブラウザの二つ目のタブをクリックし、ターゲット サイトの vCenter を確認します
5. オンプレミスの vCenter のタブをクリックします
6. **ホーム** ボタンをクリックします
7. **HCX** をクリックします
8. **Migration** タブをクリックします
9. **Migrate Virtual Machines** button ボタンをクリックします
10. 左ペインの **Tier 1 Workloads** を選択します
11. 全ての仮想マシンを選択するために **checkbox** をクリックします
12. **(Specify Destination Container)** をクリックします
13. 行き先のコンテナとして **RedwoodCluster** を選択します
14. **Select Destination** ボタンをクリックします
15. **(Select Storage)** をクリックします
16. **cloudStorage** を選択します
17. **(Select Virtual Disk Format)** をクリックします
18. **Same format as source** を選択します
19. **(Select Migration Type)** をクリックします
20. **Bulk Migration** を選択します
21. ワークロードのオプションを確認するために **HelpDesk Workload 1** をクリックします
22. ワークロードのオプションを確認するために **HelpDesk Workload 2** をクリックします
23. 全てのオプションを確認するために **sidebar** をクリックします
24. **Next** ボタンをクリックします
25. **Validation is Successful** メッセージが表示されるのを待機します
26. **Finish** ボタンをクリックします
27. マイグレーションの進捗を確認します
28. **Refresh** ボタンをクリックします
29. マイグレーションが完了したら、**ホストおよびクラスタ** ボタンをクリックします
30. クラウド側の vCenter を確認するために、2つ目のタブをクリックします
31. 両方のワークロードがターゲットの vCenter への移行が成功したことを確認します