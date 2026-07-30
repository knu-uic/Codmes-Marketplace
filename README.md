# Codmes Marketplace

Codmes가 기본으로 사용하는 공식 플러그인 Registry입니다. 이 저장소는 애플리케이션
서버가 아니라 검수된 `registry/index.json`의 변경 이력과 자동 검증·배포 workflow를
관리합니다.

## 설치 흐름

1. 개발자가 자기 저장소의 GitHub Release에 Ed25519 서명 package를 올립니다.
2. 이 저장소에 Publisher와 plugin entry를 추가하는 Pull Request를 만듭니다.
3. PR Actions가 schema, 권한, SHA-256, package 서명, Publisher 상태를 검증합니다.
4. 운영자 검수와 merge 후 Pages workflow가 Registry를 다시 검증합니다.
5. workflow가 정적 `index.json`을 Marketplace root key로 서명해
   `index.sig.json`과 함께 GitHub Pages에 배포합니다.
6. Codmes는 내장된 root 공개키로 Registry를 검증한 뒤 package의 Publisher
   서명을 별도로 검증합니다.

## 공개 주소

- Registry: `https://jeongu0569-ui.github.io/Codmes-Marketplace/index.json`
- Detached signature:
  `https://jeongu0569-ui.github.io/Codmes-Marketplace/index.sig.json`
- Health: `https://jeongu0569-ui.github.io/Codmes-Marketplace/health.json`

## 외부 개발자 등록

개발자는 Codmes 저장소의 `docs/plugin/publishing.md`에 따라 Publisher key와 signed
package를 준비합니다. 개인키는 이 저장소나 PR에 절대 포함하지 않습니다. 최초
등록은 Publisher 소유권·저장소·개인정보 처리·권한을 사람이 함께 검수합니다.

운영 Registry는 `signaturePolicy: required`, `governancePolicy: reviewed`를
강제합니다. 설치 후 update의 `publisherId`도 Codmes 설치 상태에 고정되므로 다른
Publisher가 같은 plugin id를 탈취할 수 없습니다.
