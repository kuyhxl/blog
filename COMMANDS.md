# 자주 쓰는 명령어 모음

> kuyhxl's blog (Quartz) 운영용 명령어 정리. 이 파일은 `content/` 밖이라 블로그에 게시되지 않습니다.

작업 디렉토리: `/Users/chanhyeokhwang/quartz`

---

## 1. 로컬 미리보기 서버

작성/수정 내용을 배포 전에 브라우저에서 확인.

```bash
npx quartz build --serve
```

- 기본 주소: http://localhost:8080
- 파일을 고치면 자동으로 다시 빌드되어 새로고침됨
- 포트 바꾸기: `npx quartz build --serve --port 3000`
- 중단: 터미널에서 `Ctrl + C`

---

## 2. 빌드만 (배포 안 함)

`public/` 폴더에 정적 사이트 생성. 결과물만 확인하고 싶을 때.

```bash
npx quartz build
```

---

## 3. Obsidian 볼트 → content 동기화

iCloud Obsidian 볼트의 폴더를 `content/`로 복사. **배포(sync) 전에 실행.**

```bash
rsync -av --delete \
  ~/Library/Mobile\ Documents/iCloud~md~obsidian/Documents/<볼트>/<폴더>/ \
  /Users/chanhyeokhwang/quartz/content/<폴더>/
```

- `<볼트>`: `Files` | `University` | `Web` 중 하나
- `<폴더>`: 볼트 안의 가져올 폴더 이름
- `--delete`: 볼트에서 지운 노트는 content에서도 삭제(완전 미러링). 원치 않으면 제거.
- 주의: iCloud "저장 공간 최적화"로 노트가 클라우드에만 있으면 빈 파일이 복사될 수 있음 → Finder에서 "지금 다운로드" 후 실행.

---

## 4. 배포 (커밋 + 푸시)

`content/`의 변경분을 커밋하고 GitHub로 푸시 → 자동 배포.

```bash
npx quartz sync --no-pull
```

- 내부 동작: `git add .` → `git commit` → `git push -uf origin v5`
- `--no-pull`: Quartz 템플릿 소스에서 pull 건너뜀 (커스텀 컴포넌트 충돌 방지). **권장.**

---

## 5. 한 방에 동기화 + 배포 (선택)

`~/.zshrc`에 추가해두면 `blog-sync` 한 번으로 3 + 4 단계 실행.

```bash
blog-sync() {
  rsync -av --delete \
    ~/Library/Mobile\ Documents/iCloud~md~obsidian/Documents/<볼트>/<폴더>/ \
    /Users/chanhyeokhwang/quartz/content/<폴더>/
  cd /Users/chanhyeokhwang/quartz && npx quartz sync --no-pull
}
```

---

## 6. 플러그인 설치/갱신

`quartz.config.yaml`의 커뮤니티 플러그인을 받아옴 (`.quartz/plugins/`). 새 환경 세팅이나 플러그인 추가/변경 후 실행. (배포 CI에서는 자동 실행됨)

```bash
npx quartz plugin install
```

---

## 일상 워크플로우 요약

```text
1) Obsidian에서 글 작성
2) rsync 로 content 최신화        (3번)
3) npx quartz build --serve 로 확인 (1번, 선택)
4) npx quartz sync --no-pull 로 배포 (4번)
```
