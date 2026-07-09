# 공감 네컷 (양구초등학교)

손동작을 카메라로 인식해서 자동으로 이펙트가 나오는 4컷 포토부스입니다.

## 인식되는 손동작
- 👍 엄지척 → 응원해요
- ✌️ 브이 → 우리반 최고
- 🤟 아이러브유(엄지·검지·새끼손가락) → 사랑해요
- ✋ 손바닥 펴기 → 함께해요

## GitHub Pages로 올리는 방법

1. GitHub에서 새 저장소를 만듭니다 (예: `gonggam-necut`).
2. 이 폴더 안의 파일(`index.html`, `assets/` 폴더 전체)을 저장소에 그대로 업로드합니다.
   - `assets/logo.png`, `assets/mascot.png` 파일이 반드시 같은 경로에 있어야 합니다.
3. 저장소의 **Settings → Pages** 로 이동합니다.
4. **Source**를 `Deploy from a branch`로 설정하고, 브랜치는 `main`(또는 `master`), 폴더는 `/ (root)`로 설정 후 저장합니다.
5. 1~2분 후 `https://[본인아이디].github.io/gonggam-necut/` 주소로 접속하면 바로 작동합니다.

## 참고
- 손동작 인식은 브라우저에서 직접 실행됩니다(구글의 공개 MediaPipe 모델 사용). 별도 서버나 API 키가 필요 없습니다.
- 카메라 권한은 https 환경(GitHub Pages는 기본 https)에서만 정상 동작합니다.
- 인식이 잘 안 될 때는 밝은 곳에서, 손을 화면에 크게 보이도록 해주세요.
