# Ethereum Validator Setup on Holesky Hand-On Session

## Useful link

- Google Cloud: https://cloud.google.com/free
- Eth Docker: https://github.com/eth-educators/eth-docker
- Lido CSM testnet widget: https://csm.testnet.fi/
- Official Lido CSM docs: https://operatorportal.lido.fi/modules/community-staking-module

## Step by step

1. 구글 클라우드 어카운트 가입
2. 구글 클라우드 콘솔에서 VM 생성 및 SSH 접속
   - e2-standard-4, Ubuntu 24.04 LTS x86/64, Disk 300GB +
3. User에게 sudo 권한 부여
 - `<User Name>`을 본인 User name으로 변경 후 입력
```
sudo su
usermod -aG sudo <User Name>
su - <User Name>
```

4. Download Eth Docker

```
cd ~ && git clone https://github.com/eth-educators/eth-docker.git && cd eth-docker
```

5. 이더리움 노드를 운영하기위해 필요한 요소 설치

```
./ethd install
source ~/.profile
sudo usermod -a -G docker $USER
```

6. 이더리움 노드를 운영하기 위한 config 설정
```
./ethd config
```
   - `Holešovice Testnet`
   - `Lido-compatible node (Community Staking / Simple DVT)`
   - `[Community Staking] CSM node - Consensus, execution and validator client`
   - 원하는 EL client 선택
   - 원하는 CL client 선택
   - checkpoint sync 확인
   - relays 선택
   - Grafana dashboards 선택
   - Graffiti를 원하는 경우 입력
   - Validator key 생성
   - Validator key 갯수
   - Validator keystore 암호 입력 (12글자 이상)
   - 새로운 mnemonic으로 생성
   - Withdrawal address 입력
      - withdrawal address: `0xF0179dEC45a37423EAD4FaD5fCb136197872EAd9`
   - Validator keystore 암호
   - mnemonic 언어 선택
   - withdrawal credentials prefix 0x01 (False)
   - mnemonic 저장
   - mnemonic 입력

7. Start Eth Docker

```
ethd start
```

8. Deposit data Upload 하기


```
cat ~/eth-docker/.eth/validator_keys/deposit*
```

- https://csm.testnet.fi/

9. Validator key Import & Validator Restart

```
sudo chown -R $USER:$USER ~/eth-docker/.eth/validator_keys
ethd keys import
ethd restart validator
```
