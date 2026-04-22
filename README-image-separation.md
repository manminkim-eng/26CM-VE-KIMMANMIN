# 이미지 분리 작업 결과

## 작업 개요
`index.html`에 base64로 인라인 인코딩되어 있던 이미지 9개를 별도 파일로 분리하고, 파일 경로 링크로 교체했습니다.

## 용량 변화

| 항목 | 작업 전 | 작업 후 | 비고 |
|------|---------|---------|------|
| index.html | **4.19 MB** | **54 KB** | **98.7% 감소** |
| 이미지 (별도) | (HTML 내부) | 2.9 MB | 9개 → 8개 (중복 1개 통합) |
| 전체 합계 | 4.19 MB | 약 2.95 MB | 약 30% 감소 |

> HTML 자체 용량이 극적으로 줄어들어 GitHub Pages 로딩 속도가 크게 개선됩니다. 이미지는 브라우저 캐시 + `loading="lazy"` 속성으로 재방문 시 추가 절약됩니다.

## 디렉토리 구조

```
KIMMANMIN-portfolio/
├── index.html
└── assets/
    └── images/
        ├── jeonju-convention-center.jpg     (308 KB) ← hero 배경 + 카드 양쪽에서 재사용
        ├── jeonju-stadium.jpg               (358 KB)
        ├── iksan-central-city.jpg           (402 KB)
        ├── iksan-eoyang-residential.jpg     (389 KB)
        ├── jewelry-industrial-center.jpg    (340 KB)
        ├── iksan-jewelry-rd-center.jpg      (475 KB)
        ├── gunsan-namjung-middle.jpg        (195 KB)
        └── gunsan-sangil-high.jpg           (406 KB)
```

## 추가 발견사항

- **중복 이미지 1건 통합**: hero 영역 배경 이미지와 "전주 전시컨벤션센터" 카드 이미지가 완전히 동일한 파일이었습니다(MD5 해시 일치). `jeonju-convention-center.jpg` 하나를 양쪽에서 재사용하도록 수정해 추가로 약 308 KB 절약했습니다.

## GitHub Pages 배포 방법

1. 로컬 저장소에 `assets/images/` 폴더를 만들고 이미지 8개를 복사합니다.
2. 기존 `index.html`을 새 버전으로 교체합니다.
3. 커밋 후 푸시합니다:
   ```bash
   git add index.html assets/images/
   git commit -m "Refactor: separate base64 images into external files (-98.7% HTML size)"
   git push
   ```

## 구조 검증 결과

수정 후 HTML 태그 카운트가 원본과 100% 일치함을 확인했습니다 (div 221, section 6, article 4, img 8, h1 1, h2 5, h3 18, p 18, li 19, span 58 등 모두 일치).

## 향후 추가 최적화 제안 (선택사항)

현재 모든 이미지가 JPEG 형식인데, 다음을 고려하면 추가로 30~50% 더 줄일 수 있습니다:

1. **WebP 변환**: 동일 품질에서 JPEG 대비 25~35% 작음
2. **이미지 리사이징**: 현재 일부 이미지가 표시 크기보다 큼 (예: 카드 썸네일은 600px 폭이면 충분)
3. **반응형 이미지**: `<picture>` 태그 + `srcset`으로 모바일/데스크탑 다른 해상도 제공

필요하시면 다음 세션에서 진행해드릴 수 있습니다.
