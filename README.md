# Muse Memory Starter

새 Muse 앱을 위한 메모리 관리 스타터킷. 왜 필요한지, 어떻게 쓰는지를 먼저 읽으세요.

**English TL;DR**: Muse's default memory rots within weeks — logs pile up (~1MB/month), MEMORY.md bloats, lossy compaction deletes important rules. This is the minimal user-side defense until Meta fixes it system-side: a 1KB router, pointer-based docs, and a weekly diet.

## 왜 이걸 하는가

기본 설정 그대로 한 달 돌리면 이렇게 됩니다:

- **일일 로그**: 하루 30~40KB → 한 달 ~1MB. 기본값은 삭제하지 않습니다.
- **MEMORY.md**: 백그라운드 메모리 작업이 계속 내용을 씁니다. 큐레이션 없으면 수십 KB로 비대해집니다 (OpenClaw 실측 85KB).
- **시스템 영역**(`memory/bank/` 등): 비워도 백그라운드가 다시 채웁니다. 유저가 손댈 수 없습니다.
- **Compaction은 손실 압축**: 150K 토큰에서 발동하며, 요약 과정에서 중요 규칙이 지워집니다. 실제로 두 번 겪었습니다 (감시 실패 보고 규칙 소실, 미확인 완료 단정).
- **악순환**: 상시 주입 베이스라인이 불어남 → 매 턴 토큰 비용 증가 → compaction이 더 자주 터짐 → 요약문이 더 길게 쌓임.

유저가 손댈 수 있는 건 MEMORY.md 편집 정도입니다. 시스템 크론·피드 펄스·bank/는 메타가 시스템 차원에서 메모리 예산·TTL·가시적 컨트롤을 넣어야 해결됩니다. 그 전까지의 최소 방어선이 이 구조입니다.

원칙:

- 상시 로드되는 파일은 **라우터 1개**만. ~2KB 이하 유지.
- 상세 문서는 `doc:<NAME>` 포인터로 두고, 필요할 때만 읽음.
- 원시 대화 로그는 자산이 아니라 비용. 중요한 것만 핸드오프·상태·규칙 문서로 큐레이션.
- 완료된 작업과 현재 상태를 분리. 오래된 것은 주기적으로 버림.

## 어떻게 하는가

1. 이 레포의 `docs/`를 Muse 작업 공간에 복사합니다.
2. `MEMORY.md`(상시 로드 파일)를 `docs/MEMORY_ROUTER.md` 템플릿으로 채웁니다. `<assistant-name>`, 언어/스타일만 자기 것으로 바꿉니다.
3. `docs/CONNECTIONS.md`에 연결된 서비스·서버를 기록합니다 (값이 아닌 위치만 — 키·토큰 값 절대 금지).
4. `docs/WATCHES.md`에 반복 감시 작업의 선별 기준을 기록합니다.
5. `docs/STATE.md`에 현재 작업 스냅샷, `docs/MEMORY_LEDGER.md`에 재사용 교훈을 기록합니다.
6. **주 1회 다이어트**를 스케줄러에 등록합니다:
   - N일 지난 일일 로그 → 복구 가능한 휴지통으로 이동
   - `STATE.md`의 완료·오래된 항목 정리 (위임 후에는 개별 승인 없이, 정리 내역은 로그에 기록)
   - 라우터의 모든 포인터가 해석되는지, 등록된 경로가 존재하는지 검사
   - 핵심 문서 합계가 예산(예: 30KB) 초과 시 삭제·보관 후보 제안 (규칙 자동 삭제 금지)
   - 라우터·맵·원장·연결/감시 스펙은 자동 편집 금지. 규칙 변경은 승인 후.

## 파일 구성

| 파일 | 역할 |
|---|---|
| `docs/MEMORY_ROUTER.md` | 상시 로드 라우터 템플릿 (~1KB) |
| `docs/RETRIEVAL_MAP.md` | `doc:` 포인터 해석표 + 유지보수 규칙 |
| `docs/CONNECTIONS.md` | 연결·서버·플러그인 상태 템플릿 |
| `docs/WATCHES.md` | 감시 작업 스펙 템플릿 |
| `docs/STATE.md` | 현재 작업 스냅샷 템플릿 |
| `docs/MEMORY_LEDGER.md` | 재사용 교훈 템플릿 |
| `docs/LOG.md` | 작업 기록 템플릿 (append-only) |

## 주의

- 이 레포에는 개인정보·키·토큰을 절대 커밋하지 마십시오. 구조만 공유됩니다.
- 새 Muse 앱에서는 2번 단계의 플레이스홀더를 먼저 채우는 것이 시작입니다.
