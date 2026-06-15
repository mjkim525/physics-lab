# Yonsei NPL 웹사이트 인수인계 가이드

> **Kim Minjae's Nuclear Physics Laboratory @ Yonsei University**  
> 배포 주소: **https://mjkim525.github.io/physics-lab**  
> GitHub 리포: **https://github.com/mjkim525/physics-lab**

---

## 목차

- [기술 스택](#기술-스택)
- [로컬 개발 환경](#로컬-개발-환경)
- [배포 방법](#배포-방법)
- [콘텐츠 수정 가이드](#콘텐츠-수정-가이드)
  - [기본 정보](#1-기본-정보-_configyml)
  - [소개 페이지](#2-소개-페이지-_pagesaboutmd)
  - [논문](#3-논문-_bibliographypapersbib)
  - [뉴스](#4-뉴스-_news)
  - [구성원](#5-구성원-_pagesprofilesmd)
  - [CV](#6-cv-_datacvyml)
  - [소셜 링크](#7-소셜-링크-_datasocialsyml)
- [커밋 전 체크리스트](#커밋-전-체크리스트)

---

## 기술 스택

| 항목       | 내용                                                 |
| ---------- | ---------------------------------------------------- |
| 프레임워크 | [Jekyll](https://jekyllrb.com/) (al-folio 테마 기반) |
| 호스팅     | GitHub Pages                                         |
| 배포       | GitHub Actions (push 시 자동)                        |
| 포맷터     | Prettier                                             |

---

## 로컬에서 웹사이트 열기

### 1단계: Docker Desktop 설치

https://www.docker.com/products/docker-desktop 에서 운영체제에 맞게 다운로드 후 설치하세요.

### 2단계: 코드 받기

```bash
git clone https://github.com/mjkim525/physics-lab.git
cd physics-lab
```

### 3단계: 서버 실행

```bash
docker compose up
```

처음 실행 시 5~10분 정도 소요될 수 있습니다 (이미지 다운로드).

### 4단계: 브라우저에서 확인

**http://localhost:8080** 을 열면 사이트가 보입니다.

### 종료

터미널에서 `Ctrl + C`

> 두 번째 실행부터는 3단계(`docker compose up`)만 하면 됩니다.

---

## 배포 방법

`main` 브랜치에 push하면 GitHub Actions가 자동으로 빌드 및 배포합니다.

```bash
git add <수정한 파일>
git commit -m "설명"
git push origin main
```

Actions 진행 상황: https://github.com/mjkim525/physics-lab/actions

---

## 콘텐츠 수정 가이드

### 1. 기본 정보 (`_config.yml`)

사이트 전체에 영향을 주는 전역 설정입니다.

| 항목        | 키                        | 설명                     |
| ----------- | ------------------------- | ------------------------ |
| 사이트 제목 | `title`                   | 브라우저 탭, 헤더에 표시 |
| 연구실명    | `lab_name`                | 콘텐츠 헤더에 표시       |
| 대표자 이름 | `first_name`, `last_name` | 교수님 성함              |
| 연락처 안내 | `contact_note`            | 지원 문의 안내 문구      |
| 키워드      | `keywords`                | SEO용 메타 태그          |
| 배포 URL    | `url`, `baseurl`          | GitHub Pages 주소        |

> **주의:** `url`과 `baseurl`은 항상 함께 수정하세요.  
> 현재 값: `url: https://mjkim525.github.io/physics-lab/` / `baseurl: /physics-lab`

---

### 2. 소개 페이지 (`_pages/about.md`)

메인 페이지(홈)의 텍스트와 프로필 사진을 수정합니다.

```markdown
---
layout: about
title: about
permalink: /
---

여기에 연구실 소개 텍스트를 작성하세요.
```

프로필 이미지는 `assets/img/` 에 넣고 frontmatter의 `image` 항목을 수정하세요.

---

### 3. 논문 (`_bibliography/papers.bib`)

BibTeX 형식으로 논문을 추가하면 Publications 페이지에 자동 반영됩니다.

```bibtex
@article{kim2024example,
  title   = {논문 제목},
  author  = {Kim, Minjae and 공저자},
  journal = {저널명},
  year    = {2024},
  url     = {https://doi.org/...}
}
```

al-folio 전용 필드:

- `abbr`: 배지 레이블 (예: `abbr={PRL}`)
- `selected={true}`: 홈 화면 Featured 논문으로 표시
- `pdf`, `code`, `poster`: 링크 버튼 추가

---

### 4. 뉴스 (`_news/`)

`_news/` 폴더에 마크다운 파일을 추가하면 News 페이지에 표시됩니다.

파일명 규칙: `announcement_XXXX.md` (번호를 올려서 생성)

```markdown
---
layout: post
date: 2026-06-15
inline: true
---

새로운 뉴스 내용을 여기에 작성하세요.
```

- `inline: true`: 뉴스 목록에 한 줄로 표시
- `inline: false`: 별도 페이지로 연결

---

### 5. 구성원 (`_pages/profiles.md`)

연구실 멤버 정보를 수정합니다. 프로필 사진은 `assets/img/` 에 추가하세요.

---

### 6. CV (`_data/cv.yml`)

CV 페이지의 데이터를 YAML 형식으로 관리합니다. 학력, 경력, 수상 등 섹션별로 구성되어 있습니다.

---

### 7. 소셜 링크 (`_data/socials.yml`)

GitHub, Google Scholar, 이메일 등 링크를 수정합니다.

```yaml
- icon: fab fa-github
  url: https://github.com/mjkim525
- icon: ai ai-google-scholar
  url: https://scholar.google.com/...
```

---

## 커밋 전 체크리스트

```bash
# 1. Prettier 포맷 적용 (처음 한 번만: npm install --save-dev prettier @shopify/prettier-plugin-liquid)
npx prettier . --write

# 2. 로컬에서 빌드 확인
docker compose up --build
# → http://localhost:8080 에서 페이지, 이미지, 다크모드 확인

# 3. 커밋 & 푸시
git add <파일>
git commit -m "설명"
git push origin main
```

> Prettier 검사는 GitHub Actions에서도 자동으로 실행됩니다.  
> 포맷이 맞지 않으면 Actions가 실패하므로 반드시 로컬에서 먼저 실행하세요.
