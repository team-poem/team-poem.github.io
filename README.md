# Poem · Cairn

[공개 랜딩](https://team-poem.github.io/cairn-engine/)을 배포하는 저장소입니다.
기존 루트 주소는 쿼리와 해시를 보존해 `/cairn-engine/`로 이동합니다.
랜딩 코드는 [team-poem/cairn-landing](https://github.com/team-poem/cairn-landing)의 `main`에서 수정합니다.

## 자동 배포 전환 준비

직접 호출용 `source_sha` 입력을 지원합니다. `cairn-landing`의 검증 완료 이벤트가 전용 GitHub App으로 이 workflow를 호출하도록 연결하며, App 설치와 소스 CI가 머지되기 전까지 직접 자동 호출은 활성화되지 않습니다. 요청 SHA가 현재 main과 다르면 오래된 배포를 생략합니다.

`.github/workflows/pages.yml`의 5분 스케줄은 보조 확인 용도입니다. 소스 `main`의 커밋과 공개 사이트의 `/cairn-engine/source-sha.txt`를 비교합니다. 변경이 있을 때만 해당 커밋을 고정해 체크아웃하고, Node.js 24에서 타입·린트·데모 테스트·`NEXT_PUBLIC_BASE_PATH=/cairn-engine` 정적 빌드와 경로 검증을 통과한 `dist/pages`를 GitHub Pages 루트에 게시합니다. GitHub 스케줄 상황에 따라 실행은 지연될 수 있습니다.

수동 실행 또는 이 저장소 main 변경은 커밋이 같아도 다시 배포합니다.

```sh
gh workflow run pages.yml --repo team-poem/team-poem.github.io --ref main
```

Settings → Pages의 Source는 **GitHub Actions**입니다. 이 배포 저장소 자체에는 추가 secret이 필요하지 않으며, 배포 작업만 `pages: write`와 `id-token: write` 권한을 가집니다. 빌드 실패 시 기존 사이트를 유지합니다.

Actions의 **Cairn Pages**에서 실패 원인과 배포 소스 커밋을 확인할 수 있습니다. 이 저장소에는 소스 사본이나 빌드 결과를 커밋하지 않습니다.

직접 호출에는 배포 저장소 하나의 Actions 읽기·쓰기만 허용한 GitHub App을 사용합니다. 앱 개인키는 소스 저장소의 `PAGES_APP_PRIVATE_KEY`, Client ID는 `PAGES_APP_CLIENT_ID` 변수로 관리합니다. 소스·설정 쓰기 권한은 부여하지 않습니다.

`cairn-landing`의 경로 변경 PR을 먼저 머지해야 합니다. 배포 시 소스의 `scripts/prepare-pages.py`가 랜딩과 에셋을 `cairn-engine/`에 모으고 루트 이동 페이지를 생성합니다. 경로 검사 실패 시 게시하지 않습니다.
