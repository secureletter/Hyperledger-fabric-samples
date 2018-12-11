# Hyperledger-fabric-samples

## cryptogen

cryptogen이라는 tool을 사용하여 인증서와 서명용 키를 생성한다.
cryptogen은 **crypto-config.yaml** 파일을 통해 동작한다.
crypto-config.yaml에는 네트워크 토폴로지에 대한 내용과 참여 조직과 조직의 컴포넌트들의 인증서와 서명용 키 정보가 담겨있다.
Hyperleder Fabric에서는 트랜잭션들이 entity의 개인키로 서명되며 공개키로 검증된다.

```
OrdererOrgs:
#---------------------------------------------------------
# Orderer
# --------------------------------------------------------
- Name: Orderer
  Domain: example.com
  CA:
      Country: US
      Province: California
      Locality: San Francisco
  #   OrganizationalUnit: Hyperledger Fabric
  #   StreetAddress: address for org # default nil
  #   PostalCode: postalCode for org # default nil
  # ------------------------------------------------------
  # "Specs" - See PeerOrgs below for complete description
# -----------------------------------------------------
  Specs:
    - Hostname: orderer
# -------------------------------------------------------
# "PeerOrgs" - Definition of organizations managing peer nodes
 # ------------------------------------------------------
PeerOrgs:
# -----------------------------------------------------
# Org1
# ----------------------------------------------------
- Name: Org1
  Domain: org1.example.com
  EnableNodeOUs: true
```

위의 crypto-config.yaml을 보면, node name은 "{{.Hostname}}.{{.Domain}}"으로 정해진다.
따라서 orderer의 node name은 orderer.example.com 으로 정해진다.

cryptogen을 실행시키고 나면, crypto-config라는 이름의 디렉토리에 새로 생성된 인증서와 서명용 키가 저장된다.

## Configuration Transaction Generator

configtxgen을 이용하면 3개의 configuration artifacts를 만들 수 있다.
* orderer **genesis block**
* channel **configuration transaction**
* **anchor peer transaction**

Orderer genesis block은 ordering service를 위한 genesis block이다.
Channel configuration transaction은 채널 생성 시간에 orderer에게 broadcast하기 위한 파일이다.
The anchor peer transaction은 이 채널에 각 조직의 anchor peer를 알리기 위한 파일이다.

configtxgen은 configtx.yaml 파일을 사용하고, 이 파일에는 network가 정의되어 있다.

configtx.yaml 파일은 configuration artifacts를 만들때 사용되므로 중요하다.
configtx.yaml 파일에는 두 가지가 포함되어야 한다.
첫번째는, 각각의 Peer 조직의 anchor peer를 명시해주어야 한다.
두번째는, 각각의 멤버의 MSP directory의 위치를 명시해주어야 한다.
이것은 network의 entity들이 통신할 때 전자 서명을 검증해야하기 때문이다.
