# IPFS
Distributed Web : A peer-to-peer hypermedia protocol to make the web faster, safer, and more open.


## P2P Private Network IPFS

IPFS 프라이빗 네트워크 구성하기!!!

* 준비 사항  
Ubuntu 18.04  
server1 : 192.168.0.1  
server2 : 192.168.0.2  
server3 : 192.168.0.3  

* IPFS 다운 및 설치 (Server 1,2,3)  
```bash
$ cd /usr/local/src/
$ sudo wget https://dist.ipfs.io/go-ipfs/v0.4.18/go-ipfs_v0.4.18_linux-amd64.tar.gz
$ sudo tar xvfz go-ipfs_v0.4.18_linux-amd64.tar.gz
$ cd go-ipfs/
$ sudo ./install.sh
```

* Initialize the repository (Server 1,2,3)
```bash
$ ipfs init

initializing ipfs node at /Users/jbenet/.go-ipfs
generating 2048-bit RSA keypair...done
peer identity: Qmcpo2iLBikrdf1d6QU6vXuNb6P7hwrbNPW9kLAH8eG67z
to get started, enter:

  ipfs cat /ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/readme
```

* 서비스 등록 (Server 1,2,3)
```bash
$ sudo vi /etc/systemd/system/ipfs.service

[Unit]
Description=IPFS Daemon
#After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
#Type=simple
ExecStart=/usr/local/bin/ipfs daemon
Restart=always
User=ubuntu
Group=ubuntu

[Install]
WantedBy=multi-user.target
```

* config setting (server 1,2,3)
```bash
$ ipfs config Addresses.Gateway /ip4/0.0.0.0/tcp/8080
$ ipfs config Addresses.API /ip4/0.0.0.0/tcp/5001
```

* public에 연결된 peer 확인 (Server 1,2,3)
```bash
$ ipfs swarm peers

/ip4/104.131.131.82/tcp/4001/ipfs/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
/ip4/104.236.151.122/tcp/4001/ipfs/QmSoLju6m7xTh3DuokvT3886QRYqxAzb1kShaanJgW36yx
/ip4/134.121.64.93/tcp/1035/ipfs/QmWHyrPWQnsz1wxHR219ooJDYTvxJPyZuDUPSDpdsAovN5
/ip4/178.62.8.190/tcp/4002/ipfs/QmdXzZ25cyzSF99csCQmmPZ1NTbWTe8qtKFaZKpZQPdTFB
```

* public peer 삭제 및 확인
```bash
$ ipfs bootstrap rm —all
$ ipfs swarm peers
```

* private node 환경변수 설정 (server 1,2,3)
```bash
Server 1,2,3
$ export LIBP2P_FORCE_PNET=1
```

* ipfs 시작 및 상태확인
```bash
$ sudo systemctl restart ipfs
$ sudo systemctl status ipfs
```

* bootnode 확인 (server 1)
```bash
$ ipfs id
```

* 연결 node 등록 및 연결확인 (server 2,3)
```bash
$ ipfs bootstrap add /ip4/192.168.0.1/tcp/4001/ipfs/QmWbvXsKS4ytbPXYAcpT6ETusL6dnZsJsDdLgqxdo11NyP

$ ipfs swarm peers
```
