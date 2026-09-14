# Poem · Cairn

[공개 랜딩](https://team-poem.github.io/)을 배포하는 저장소입니다.
랜딩 코드는 [team-poem/cairn-landing](https://github.com/team-poem/cairn-landing)의 `main`에서 수정합니다.

## 자동 배포

`.github/workflows/pages.yml`은 5분 주기로 소스 `main`의 커밋과 공개 사이트의 `source-sha.txt`를 비교합니다. 변경이 있을 때만 해당 커밋을 고정해 체크아웃하고, Node.js 24에서 타입·린트·데모 테스트·정적 빌드를 통과한 `dist/client`를 GitHub Pages에 게시합니다. GitHub 스케줄 상황에 따라 실행은 지연될 수 있습니다.

수동 실행 또는 이 저장소 main 변경은 커밋이 같아도 다시 배포합니다.

```sh
gh workflow run pages.yml --repo team-poem/team-poem.github.io --ref main
```

Settings → Pages의 Source는 **GitHub Actions**입니다. 추가 secret·개인 토큰·deploy key는 사용하지 않으며, 배포 작업만 `pages: write`와 `id-token: write` 권한을 가집니다. 빌드 실패 시 기존 사이트를 유지합니다.

Actions의 **Cairn Pages**에서 실패 원인과 배포 소스 커밋을 확인할 수 있습니다. 이 저장소에는 소스 사본이나 빌드 결과를 커밋하지 않습니다.
