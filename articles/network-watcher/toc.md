# [Azure Network Watcher のドキュメント](index.md)

# 概要
## [Network Watcher とは](network-watcher-monitoring-overview.md)
## [トポロジ](network-watcher-topology-overview.md)
## [可変パケット キャプチャ](network-watcher-packet-capture-overview.md)
## [ネットワーク セキュリティ グループのフロー ログ](network-watcher-nsg-flow-logging-overview.md)
## [IP flow verify](network-watcher-ip-flow-verify-overview.md)
## [セキュリティ グループ ビュー](network-watcher-security-group-view-overview.md)
## [次ホップ](network-watcher-next-hop-overview.md)
## [VPN 接続のトラブルシューティング](network-watcher-troubleshoot-overview.md)
## [接続のトラブルシューティング](network-watcher-connectivity-overview.md)
## [サービスの制限](../azure-subscription-service-limits.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-watcher-limits)

# 作業の開始
## [Network Watcher の構成](network-watcher-create.md)
## パケット キャプチャの管理
### [Azure Portal](network-watcher-packet-capture-manage-portal.md)
### [Azure PowerShell](network-watcher-packet-capture-manage-powershell.md)
### [Azure CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
### [Azure CLI](network-watcher-packet-capture-manage-cli.md)
### [REST](network-watcher-packet-capture-manage-rest.md)
## VM との間の IP フローの確認
### [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
### [Azure PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
### [Azure CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
### [Azure CLI](network-watcher-check-ip-flow-verify-cli.md)
### [REST](network-watcher-check-ip-flow-verify-rest.md)
## 次ホップの分析
### [Azure Portal](network-watcher-check-next-hop-portal.md)
### [Azure PowerShell](network-watcher-check-next-hop-powershell.md)
### [Azure CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
### [Azure CLI](network-watcher-check-next-hop-cli.md)
### [REST](network-watcher-check-next-hop-rest.md)
## ネットワーク トポロジの表示
### [Azure PowerShell](network-watcher-topology-powershell.md)
### [Azure CLI 1.0](network-watcher-topology-cli-nodejs.md)
### [Azure CLI](network-watcher-topology-cli.md)
### [REST](network-watcher-topology-rest.md)
## VPN Gateway と接続のトラブルシューティング
### [Azure Portal](network-watcher-troubleshoot-manage-portal.md)
### [Azure PowerShell](network-watcher-troubleshoot-manage-powershell.md)
### [Azure CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
### [Azure CLI](network-watcher-troubleshoot-manage-cli.md)
### [REST](network-watcher-troubleshoot-manage-rest.md)
## ネットワーク セキュリティ グループ ビューの分析
### [Azure PowerShell](network-watcher-security-group-view-powershell.md)
### [Azure CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
### [Azure CLI](network-watcher-security-group-view-cli.md)
### [REST](network-watcher-security-group-view-rest.md)
## 接続のトラブルシューティング
### [Azure Portal](network-watcher-connectivity-portal.md)
### [Azure PowerShell](network-watcher-connectivity-powershell.md)
### [Azure CLI](network-watcher-connectivity-cli.md)
### [REST](network-watcher-connectivity-rest.md)
## NSG フロー ログの構成
### [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
### [Azure PowerShell](network-watcher-nsg-flow-logging-powershell.md)
### [Azure CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
### [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
### [REST](network-watcher-nsg-flow-logging-rest.md)

# 方法
## [Azure Automation を使用して VPN Gateway を監視する](network-watcher-monitor-with-azure-automation.md)
## [VPN Gateway を使用してオンプレミスの接続を診断する](network-watcher-diagnose-on-premises-connectivity.md)
## [ネットワーク接続を監視する](connection-monitor.md)
## [PowerShell を使用してネットワークのコンプライアンスと監査を実行する](network-watcher-nsg-auditing-powershell.md)
## [Azure Log Analytics で Application Gateway のログを分析する](../log-analytics/log-analytics-azure-networking-analytics.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)
## パケット キャプチャ
### [異常の検出](network-watcher-deep-packet-inspection.md)
### [Azure Functions によるプロアクティブなネットワーク監視](network-watcher-alert-triggered-packet-capture.md)
### [オープン ソース ツールを使用して不正侵入検出を実行する](network-watcher-intrusion-detection-open-source-tools.md)
### [オープン ソース ツールを使用してネットワーク トラフィック パターンを視覚化する](network-watcher-using-open-source-tools.md)
## NSG フロー ログの分析
### [NSG フロー ログの読み取り](network-watcher-read-nsg-flow-logs.md)
### Traffic Analytics
#### [Traffic Analytics の使用](traffic-analytics.md)
#### [よく寄せられる質問](traffic-analytics-faq.md)
### [Power BI の使用](network-watcher-visualize-nsg-flow-logs-power-bi.md)
### [Elastic Stack の使用](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
### [Grafana の使用](network-watcher-nsg-grafana.md)
### [Graylog の使用](network-watcher-analyze-nsg-flow-logs-graylog.md)
## [場所と Azure リージョン間の相対待ち時間の確認](view-relative-latencies.md)

# リファレンス
## [Azure CLI](/cli/azure/network/watcher)
## [Azure PowerShell](/powershell/module/azurerm.network/#network_watcher)
## [Java](/java/api/com.microsoft.azure.management.network)
## [Ruby](http://www.rubydoc.info/gems/azure_mgmt_network/Azure/Network/Mgmt/V2016_09_01/Models/NetworkWatcher) 
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.mgmt.network.html#azure.mgmt.network.NetworkManagementClient.network_watchers)

## [.NET](/dotnet/api)
## [REST](/rest/api/networkwatcher/)
# 関連項目
## [Virtual Network](/azure/virtual-network/)
## [仮想マシン](/azure/virtual-machines/)
## [VPN Gateway](/azure/vpn-gateway/)
# リソース
## [Azure のロードマップ](https://azure.microsoft.com/roadmap/)
## [MSDN フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork)
## [料金](https://azure.microsoft.com/pricing/details/network-watcher/)
## [サービスの更新情報](https://azure.microsoft.com/updates/?product=network-watcher)
## [SLA](https://azure.microsoft.com/support/legal/sla/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/network-watcher)
