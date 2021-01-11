# VPN Gateway

## VPN이란?
Virtual Private Network는 방화벽, 침입 탐지 시스템과 같이 사용되는 보안 솔루션 중 하나이다.
Network에 안전하게 접속하도록 하며 외부에서 접속하는 경우에도 VPN을 통해서 내부 서버나 네트워크 리소스에 접속할 수 있다.
터널링을 제공해서 원격의 두 지점을 내부 네트워크처럼 이용할 수 있다.
보안을 위해서 VPN 양끝에 Gateway가 필요하다.

## VPN Gateway connection option
1. [Site-to-Site VPN(over IPsec)](https://docs.microsoft.com/ko-kr/azure/vpn-gateway/tutorial-site-to-site-portal)
2. [Point-to-Site VPN(over SSTP)](https://docs.microsoft.com/ko-kr/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
3. [VNet-to-VNet(over IPsec)](https://docs.microsoft.com/ko-kr/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)
  * 다른 Region, 다른 Subscription에 있는 Vnet끼리 연결
4. [Multi Site](https://docs.microsoft.com/ko-kr/azure/vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal)
5. [ExpressRoute](https://docs.microsoft.com/ko-kr/azure/expressroute/expressroute-introduction)



# VPN Gateway
공용 연결을 통해 on-premise와 Azure Vnet사이에 암호화된 트래픽을 보내는 가상 네트워크 게이트웨이이다.
각 Vnet은 오직 1개의 VPN Gateway를 가진다. 1개의 VPN gateway에 여러 개의 connection 가능



