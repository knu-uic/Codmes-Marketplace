# Plugin 등록 기여

1. 자신의 Publisher Ed25519 key로 `.codmes-plugin` package를 서명합니다.
2. package를 자신의 GitHub Release에 version별 immutable asset으로 올립니다.
3. `registry/index.json`에 Publisher 신청과 plugin entry를 추가해 Pull Request를
   만듭니다.
4. 자동 검수가 package URL을 다운로드해 SHA-256과 서명, manifest, 권한,
   release note를 확인합니다.
5. 최초 Publisher는 저장소 소유권, 개인정보 처리, 외부 서버와 권한 범위를 사람이
   추가 검수합니다.

개인키, 사용자 credential, API token은 PR이나 package에 포함하면 안 됩니다.
기존 release asset을 같은 version으로 교체하지 말고 반드시 새 version을
발행하세요.
