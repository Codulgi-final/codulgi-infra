# 인프라 관리 툴

# 프로젝트 개요
- 목적: Docker 기반 인프라를 관리하고 모니터링할 수 있는 웹 기반 대시보드를 개발하여, 인프라의 상태 확인, Docker Compose 파일 관리, 컨테이너 제어, 리소스 모니터링 등을 효율적으로 수행.
- 기술 스택: FastAPI (백엔드 API), Streamlit (프론트엔드 대시보드)

---
# 주요 기능 목록

## 1. 컨테이너 상태 조회
- **기능 설명**: 현재 실행 중인 Docker 컨테이너 목록을 조회하고 상태를 확인.
- **세부 기능**:
  - 컨테이너 ID, 이름, 상태(실행 중/중지), 포트, 실행 시간 표시.
  - 실시간 상태 업데이트를 통해 최신 정보를 제공.
- **사용 API**: `/containers` (FastAPI GET 엔드포인트)
- **프론트엔드 구현**: Streamlit 테이블에 상태 표시, 일정 주기로 새로고침하여 업데이트.

## 2. 컴포즈 파일 관리
- **기능 설명**: Docker Compose 파일의 목록화, 생성, 수정, 삭제 기능 제공.
- **세부 기능**:
  - **목록 보기**: 서버에 있는 Docker Compose 파일의 목록을 Streamlit에 표시.
  - **파일 생성**: 새로운 컴포즈 파일을 생성하고 저장.
  - **파일 수정**: 기존 컴포즈 파일 내용을 수정할 수 있는 에디터 제공.
  - **파일 삭제**: 필요 없는 컴포즈 파일을 삭제.
  - 파일 내용을 미리 보고, 에디터로 수정하는 기능 제공.
- **사용 API**: `/compose-files` (GET, POST, PUT, DELETE 엔드포인트)
- **프론트엔드 구현**: Streamlit에서 파일 목록과 편집기를 통해 관리.

## 3. 컴포즈 파일 `up` / `down` 제어
- **기능 설명**: Docker Compose 파일을 사용하여 컨테이너를 시작(`up`)하거나 중지(`down`)하는 기능.
- **세부 기능**:
  - 특정 컴포즈 파일에 대해 `up` 명령으로 컨테이너 시작.
  - 특정 컴포즈 파일에 대해 `down` 명령으로 컨테이너 중지.
  - 명령 실행 후 결과를 JSON 형식으로 반환해 로그나 성공/실패 상태 표시.
- **사용 API**:
  - `/compose-files/{file_name}/up` (POST 엔드포인트)
  - `/compose-files/{file_name}/down` (POST 엔드포인트)
- **프론트엔드 구현**: Streamlit에서 각 컴포즈 파일 옆에 `up`, `down` 버튼을 배치하여 제어.

## 4. 컨테이너 리소스 상태 모니터링
- **기능 설명**: 각 컨테이너의 CPU, 메모리 사용량을 실시간으로 모니터링.
- **세부 기능**:
  - 각 컨테이너의 CPU 사용률, 메모리 사용률 등을 실시간 차트로 표시.
  - 리소스 사용량 과부하 시 알림 기능 제공.
- **사용 API**: `/containers/{container_id}/stats` (GET 엔드포인트)
- **프론트엔드 구현**: Streamlit에서 리소스 상태를 실시간 차트로 표시.

## 5. 로그 조회
- **기능 설명**: 각 컨테이너의 실시간 로그를 조회하고, 로그 필터링 기능 제공.
- **세부 기능**:
  - 각 컨테이너별로 `docker logs` 데이터를 실시간으로 표시.
  - 특정 키워드 또는 날짜 범위로 로그를 필터링.
- **사용 API**: `/containers/{container_id}/logs` (GET 엔드포인트)
- **프론트엔드 구현**: Streamlit에 텍스트 뷰어를 제공하여 로그를 실시간 스트리밍.

## 6. 컨테이너 네트워크 관리
- **기능 설명**: Docker 네트워크 상태 확인 및 네트워크 생성, 수정, 삭제 기능 제공.
- **세부 기능**:
  - **네트워크 목록 조회**: 생성된 네트워크 이름, 드라이버 유형, 연결된 컨테이너 리스트 표시.
  - **네트워크 생성**: 새로운 네트워크를 생성하고 설정 추가.
  - **네트워크 수정**: IP 범위, 서브넷, 게이트웨이 주소 등의 네트워크 설정을 수정.
  - **네트워크 삭제**: 사용하지 않는 네트워크 삭제.
  - 네트워크 트래픽 모니터링과 네트워크 간 통신 제어.
- **사용 API**: `/networks` (GET, POST, PUT, DELETE 엔드포인트)
- **프론트엔드 구현**: Streamlit에 네트워크 상태와 제어 버튼 추가.

## 7. 알림 시스템
- **기능 설명**: 컨테이너 상태 변화, 네트워크 오류, 리소스 과부하 등의 이벤트 발생 시 알림 제공.
- **세부 기능**:
  - **실시간 알림**: 컨테이너가 비정상 종료되거나 문제가 발생했을 때 알림.
  - 이메일, Slack, 또는 Streamlit 대시보드 상에서 알림 표시.
- **프론트엔드 구현**: Streamlit에서 알림 메시지를 실시간으로 표시하는 UI 구성.

## 8. 웹 기반 대시보드 통합
- **기능 설명**: 전체 기능을 통합한 대시보드를 제공하여 모든 기능을 한눈에 확인하고 제어 가능.
- **세부 기능**:
  - 컨테이너 목록, 컴포즈 파일 관리, 네트워크 상태, 리소스 모니터링을 한 화면에서 관리.
  - 버튼으로 `up`, `down`, `start`, `stop`, `restart` 등의 명령을 쉽게 실행.
  - 전체 시스템의 실시간 모니터링과 직관적인 시각화 제공.
- **프론트엔드 구현**: Streamlit에서 모든 기능을 통합하여 대시보드 형태로 구성.