# Filee 기여 인수인계

SeoJCalive가 KnifeLemon/Filee에 기여하면서 정한 작업 규칙, 현재 상태, 남은 일, 리뷰에서 배운 점을 모았다.
다른 PC나 다른 에이전트(omo 등)가 이어서 작업할 때 이 문서부터 읽는다.

> 이 브랜치(`notes/handoff`)에는 이 문서만 있다. 코드와 섞이지 않게 일부러 빈 브랜치로 만들었으니
> **여기서 기능 작업을 하거나 PR을 열지 않는다.**

---

## 1. 저장소와 브랜치

| 항목 | 값 |
|---|---|
| 원본(upstream) | https://github.com/KnifeLemon/Filee (메인테이너 KnifeLemon) |
| 내 fork(origin) | https://github.com/SeoJCalive/Filee |
| 로컬 경로 | Windows `C:\Users\BILAB\Documents\Filee`, Mac `~/Filee` |
| 스택 | C# / .NET 10, Avalonia 12, Magick.NET, CommunityToolkit.Mvvm, xUnit v3 (Avalonia headless 테스트) |

### 새 작업 시작법 (중요)
fork의 `main`은 오래됐다(7d02987에 멈춰 있음). **새 작업은 항상 upstream 최신 main에서 새 브랜치로 시작한다.**

```sh
git remote add upstream https://github.com/KnifeLemon/Filee   # 처음 한 번
git fetch upstream main
git checkout -b feature/<이름> upstream/main
```

### fork의 브랜치 상태 (2026-10-17 기준)
| 브랜치 | 상태 |
|---|---|
| `feature/donut-groups` | PR #88로 머지됨. 다시 쓰지 않는다 |
| `feature/rotate-flip` | PR #89로 머지됨. 다시 쓰지 않는다 |
| `feature/watch-keep-subfolders` | PR #73으로 머지됨 |
| `claude/brave-brahmagupta-mnmj17` | PR #70(배경 제거)으로 머지됨 |
| `proto/donut-groups` | 그룹 기능 초기 프로토타입. 머지 안 됨, 참고용 |
| `main` | 오래됨. 기준으로 쓰지 않는다 |

머지된 PR의 브랜치에 커밋을 더 쌓지 않는다. 후속 작업은 새 브랜치를 만들고 새 PR로 올린다.

---

## 2. 작업 규칙 (꼭 지킬 것)

1. **기본 도넛은 건드리지 않는다.** 기본 8칸 도넛 프로필에 새 프리셋이나 그룹을 넣지 않는다.
   새 기능 프리셋은 라이브러리에만 추가하고, 사용자가 직접 도넛에 끌어다 놓게 한다.
2. **PR은 사용자가 직접 연다.** 에이전트는 브랜치에 push까지만 하고, PR 본문 초안과 compare 링크를 준다.
   링크 형식: `https://github.com/KnifeLemon/Filee/compare/main...SeoJCalive:Filee:<branch>?expand=1`
3. **커밋 작성자는 SeoJCalive로 유지한다.**
   `user.name = SeoJCalive`, `user.email = 180237232+SeoJCalive@users.noreply.github.com`(저장소 로컬 설정).
   그래야 GitHub에서 기여자로 인정된다. 에이전트를 작성자로 바꾸지 않는다(`Co-Authored-By` 줄은 괜찮다).
4. **답변은 한국어로 한다.** 코드, 커밋 메시지, PR 본문은 저장소 관례에 따라 영어로 쓴다.
5. **테스트가 필수다.** CONTRIBUTING에 "make your change with tests"라고 되어 있고, CI가 `dotnet test`를 돌린다.
   push 전에 반드시 다음을 통과시킨다.
   ```sh
   dotnet format --verify-no-changes
   dotnet test
   ```
   FFmpeg와 LibreOffice가 필요한 테스트는 그 엔진이 없는 PC에서 원래 실패할 수 있다. PR에는 "기존 실패"라고 적는다.
6. **코드 스타일은 주변 코드에 맞춘다.** 커밋 메시지는 upstream처럼 `영역: 사용자 입장에서 무엇이 달라졌나` 형식으로 쓰고,
   본문에는 이유를 쓴다.
7. **PR 본문은 그 PR 내용만 깔끔하게 쓴다.** 무엇을, 왜, 어떻게 테스트했는지만 적는다.

### 수동 테스트 팁
- Filee는 **한 번에 하나만 실행되는 앱**이다. 새 빌드를 테스트하기 전에 기존 프로세스를 끈다.
  - Mac: `pkill -f Filee`
  - Windows: 트레이에서 종료하거나 작업 관리자에서 끝낸다.
- Mac zsh에서는 붙여넣은 명령의 `#` 주석이 오류를 낸다. 명령은 주석 없이 준다.
- 실행: `dotnet run --project src/Filee.App`
- 제스처 로그를 보려면 `FILEE_TRACE_GESTURES=1`을 켠다.

---

## 3. 지금까지 기여한 것 (모두 머지됨)

| PR | 내용 |
|---|---|
| #70 | 간단한 이미지 배경 제거("Remove background"). 기본 도넛에는 넣지 않음 |
| #73 | 감시 폴더가 출력 폴더에서도 하위 폴더 구조를 유지 |
| #88 | 도넛 **그룹(묶음) 칸**. 한 칸에 프리셋 여러 개를 담고, 열면 바깥 링으로 펼쳐짐 (#74에서 받은 리뷰 5개 반영) |
| #89 | 이미지 **회전·뒤집기** 옵션, 내장 "회전"/"뒤집기" 그룹 (기본 도넛에는 없음) |

### 그룹 기능의 핵심 구조 (#88)
- `Preset.Kind`는 `Convert` 또는 `Group`이고, 그룹은 `SubPresetIds`를 가진다.
  `TargetFormat = "group"`(`Preset.GroupTarget`)은 어떤 형식도 쓰지 않는 값이라, 구버전 Filee가 그룹을 실행하지 않는다.
- 그룹은 프리셋 목록, 감시 폴더, CLI `--preset`에서 빠진다. 관련 코드는 `UserDataStore.FindConversion`.
- 도넛 코드는 partial class로 나뉜다. `DonutMenu.cs`(본체), `DonutMenu.Groups.cs`(그룹 전부), `DonutMenu.Progress.cs`.
- 링을 여는 조건:
  - 그룹 칸에 160ms 머무르거나, 바깥쪽으로 10px 움직이거나, 클릭·탭하면 열린다.
  - 키보드: 방향키, Tab, Enter, Esc. 한 그룹에 최대 6개.
- **투명 픽셀 문제:** Windows와 macOS는 완전히 투명한 픽셀에서 포인터와 드래그를 창 밖으로 처리한다.
  그래서 링이 열려 있는 동안 알파 1/255짜리 배경(HitBackdrop)을 그린다.
  "창을 떠났다"는 신호가 와도 도넛 안쪽이나 링 배경 가장자리에서 12px 안쪽이면 무시한다.
  메인테이너가 이 범위를 좁혔다(31baed2).

### 회전·뒤집기의 핵심 구조 (#89)
- `ImageOptions.Rotation`과 `ImageOptions.Flip`. 사진의 EXIF 방향을 먼저 적용한 뒤 회전한다.
- 엔진은 `IImageTransformer.Transforms(from, to)`로 회전을 할 수 있는지 밝힌다.
  - 변환 경로에서 **회전하는 단계 하나만** 회전 값을 받는다.
  - 회전할 수 있는 단계가 없으면 `error.cannot_turn`으로 실패하고, 도넛 칸이 비활성화된다.
- 설정 마이그레이션은 스키마 **v17**이다. main이 v11~v16을 먼저 써서 메인테이너가 옮겼다.
- JPEG를 JPEG로 회전할 때는 원본의 추정 품질이 프리셋 품질보다 낮으면 원본 품질로 저장한다.
- 저장된 enum 값은 이름이 정확히 일치할 때만 읽는다. 모르는 값은 기본값으로 읽는다(`LenientEnumJsonConverter`).

---

## 4. 리뷰에서 배운 점 (#88, #89 머지 때 메인테이너가 직접 고친 것)

에이전트가 리뷰를 두 번 했는데도 메인테이너가 아래를 찾아 고쳤다. **다음 PR 전에 이 목록으로 스스로 점검한다.**

| 커밋 | 놓친 것 | 원인 |
|---|---|---|
| f4c7dd4 | 링이 열렸을 때 옆 칸 바깥쪽 절반을 그룹으로 판정 → 거기에 클릭·드롭하면 무시되거나 취소됨 | 링을 안 닫히게 하는 규칙이 클릭·드롭 판정에도 영향을 주는지 확인하지 않음 |
| 6560713 | 구버전 Filee가 저장하면 `kind`가 사라져 그룹이 "group으로 변환하는 프리셋"이 됨 | PR B에는 같은 복구를 넣고 PR A에는 빠뜨림 |
| 31baed2 | 실제로 밖으로 나가도 링이 안 닫힘, 도넛을 닫아도 타이머가 살아 있음 | Mac 버그를 급하게 고치면서 범위를 너무 넓게 잡음 |
| 52c55ed | STL→HEIC가 두 번 회전됨 | "회전 단계 이후만 빼는" 구조였음. "회전 단계만 넣는" 구조였어야 함. main에 새로 들어온 3D 엔진과 맞물림 |
| 8c9cda1 | PDF 대상이면 회전이 적용되는데 화면에 안 보임, 동영상·문서 대상이면 모두 실패하는데 끌 방법이 없음 | 대상 형식별로 편집기 화면을 점검하지 않음 |
| c024fb7 | 품질 75 사진을 95로 다시 저장해서 용량만 커짐 | 화질만 보고 용량은 측정하지 않음 |
| acd7f05 | `Enum.TryParse`가 `"Left90, Rotate180"`이나 `"2"`를 받아들임 | 직접 만든 변환기의 경계값을 시험하지 않음 |
| 4938450 | 중국어 문구에 전각 콜론 | 주변 번역 문체를 확인하지 않음 |

### PR 전 체크리스트
- [ ] 리뷰는 **새 세션에서 diff만 주고** 시킨다. 작성한 대화 안에서 셀프 리뷰를 하지 않는다.
- [ ] 버그를 고칠 때마다 "이 수정이 바꿀 수 있는 다른 동작"을 적고 각각 테스트한다.
  예: 호버를 고치면 클릭, 드롭, 닫기, 창 숨기기도 확인한다.
- [ ] 옵션을 추가하면 **대상 형식별 표**를 만든다(이미지, PDF, 동영상, 문서, 압축 파일).
  편집기에 보이는지, 실제로 적용되는지, 끌 수 있는지 확인한다.
- [ ] 새 저장 필드는 **구버전 왕복**(구버전이 모르는 필드를 지우고 저장)과 **모르는 값**(신버전이나 손으로 고친 값)을 테스트한다.
- [ ] 화질, 크기, 시간 같은 수치는 하나만 보지 말고 트레이드오프를 실제로 측정한다.
- [ ] push 직전에 upstream 최신 main을 합치고, 새로 들어온 코드와 내 코드가 어떻게 맞물리는지 다시 본다.
- [ ] 번역 문자열은 같은 파일의 주변 문체(문장부호, 존댓말)에 맞춘다.
- [ ] 기존 테스트를 지우거나 약하게 바꾸지 않는다. 바꿔야 한다면 PR에 이유를 쓴다.

---

## 5. 남은 일과 후보

### #63 동영상을 프레임별 이미지로 저장 — **보류 (다른 사람이 진행 중)**
- 이슈: https://github.com/KnifeLemon/Filee/issues/63 (요청자 pleguen. 메인테이너가 pleguen에게 PR을 요청함)
- pleguen의 fork에 Draft PR이 있다: https://github.com/pleguen/Filee/pull/1 (브랜치 `feature/video-frame-splitting`)
- 메인테이너 조건:
  1. 기본값이 꺼진 옵션
  2. 기존 한 장 추출 동작 유지 + 새 테스트
  3. 겹치지 않는 `<이름>_frames`, `<이름>_frames (2)` 폴더
  4. (선택) N프레임마다
  5. 프리셋 품질 사용
- 그 Draft를 점검한 결과:
  - **치명적:** 영상→JPG 직행 경로(비용 11)를 추가해서, 옵션이 꺼져 있어도 모든 영상→JPG가 바뀌고 이미지 옵션이 빠진다.
    그걸 맞추려고 기존 테스트를 지우고 진행률 테스트를 약하게 바꿨다.
  - 그 밖에:
    - 폴더 이름을 정하려고 받아 둔 파일 경로를 실제로 쓰지 않는다(`MyVideo (2)_frames`처럼 이상한 이름이 나올 수 있음).
    - 폴더 이름 중복 검사를 출력 경로 잠금 밖에서 한다.
    - 없는 형식 ID `"jpeg"`를 쓴다.
    - `FramesPerSecond`는 UI가 없다.
    - 프레임 수에 상한이 없다.
    - 현재 main과 4개 파일이 충돌한다.
- **방침:** 남의 작업이므로 손대지 않는다. pleguen이 오래 소식이 없으면 이슈에 "도와도 될지" 먼저 묻는다.

### #86 Linux에서 단축키가 안 됨 — **정보 부족**
- 이슈: https://github.com/KnifeLemon/Filee/issues/86 (Arch/CachyOS, 본문 한 줄)
- 아마 버그가 아니라 알려진 제한이다.
  - Linux에서 키보드 단축키와 길게 누르기는 기본 비활성이다(`LinuxPlatformServices.SupportsSelectionShortcut => false`).
    예외는 Thunar 하나로, 첫 번째 단축키가 Thunar 자체 단축키로 등록된다.
  - 드래그 제스처는 X11에서만 된다. Wayland에서는 SharpHook이 동작하지 않는다.
  - CachyOS 기본값인 KDE Plasma(Wayland) + Dolphin이면 둘 다 안 된다.
- 다음 할 일: 작성자에게 데스크톱 환경, `echo $XDG_SESSION_TYPE` 결과, 파일 관리자, 켠 단축키 종류를 묻는다.
- 기여 후보:
  - Nautilus의 `~/.config/nautilus/scripts-accels`로 단축키 연동(`LinuxDesktopIntegration.cs`에서 Thunar 방식을 확장).
    최신 Nautilus에서 되는지 확인이 필요하다.
  - Dolphin은 우클릭 메뉴 항목에 단축키를 붙이는 방법부터 조사해야 한다.
- 테스트 환경: Windows의 WSL2(WSLg)로 어느 정도 가능하다. 최종 확인은 실제 Linux가 좋다.

### 보안 스캐너 오탐 (gitleaks `generic-api-key`) — **선택 사항**
- 걸린 줄: `src/Filee.App/Services/Triggers/GestureDetector.cs`의 `token = Environment.TickCount64 ^ (x * 7919L) ^ y;`
  과거 커밋 6개에도 같은 줄이 있다.
- **비밀값이 아니다.** 길게 누르기 타이머를 해당 누름과 맞추는 내부 번호일 뿐이다.
  변수 이름 `token` 옆에 긴 식이 와서 스캐너가 잡았다.
- 원하면 이름만 바꾸는 작은 PR을 올린다. `token` → `pressId` 등으로, `GestureDetector.cs`와 `TriggerService.cs` 두 파일 약 10줄이다.
  PR 설명에 "보안 문제가 아니라 스캐너 오탐을 피하려는 이름 변경"이라고 솔직히 쓴다.
- 7-Zip의 `NoPassword = "-pfilee-no-password"`는 비밀번호 입력 대기를 막는 고정 값이고, 스캐너도 잡지 않았다.

---

## 6. 참고 링크
- 기여 가이드: upstream `CONTRIBUTING.md` (설치, 엔진, Linux/macOS 개발, 파일 위치)
- 구조 문서: `docs/ARCHITECTURE.md`, `docs/ENGINES.md`
- 변경 기록: `CHANGELOG.md`
- 머지된 PR: https://github.com/KnifeLemon/Filee/pull/88 , https://github.com/KnifeLemon/Filee/pull/89
