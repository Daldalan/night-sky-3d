# 배포 가이드 — 밤하늘 천문대 (Night Sky 3D)

이 앱은 **단일 정적 파일 `index.html`** 입니다. 빌드 과정이 없고, Three.js와 지구 텍스처는
CDN(jsdelivr/unpkg)에서 로드합니다. 어떤 정적 호스팅에도 그대로 올리면 됩니다.

> 참고: 위치 자동 감지(Geolocation)와 CDN 텍스처 로드 때문에 **HTTPS**로 서빙되어야 합니다.
> 아래 두 서비스는 모두 자동으로 HTTPS를 제공합니다.

---

## 옵션 A — Vercel (권장, 가장 간단)

프로젝트 폴더에서:

```bash
npx vercel --prod
```

- 처음 실행하면 브라우저 로그인 → 프로젝트 이름 확인 몇 번이면 끝납니다.
- `.vercelignore`가 로컬 도구(`.claude`, 문서)를 제외하므로 `index.html`만 배포됩니다.
- `vercel.json`에 캐시/보안 헤더가 설정돼 있습니다.
- 배포가 끝나면 `https://<프로젝트>.vercel.app` 주소가 출력됩니다.

**대시보드 방식(로그인 CLI 없이):** https://vercel.com/new 에서 이 폴더를 GitHub에 올린 뒤 Import 하거나,
드래그 업로드로도 가능합니다.

---

## 옵션 B — Cloudflare Pages

`.claude` 같은 로컬 파일을 올리지 않도록 배포용 폴더에 `index.html`만 담아 올립니다:

```bash
# 배포용 폴더 준비 (index.html만 복사)
mkdir -p dist && cp index.html dist/

# 최초 1회: 로그인
npx wrangler login

# 배포
npx wrangler pages deploy dist --project-name=night-sky-3d
```

- 완료되면 `https://night-sky-3d.pages.dev` 주소가 나옵니다.
- 이후 업데이트: `cp index.html dist/` 후 위 `pages deploy` 명령만 다시 실행.

**대시보드 방식(CLI 없이):** https://dash.cloudflare.com → Workers & Pages → Create → Pages →
**Upload assets** 에서 `index.html`(또는 `dist` 폴더)을 드래그하면 됩니다.

---

## 업데이트하는 법
`index.html`을 수정한 뒤 위의 배포 명령을 다시 실행하면 새 버전이 반영됩니다.

## 의존성 메모
현재 다음을 외부 CDN에서 불러옵니다. CDN 장애 시 앱이 영향을 받을 수 있습니다.
- Three.js: `cdn.jsdelivr.net/npm/three@0.160.0`
- 지구 텍스처: `cdn.jsdelivr.net` / `unpkg.com` (실패 시 격자 폴백 내장)

완전 자립형으로 만들고 싶으면 Three.js를 로컬에 내려받아 `import` 경로만 바꾸면 됩니다.
