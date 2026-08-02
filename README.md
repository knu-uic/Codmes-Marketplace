# Codmes Marketplace

Codmes가 기본으로 사용하는 공식 플러그인 Registry입니다. 이 저장소는 애플리케이션
서버가 아니라 검수된 `registry/index.json`의 변경 이력과 자동 검증·배포 workflow를
관리합니다.

Chat·Notes·Code·Planner처럼 Codmes에 포함되는 built-in plugin은 이 Registry에
등록하지 않습니다. built-in과 community plugin은 Codmes의 같은 Plugin Runtime을
사용하지만, 이 저장소는 KNU처럼 사용자가 선택해 설치하고 Codmes와 독립적으로
업데이트·제거할 수 있는 community plugin만 배포합니다.

## 설치 흐름

1. 개발자가 Ed25519로 서명한 package를 준비합니다.
2. 이 저장소에 Publisher와 plugin entry를 추가하는 Pull Request를 만듭니다.
3. PR Actions가 schema, 권한, SHA-256, package 서명, Publisher 상태를 검증합니다.
4. 운영자 검수와 merge 후 Pages workflow가 Registry를 다시 검증합니다.
5. workflow가 정적 `index.json`을 Marketplace root key로 서명하고
   `index.sig.json`, 검수된 package와 함께 GitHub Pages에 배포합니다.
6. Codmes는 내장된 root 공개키로 Registry를 검증한 뒤 package의 Publisher
   서명을 별도로 검증합니다.

Plugin entry는 Registry 기준 상대 `packagePath`를 사용합니다. 설치가 Publisher
저장소나 GitHub 조직 URL에 의존하지 않으며, Pages workflow도 현재 저장소의
`github.repository_owner`에서 형제 `Codmes` 저장소를 찾습니다. 조직을 다시 옮길
때에는 Codmes의 신뢰 루트 설정 한 곳만 새 Registry 주소로 바꾸면 됩니다.

## 공개 주소

- Registry: `https://knu-uic.github.io/Codmes-Marketplace/index.json`
- Detached signature:
  `https://knu-uic.github.io/Codmes-Marketplace/index.sig.json`
- Health: `https://knu-uic.github.io/Codmes-Marketplace/health.json`

## 외부 개발자 등록

개발자는 Codmes 저장소의 `docs/plugin/publishing.md`에 따라 Publisher key와 signed
package를 준비합니다. 개인키는 이 저장소나 PR에 절대 포함하지 않습니다. 최초
등록은 Publisher 소유권·저장소·개인정보 처리·권한을 사람이 함께 검수합니다.

외부 plugin 저장소는 Codmes GitHub 저장소나 공식 조직명을 참조하지 않습니다.
서명 package와 Registry 변경을 이 저장소에 PR로 제출하면 Marketplace Actions가
공식 Codmes validator를 사용해 최종 검증합니다.

운영 Registry는 `signaturePolicy: required`, `governancePolicy: reviewed`를
강제합니다. 설치 후 update의 `publisherId`도 Codmes 설치 상태에 고정되므로 다른
Publisher가 같은 plugin id를 탈취할 수 없습니다.

## 승인된 plugin 업데이트 준비

승인된 Publisher의 반복 릴리스는 `registry/index.json`을 수동 편집하지 않습니다.
Codmes Publisher CLI가 manifest의 버전을 읽어 서명 package 생성, SHA-256 계산과
Registry 갱신을 한 번에 수행합니다.

```sh
node /path/to/Codmes/bin/codmes.mjs plugin publisher prepare /path/to/plugin \
  --sign-key "$HOME/.codmes-publisher/<publisher>/private-key.pem" \
  --publisher-id <publisher-id> \
  --package-directory <plugin-slug> \
  --registry /path/to/Codmes-Marketplace/registry/index.json \
  --release-notes-file /path/to/release-notes.md
```

생성된 `registry/packages/<plugin-slug>/<version>.codmes-plugin`과 자동 갱신된
`registry/index.json`을 같은 Pull Request에 제출합니다.

KNU처럼 Marketplace 운영 조직이 관리하는 공식 Community plugin은 plugin
저장소에서 GitHub Release를 발행하면 위 명령, package 업로드, release 브랜치 push와
Pull Request 생성을 Actions가 대신 수행할 수 있습니다. 자동화는 Marketplace
`main`에 직접 쓰지 않으며 검증된 Pull Request를 사람이 병합하는 단계는 유지합니다.
