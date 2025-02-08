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

## Operation

1. Execution, Consensus, Validaotr, MEV-Boost 프로세스가 다운 된 경우

```
$ docker ps

CONTAINER ID   IMAGE              COMMAND                  CREATED        STATUS        PORTS                                                                                                              NAMES
55073ed61e89   nethermind:local   "docker-entrypoint.s…"   12 hours ago   Up 12 hours   8545/tcp, 8551/tcp, 0.0.0.0:30303->30303/tcp, 0.0.0.0:30303->30303/udp, :::30303->30303/tcp, :::30303->30303/udp   eth-docker-execution-1
250d9eea24b4   lighthouse:local   "docker-entrypoint-v…"   12 hours ago   Up 12 hours                                                                                                                      eth-docker-validator-1
f51abab548f9   lighthouse:local   "docker-entrypoint.s…"   12 hours ago   Up 12 hours   0.0.0.0:9000->9000/tcp, 0.0.0.0:9000-9001->9000-9001/udp, :::9000->9000/tcp, :::9000-9001->9000-9001/udp           eth-docker-consensus-1
343167d6cacb   mev-boost:local    "/app/mev-boost -add…"   12 hours ago   Up 12 hours   18550/tcp                                                                                                          eth-docker-mev-boost-1
```

- 정상적인 상황이라면, 위와 같이 4개의 도커 컨테이너가 떠있어야함

```
ethd restart <execution, consensus, validator, mev-boost>
ethd start <execution, consensus, validator, mev-boost>
```

- 4개의 도커 컨테이너중 하나라도 다운이 되어있으면 재시작

2. 디스크에 여유 공간이 없는 경우

```
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       290G  177G  114G  61% /
```

- df 명령어로 확인해보고 Use가 100% 인경우 노드가 싱크를 받을 수 없는 상황
- 디스크의 볼륨을 증가시킬 수 있으면, 증가
- 디스크의 볼륨을 증가시킬 수 없다면, Pruning 또는 다시 싱크를 받아야함
- 수시로 디스크의 사용량을 모니터링 해주는게 좋음
