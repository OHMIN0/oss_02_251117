<div align="center">

# 🐧 리눅스 프로세스 관리 명령어 사용 설명
**Linux Process Management Commands Report**

![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)
![Shell](https://img.shields.io/badge/Shell-121011?style=flat-square&logo=gnu-bash&logoColor=white)
![Markdown](https://img.shields.io/badge/Docs-Markdown-000000?style=flat-square&logo=markdown&logoColor=white)

<br>

> **오픈소스SW개론 과제 #2**<br>
> 리눅스 시스템 관리의 핵심인 `top`, `ps`, `jobs`, `kill`의 사용 설명을 위한 README 작성

</div>

---

### 📝 과제 제출 정보
| 항목 | 내용 |
| :---: | :--- |
| **작성자** | 20202980 오민영 |
| **제출일** | 2025년 11월 nn일 |
| **과목명** | 오픈소스SW개론 |

---

## 📑 목차 (Table of Contents)
1. [📊 top - 실시간 시스템 모니터링](#1--top---실시간-시스템-모니터링)
2. [📸 ps - 프로세스 상태 확인](#2--ps---프로세스-상태-확인-snapshot)
3. [⏯️ jobs - 백그라운드 작업 관리](#jobs-section)
4. [🔫 kill - 프로세스 종료 및 제어](#4--kill---프로세스-종료-및-시그널-전송)

---

## 1. 📊 top - 실시간 시스템 모니터링

`top` 명령어
- 리눅스 시스템의 상태를 **실시간(Real-time)**으로 갱신하며 보여주는 대시보드 역할
- Windows의 '작업 관리자'와 가장 유사함.

### 🔹 주요 특징
- **시스템 요약:** Uptime, CPU 부하(Load Average), 메모리 사용량을 상단에 표시
- **실시간 갱신:** 기본 3초 간격으로 프로세스 리스트를 업데이트
- **리소스 분석:** CPU 및 메모리를 과도하게 점유하는 프로세스 식별 가능

### 💻 실행 화면
```bash
$ top
```
> **👇 [실습 스크린샷]**
>
> <img width="796" height="227" alt="image" src="https://github.com/user-attachments/assets/0d0597d8-6538-4bba-a68e-4c2d6cecf37c" />



### 🔍 화면 상세 분석 (Screen Analysis)
`top` 실행 화면은 크게 상단의 **요약 영역**과 하단의 **프로세스 영역**으로 나뉩니다.

#### 1. 요약 영역 (Summary Area)
화면 상단 5줄에 해당하는 영역으로, 시스템 전체의 리소스 상태를 보여줍니다.
- **Tasks:** 전체 프로세스 개수 및 상태별(Running, Sleeping, Stopped, Zombie) 개수
- **%Cpu(s):** CPU 사용 현황
  - `us` (User): 사용자 프로세스(Un-niced) 사용율
  - `sy` (System): 커널(시스템) 프로세스 사용율
  - `ni` (Nice): 우선순위가 조정된(Nice) 사용자 프로세스 사용율
  - `id` (Idle): 유휴 상태 (사용되지 않는 비율)
  - `wa` (IO-wait): 디스크 I/O 작업 완료를 대기하는 시간 (높으면 디스크 병목 의심)
  - `hi` (Hardware IRQ): 하드웨어 인터럽트 처리 시간
  - `si` (Software IRQ): 소프트웨어 인터럽트 처리 시간
  - `st` (Steal Time): 하이퍼바이저에 의해 다른 가상 머신에게 CPU 자원을 빼앗긴 시간
- **MiB Mem / Swap:** 물리 메모리(RAM)와 스왑 메모리의 전체 용량, 사용량, 여유량 표시

#### 2. 프로세스 영역 (Process Area)
각 프로세스의 상세 정보를 테이블 형태로 보여줍니다.
| 필드명 | 설명 | 비고 |
| :--- | :--- | :--- |
| **PID** | 프로세스 ID (Process ID) | 각 프로세스의 고유 식별 번호 |
| **USER** | 사용자 (User) | 해당 프로세스를 실행한 사용자 이름 |
| **PR / NI** | 우선순위 (Priority / Nice) | 실행 우선순위 값 (낮을수록 높음) |
| **VIRT** | 가상 메모리 (Virtual Image) | 프로세스가 사용하는 가상 메모리 총량 |
| **RES** | 상주 메모리 (Resident Size) | 실제로 사용 중인 물리 메모리 양 |
| **SHR** | 공유 메모리 (Shared Mem) | 다른 프로세스와 공유하는 메모리 양 |
| **S** | 상태 (Status) | `R`(실행중), `S`(슬립), `Z`(좀비) 등 |
| **%CPU** | CPU 사용률 | 프로세스가 차지하는 CPU 비중 |
| **%MEM** | 메모리 사용률 | 프로세스가 차지하는 메모리 비중 |
| **TIME+** | 실행 시간 | 프로세스가 시작된 후 사용한 총 CPU 시간 |
| **COMMAND** | 명령어 | 프로세스를 실행한 명령어 이름 |

### 🎮 핵심 단축키 (Interactive Keys)
`top` 실행 중 사용할 수 있는 유용한 키 모음입니다. (대소문자 구별 주의)

#### 1. 정렬 및 필터링 (Sorting)
| 키 | 동작 |
| :---: | :--- |
| `P` | **CPU** 사용량 순서로 정렬 (기본값) |
| `M` | **메모리** 사용량 순서로 정렬 |
| `T` | 프로세스 **실행 시간** 순서로 정렬 |
| `R` | 정렬 순서 뒤집기 (오름차순/내림차순 토글) |

#### 2. 프로세스 제어 (Control)
| 키 | 동작 |
| :---: | :--- |
| `k` | 프로세스 종료 (Signal 전송, PID 입력 필요) |
| `r` | 프로세스 우선순위(Nice 값) 변경 |

#### 3. 화면 표시 설정 (Display)
| 키 | 동작 |
| :---: | :--- |
| `1` | CPU 코어별 상세 사용량 보기/접기 |
| `c` | 명령어 전체 경로(Full Path) 보기 토글 |
| `z` | 컬러 모드 켜기/끄기 (강조 표시) |
| `x` | 현재 정렬 기준이 되는 컬럼 하이라이트 |

#### 4. 기타 설정
| 키 | 동작 |
| :---: | :--- |
| `d` | 화면 갱신 주기 변경 (기본 3초) |
| `h` | 도움말 보기 |
| `q` | top 종료 |

---

## 2. 📸 ps - 프로세스 상태 확인 (Snapshot)

`ps` (Process Status)
- 명령어가 실행된 **바로 그 순간**의 프로세스 상태를 스냅샷처럼 찍어서 보여줌.

### 🔹 필수 옵션 (Flags)
아래는 가장 자주 사용되는 옵션들을 정리한 것임.

| 옵션 | 설명 | 활용 팁 |
| :---: | :--- | :--- |
| `-e` | **E**very | 시스템의 모든 프로세스를 출력 |
| `-f` | **F**ull | UID, PPID 등 상세 정보를 포함하여 출력 |
| `-a` | **A**ll | 다른 사용자의 프로세스도 함께 표시 |
| `-u` | **U**ser | 프로세스 소유자 이름, 시작 시간 등 표시 |
| `-x` | No TTY | 터미널에 종속되지 않은 데몬 프로세스 표시 |

### 🚀 추천 명령어 조합
시스템 관리자들이 가장 애용한다고 알려진 두 가지 명령어 조합

**1. System V 스타일 (가장 보편적)**
```bash
$ ps -ef
```

**2. BSD 스타일 (상세 정보 확인용)**
```bash
$ ps -aux
```
```text
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 168456 12544 ?        Ss   Nov20   0:05 /sbin/init
user      1234  0.5  2.1 543210 98765 pts/0    Sl+  10:00   0:12 ./my_program
```

---

<a id="jobs-section"></a>
## 3. ⏯️ jobs - 백그라운드 작업 관리

`jobs`
- 현재 쉘 세션에서 실행 중인 **백그라운드(Background)** 작업과 **중지된(Stopped)** 작업의 목록을 관리.

### 🔹 상태(State) 구분
- `Running`: 백그라운드에서 현재 계속 실행 중인 상태
- `Stopped`: 사용자가 `Ctrl + Z` 등을 통해 일시 정지시킨 상태
- `Done`: 작업이 정상적으로 완료된 상태

### 💻 사용 예시
```bash
$ jobs
[1]-  Running                 sleep 100 &
[2]+  Stopped                 vim my_code.c
```
- **`[N]`**: 작업 번호 (Job ID) -> `fg`나 `bg` 명령에서 사용
- **`+`**: 현재 작업 (Current Job)
- **`-`**: 이전 작업 (Previous Job)

> 💡 **Tip: 작업 전환하기**
> - `fg %1`: 1번 작업을 **포그라운드**(화면 앞)로 가져옵니다.
> - `bg %2`: 2번 작업을 **백그라운드**(뒤)에서 다시 실행합니다.

---

## 4. 🔫 kill - 프로세스 종료 및 시그널 전송

`kill`
- 프로세스에 특정 **시그널(Signal)**을 전송하는 명령어.
- 이름은 kill이지만, 종료 외에도 다양한 신호를 보낼 수 있음.

### 🔹 주요 시그널 (Signals)

1. **SIGHUP (1)**: **재시작**. 설정 파일이 변경되었을 때 데몬을 재시작용으로 사용.
2. **SIGINT (2)**: **인터럽트**. 키보드 `Ctrl + C` 입력과 동일.
3. **SIGKILL (9)**: **강제 종료**. 프로세스가 무시할 수 없는 강력한 종료 신호.
4. **SIGTERM (15)**: **정상 종료**. (기본값) 프로세스가 정리할 시간을 주고 종료.

### 💻 문법 및 주의사항
```bash
kill [옵션/시그널] [PID]
```

> **🚨 주의 (Warning)**
>
> `kill -9 [PID]`는 프로세스를 즉시 강제 종료시킵니다. 데이터 저장 등 마무리 작업을 할 수 없으므로, **먼저 `-15`(기본값)로 종료를 시도**하고 반응이 없을 때 최후의 수단으로 `-9`를 사용하세요.

---
