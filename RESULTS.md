# Results

## 1. 실험 요약
- 저장소: exp-browser-agent-local
- 커밋 해시: 5238287
- 실험 일시: 2026-05-20T15:41:00.825Z -> 2026-05-20T15:41:04.660Z
- 담당자: ai-webgpu-lab
- 실험 유형: `agent`
- 상태: `success`

## 2. 질문
- 브라우저 agent 실험으로 넘기기 전에 task success, step latency, intervention 보고 경로를 먼저 고정할 수 있는가
- workflow id, tool catalog, local-only page metadata, fallback metadata가 agent 결과 문서에 같이 남는가
- 실제 browser control runtime 교체 전 deterministic local task-deck harness로 반복 검증이 가능한가

## 3. 실행 환경
### 브라우저
- 이름: Chrome
- 버전: 147.0.7727.15

### 운영체제
- OS: Linux
- 버전: unknown

### 디바이스
- 장치명: Linux x86_64
- device class: `desktop-high`
- CPU: 16 threads
- 메모리: 32 GB
- 전원 상태: `unknown`

### GPU / 실행 모드
- adapter: navigator.gpu available
- backend: `webgpu`
- fallback triggered: `false`
- worker mode: `worker`
- cache state: `warm`
- required features: ["shader-f16"]
- limits snapshot: {"maxStorageBuffersPerShaderStage":8,"maxBindGroups":4}

## 4. 워크로드 정의
- 시나리오 이름: Browser Agent Local Readiness
- 입력 프로필: 3-tasks-5-tools
- 데이터 크기: workflow=browser-agent-local-v1; page=ops-dashboard; tasks=3; tools=open_panel|filter_cards|copy_field|write_note|toggle_flag; interventions=0; backend=webgpu; fallback=false; automation=playwright-chromium, workflow=browser-agent-local-v1; page=ops-dashboard; tasks=3; tools=open_panel|filter_cards|copy_field|write_note|toggle_flag; interventions=0; backend=webgpu; fallback=false; realAdapter=fallback(adapter.loadModel is not a function); automation=playwright-chromium
- dataset: browser-agent-fixture-v1
- model_id 또는 renderer: deterministic-browser-agent-v1
- 양자화/정밀도: -
- resolution: -
- context_tokens: -
- output_tokens: -

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 486.2 ~ 1229.1 ms
- init_ms: 34 ms
- success_rate: 1
- peak_memory_note: 32 GB reported by browser
- error_type: -

### Browser Agent
- task_success_rate: 1
- avg_step_latency_ms: 34 ms
- tool_call_success_rate: 1
- user_intervention_count: 0
- worker modes: worker
- backends: webgpu
- fallback states: false

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | Browser Agent Local Readiness | webgpu | warm | 1 | 34 | tool_success=1, interventions=0 |
| 2 | Browser Agent Local Readiness | webgpu | warm | 1 | 34 | tool_success=1, interventions=0 |

## 7. 관찰
- browser agent local readiness baseline은 backend=webgpu, fallback_triggered=false, worker_mode=worker로 기록됐다.
- agent summary는 task_success_rate=1, avg_step_latency_ms=34, tool_call_success_rate=1였다.
- agent metadata는 workflow=browser-agent-local-v1; page=ops-dashboard; tasks=3; tools=open_panel|filter_cards|copy_field|write_note|toggle_flag; interventions=0; backend=webgpu; fallback=false; automation=playwright-chromium로 남았다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. Real Adapter vs Deterministic
- adapter: real=browser-agent-xenova-phi-3-mini-4k-instruct-300, deterministic=deterministic-mock
- adapter_run: real=connected, deterministic=deterministic
- success_rate: real=1, deterministic=1

## 9. 결론
- browser agent readiness harness가 task success, step latency, tool success, intervention count를 같은 문서에 남기게 됐다.
- 다음 단계는 deterministic local task deck을 실제 browser controller, planner, DOM policy, tool routing runtime으로 교체하되 task_success_rate/avg_step_latency_ms/tool_call_success_rate/user_intervention_count metric 구조를 유지하는 것이다.
- 이후 `bench-agent-step-latency`와 `app-voice-agent-lab`의 공통 browser-agent fixture 입력으로 재사용할 수 있다.

## 10. 첨부
- 스크린샷: ./reports/screenshots/01-browser-agent-local-readiness.png, ./reports/screenshots/10-browser-agent-local-real-browser-agent.png
- 로그 파일: ./reports/logs/01-browser-agent-local-readiness.log, ./reports/logs/10-browser-agent-local-real-browser-agent.log
- raw json: ./reports/raw/01-browser-agent-local-readiness.json, ./reports/raw/10-browser-agent-local-real-browser-agent.json
- 배포 URL: https://ai-webgpu-lab.github.io/exp-browser-agent-local/
- 관련 이슈/PR: -
