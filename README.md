# SBT_token — 양도 불가 토큰(Soulbound Token) PoC

자격·이력 증명처럼 **발급받은 사람에게만 묶여 있어야 하는 토큰**을 ERC-721 위에서 구현해 본 개념 검증(PoC)입니다. (2024년 작성)

## 설계

- **양도 차단:** ERC-721의 전송 경로 4개(`_transfer`, `transferFrom`, `safeTransferFrom` 2종)를 재정의해 모두 `revert`합니다.
- **발급 제한:** `mint`는 컨트랙트 소유자만 호출할 수 있습니다(`onlyOwner`).
- **메타데이터:** 두 가지 방식으로 나눠 작성했습니다.

| 파일 | 메타데이터 방식 |
|---|---|
| `contracts/SBT_Simple.sol` | 토큰마다 이름 문자열을 온체인에 저장 (`getTokenName`) |
| `contracts/SBT_Image_IpfsGateway.sol` | 발급 시 IPFS CID를 받아 `https://ipfs.io/ipfs/<CID>`를 `tokenURI`로 저장 |
| `contracts/SBT_Image_W3sGateway.sol` | 위와 같되 `https://<CID>.ipfs.w3s.link` 게이트웨이 사용 |

## 실행

Remix IDE에서 파일을 열고 컴파일·배포합니다. OpenZeppelin v4.4.1을 GitHub URL로 직접 가져오므로 별도 설치가 없습니다.

## 한계 (PoC 범위)

- 자동화된 테스트와 배포 기록이 없습니다.
- 토큰 회수(burn)·재발급 기능이 없습니다.
- 외부 라이브러리를 URL로 가져와 로컬 빌드 도구(Hardhat·Foundry)에서는 import 경로 수정이 필요합니다.
