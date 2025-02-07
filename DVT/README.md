# DVT 

## Useful link

- Obol distributed validator node: https://github.com/ObolNetwork/charon-distributed-validator-node
- Holesky Obol Launchpad: https://holesky.launchpad.obol.org/
- Lido CSM testnet widget: https://csm.testnet.fi/
- Official Lido CSM docs: https://operatorportal.lido.fi/modules/community-staking-module

## Step by step

1. Obol distributed validator node 다운로드

```
git clone https://github.com/ObolNetwork/charon-distributed-validator-node.git
```

2. 환경 파일 생성

```
cd charon-distributed-validator-node
mkdir .charon
cp .env.sample.holesky .env
```

3. ENR (Ethereum Node Record) 생성

- 모든 클러스터 멤버는 Obol 네트워크에 연결하기 위해 ENR(Ethereum Node Record)이 필요

```
docker compose run -u root --rm charon create enr
```

- ENR 기록

4. DV cluster 생성

- 리더 선정, 리더가 클러스터 생성
- https://holesky.launchpad.obol.org/

5. Create a cluster with a group 선택

![image](https://github.com/user-attachments/assets/9b6205c0-4b79-4c45-a82c-70bb61c535dc)

- Cluster Operator 주소
- Validator 갯수 선택
- 본인 ENR
- 참여자 주소
- 생성할 Validator 갯수

6. Confirm Configuration

7. Invite Operators

![image](https://github.com/user-attachments/assets/01098c8c-6e03-4876-af02-3e4234c15426)

- 모든 Operator들에게 초대 코드를 전송해서 위에 입력한 Wallet으로 ENR 입력

8. DKG

![image](https://github.com/user-attachments/assets/6228a485-ef1b-4cce-b740-1a8cc4867997)

![image](https://github.com/user-attachments/assets/9390d383-f2e7-408c-bd2d-a863e02a7909)

```
docker run -u root --rm -v 
"$(pwd)/:/opt/charon" obolnetwork/charon:v1.2.0 dkg --definition-file="https://api.obol.t
ech/v1/definition/0x5c869d7d4c25606c70679db8aa58859cbd1d6aebd235636ac66a07855ebdf98d" --p
ublish
```

9. Start Obol

```
docker compose up -d
```
