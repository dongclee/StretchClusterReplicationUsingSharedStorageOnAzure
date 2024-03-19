# StretchClusterReplicationUsingSharedStorageOnAzure
안녕하세요,,, 오래 간만에 포스팅합니다.
제가 최근에 아래와 같은 고객의 요청을 받았습니다.

“Pure Microsoft 기술 만으로 Hyper-V 클러스터의 Disaster Recovery를 구현해 주세요”

그래서, 제가 처음 접해본 “Storage Replica” 기술을 기반으로 고객의 요청 사항을 처리해 보았습니다. Storage Replica 때문에, 간만에 Step by Step 가이드를 한 번 만들어 보았습니다.

위 고객의 요청 사항을 처리하기 위해서 사용된 자료 및 기술은 아래와 같습니다.

“Stretch cluster replication using shared storage | Microsoft Learn (https://learn.microsoft.com/en-us/windows-server/storage/storage-replica/stretch-cluster-replication-using-shared-storage)”

•	Hyper-V

•	Multisite Failover Clustering

•	Storage Replica

•	iSCSI Target

•	File Server


데모 환경은 아래와 같이 구현했습니다.

제가 물리 Hyper-V 호스트를 가지고, 아래 데모 환경을 구성할 수 없어서, 모든 데모 환경을 Azure에서 구현해 보았음을 참조해 주셨으면 합니다.

![demo](https://github.com/dongclee/StretchClusterReplicationUsingSharedStorageOnAzure/assets/42400574/3bd552e8-8af9-4e76-afa4-795fb4e974d6)

위 그림에서 표현되지 않았지만, 추가적인 VM을 포함해서 데모 환경 구현에 필요한 VM 항목은 아래와 같습니다.
그리고, 각 Hyper-V 호스트에 NIC을 1개만 장착했습니다. (데모 환경 단순화, 실제 환경에서는 트래픽을 고려하여 여러 개의 NIC이 필요할 수 있습니다.)

<img width="673" alt="VM" src="https://github.com/dongclee/StretchClusterReplicationUsingSharedStorageOnAzure/assets/42400574/8bbab50a-e205-4b01-bd05-7ff869befe3c">


데모 환경의 동작은 아래와 같습니다.

1.	Redmond 사이트에 존재하는 SR-SRV01 및 SR-SRV02 Hyper-V 호스트에만 연결 가능한 공유 스토리지를 iSCSI로 구성합니다.
2.	Bellevue 사이트에 존재하는 SR-SRV03 및 SR-SRV04 Hyper-V 호스트에만 연결 가능한 공유 스토리지를 iSCSI로 구성합니다.
3.	SR-SRV01, SR-SRV02, SR-SRV03 및 SR-SRV04는 단일 Multisite Failover Clustering으로 구성합니다.
4.	Redmond 사이트에 존재하는 SR-SRV01 및 SR-SRV02 Hyper-V 호스트에만 연결 가능한 공유 스토리지에는 Storage Replica 기술을 위해서, 원본(Source) 데이터 용도의 볼륨 1개, 원본로그 용도의 볼륨 1개를 구성했습니다.
5.	Bellevue 사이트에 존재하는 SR-SRV03 및 SR-SRV04 Hyper-V 호스트에만 연결 가능한 공유 스토리지에는 Storage Replica 기술을 위해서, 대상(Destination) 데이터 용도의 볼륨 1개, 원본로그 용도의 볼륨 1개를 구성했습니다.
6.	Redmond 사이트에 존재하는 SR-SRV01 및 SR-SRV02 Hyper-V 호스트의 공유 저장소에 테스트 VM을 생성합니다.
7.	6번에서 생성한 테스트 VM의 관련 데이터 (각종 VM 관련 파일)가 Storage Replica 기술을 사용하여, Bellevue 사이트에 존재하는 SR-SRV03 및 SR-SRV04에 연결된 대상(Destination) 데이터 용도의 볼륨에 자동으로 복제됩니다.
8.	Redmond 사이트가 Disaster Recovery 상황일 경우, Bellevue 사이트에서 VM을 복구할 수 있습니다.

감사합니다.

[Stretch Cluster Replication Using Shared Storage.pdf](https://github.com/dongclee/StretchClusterReplicationUsingSharedStorageOnAzure/files/14651373/Stretch.Cluster.Replication.Using.Shared.Storage.pdf)
