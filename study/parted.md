[[study]]
# parted

2TB 이상의 디스크의 파티션 테이블 변경, 파티션 생성 할때는 `parted` 명령어를 사용


### 주의 사항
- parted 명령어는 fdisk 명령어와 다르게 설정 즉시 디스크에 바로 적용됨

## 파티션 생성하기
/dev/sdb 3TB 디스크를 이용하여 작업을 진행

```bash
[root@localhost ~]# parted 
GNU Parted 3.1 
Using /dev/sda Welcome to GNU Parted! Type 'help' to view a list of commands. 
(parted)
```

parted 명령어를 입력하면 자동으로 맨 앞에 있는 디스크가 선택
아니면, `prated /dev/sdb` 이런 식으로 입력하여 설정할 디스크 지정

```bash
(parted) print all
```

설정 모드에서 `print all`을 입력하면 현재 디스크와 파티션에 대한 정보를 확인 할 수 있음

```bash
(parted) print all 
Error: /dev/sdb: unrecognised disk label 
Model: HP LOGICAL VOLUME (scsi) 
Disk /dev/sdb: 3001GB 
Sector size (logical/physical): 512B/512B 
Partition Table: unknown 
Disk Flags: 

Model: HP LOGICAL VOLUME (scsi)
Disk /dev/sda: 300GB 
Sector size (logical/physical): 512B/512B 
Partition Table: msdos Disk Flags: 

Number Start End    Size   Type    File system    Flags 
 1    1049kB 1075MB 1074MB primary ext4           boot 
 2    1075MB 11.8GB 10.7GB primary linux-swap(v1) 
 3    11.8GB 300GB  288GB  primary ext4 

Model: HP LOGICAL VOLUME (scsi) 
Disk /dev/sdc: 73.4GB 
Sector size (logical/physical): 512B/512B 
Partition Table: msdos 
Disk Flags: 

Number Start  End    Size   Type    File system   Flags 
 1     1049kB 73.4GB 73.4GB primary xfs
```

맨 위에 ``3001GB`` 용량으로 보이는 `/dev/sdb` 디스크가 있는 것을 확인할 수 있음

```bash
(parted) select /dev/sdb
```

/dev/sdb 디스크에 설정을 하기 위해 `select`를 이용하여 `/dev/sdb` 디스크를 지정

```bash
(parted) help mklabel 
   mklabel,mktable LABEL-TYPE create a new disklabel (partition table) 
   
   LABEL-TYPE is one of: aix, amiga, bsd, dvh, gpt, mac, msdos, pc98, sun, loop
```

파티션을 생성하기 전 `디스크 라벨` 변경
`help mklabel` 입력하면 변경 가능한 라벨 목록 확인 가능

```bash
(parted) mklabel gpt
```

`mklabel`를 이용하여 `/dev/sdb`의 디스크 라벨 `GPT`로 변경

```bash
(parted) p 
Model: HP LOGICAL VOLUME (scsi) 
Disk /dev/sdb: 3001GB 
Sector size (logical/physical): 512B/512B
Partition Table: gpt 
Disk Flags: 

Number Start End Size File system Name Flags
```

`print`를 입력하면 `Partition Table` 부분 `gpt`로 변경된 것 확인 가능

```bash
(parted) mkpart
```

`mkpart`로 파티션 생성 진행

```bash
(parted) mkpart
Partition name? []?
File system type? [ext2]?
Start? 1
End? 3001GB
```

`mkpart`를 입력한 후 `Partition name`과 `File system type` 부분은 엔터를 눌러 넘어갈 수 있음 (이름지정 선택사항)

그 뒤에, `Start`와 `End`의 범위 지정. 전체 디스크 용량은 `print`로 확인 가능

```bash
(parted) print 
Model: HP LOGICAL VOLUME (scsi) 
Disk /dev/sdb: 3001GB 
Sector size (logical/physical): 512B/512B 
Partition Table: gpt 
Disk Flags: 

Number Start  End    Size   File system Name Flags 
1      1049kB 3001GB 3001GB
```

파티션 설정이 완료되면 `print`를 입력하여 파티션이 새로 생성된 것 확인가능

```bash
quit
```

마지막으로 `quit` 이용하여 설정 모드에서 빠져나감


`parted` 명령얼르 통한 파티션 생성 작업 완료.
이제 해당 파티션에 파일 시스템을 생성한 후 마운트 진행하면 됨