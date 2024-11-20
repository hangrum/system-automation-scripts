# 시스템 자동화 스크립트

- 시스템 관리, 자동화, 문제 해결을 위해 개발한 스크립트와 플레이북 모음입니다.
- 시스템 엔지니어링에서 실제로 겪은 문제를 해결하기 위해 만들어졌습니다.
- **ChatGPT 나오기 이전에 작성함**
  - 본 README.md는 약간 힘을 빌렸습니다.

---

## 📂 주요 스크립트

### 🔧 **시스템 관리**
- **`bash/motd-secure.sh`**  
  시스템 MOTD를 비활성화하고 관련 설정을 보안적으로 강화하며 불필요한 메시지를 정리합니다.
- **`bash/set-umask-to-all.sh`**  
  모든 사용자에 대해 `umask 022`를 강제 적용하여 일관된 파일 권한을 유지합니다.

### 🌐 **네트워크 및 API**
- **`python/pause_prtg.py`**  
  호스트 이름을 기반으로 PRTG 장치를 API 통합을 통해 일시 중지 또는 재개합니다.
- **`bash/ipmi-sel-clean.sh`**  
  IPMI SEL 로그를 정리하고 시스템 시계를 동기화하여 하드웨어 상태를 유지합니다.

### 🛠️ **DevOps 및 자동화**
- **`ansible/update-ca-certificates.yml`**  
  Ansible을 사용하여 CA 인증서를 업데이트하고, Comodo AddTrust 만료와 같은 문제를 해결합니다.
- **`python/check_ceph_daemons.py`**  
  [README.md](./python/README.md)  
  Ceph 데몬 상태를 모니터링하고 비정상 데몬을 자동으로 재시작하여 클러스터 안정성을 보장합니다.

### 📊 **고성능 작업**
- **`bash/s3-log-structure-update.sh`**  
  작성 동기:
    - S3 access log 데이터가 계속 쌓이지 않음
    - 확인 결과 기존 파일경로 재구성 스크립트가 속도를 따라가지 못함
    - 기존 누락되어 밀린 파일까지 재구성 하기 위해 병렬 처리 필요
  
  S3 로그 파일 경로를 병렬로 처리 및 재구조화하여 대량 로그 데이터를 효율적으로 처리합니다.
  - **스레드 변수로 최대 스레드 수 지정** 
    - 기본값은 5로 설정. 파일이 5개 이하일 경우 자동으로 스레드 수를 줄임.
    - 파일이 5개 이상일 경우, 파일 목록을 최대 5개의 스레드로 고르게 분배.
  - **S3에서 파일 목록 생성 및 분배**  
    - S3에서 대상 파일(`access-log202`)의 전체 목록을 가져오고 `split` 명령어로 병렬 처리를 위한 소규모 목록으로 분할.
  - **새로운 구조로 복사, 검증 후 원본 삭제**  
    - 파일을 새로운 디렉토리 구조로 복사하고, 검증 후 원본 파일을 삭제.

  주요 변수
  - **BUCKET**: S3 버킷 주소.
  - **THREAD**: 실행 가능한 최대 스레드 수.
  - **FILE_DIR**: S3 파일 목록을 임시 저장하는 디렉토리.
  - **LOG_DIR**: 로그 파일을 저장할 디렉토리.
