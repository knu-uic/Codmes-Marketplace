# Plugin 등록 기여

1. 자신의 Publisher Ed25519 key로 `.codmes-plugin` package를 서명합니다.
2. package를 `registry/packages/<plugin-id>-<version>.codmes-plugin`에 추가합니다.
3. `registry/index.json`에 Publisher 신청과 `packagePath`를 사용하는 plugin
   entry를 추가해 Pull Request를 만듭니다.
4. Marketplace 자동 검수가 SHA-256과 서명, manifest, 권한, release note를
   최종 확인합니다.
5. 최초 Publisher는 저장소 소유권, 개인정보 처리, 외부 서버와 권한 범위를 사람이
   추가 검수합니다.

개인키, 사용자 credential, API token은 PR이나 package에 포함하면 안 됩니다.
기존 release asset을 같은 version으로 교체하지 말고 반드시 새 version을
발행하세요.

Community plugin 저장소에는 Codmes 저장소를 checkout하는 검증 workflow가
필요하지 않습니다. 개발자는 설치된 Codmes CLI로 로컬 검증할 수 있고, 공개
배포의 신뢰 기준은 이 저장소의 Pull Request 검증 결과입니다.
