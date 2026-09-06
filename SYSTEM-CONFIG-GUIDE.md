# GongBoo System Configuration Guide

## 핵심 원칙

**One setting = one management location.**

같은 URL, Key, 설정을 여러 파일에 중복 하드코딩하지 않는다.

## 1. Project URLs

관리 파일:

`BIBLEOFGONGBOO/gongboo-shared/catalog.js`

여기서만 관리: - Bible URL - License URL - Easy Learning URL - 향후
Hymns / History / Conversation 등 프로젝트 URL

`navigator.js`, `index.html`, 각 프로젝트 파일에는 프로젝트 URL을 중복
하드코딩하지 않는다.

## 2. Shared Navigator

관리 파일:

`BIBLEOFGONGBOO/gongboo-shared/navigator.js`

역할: - 좌측 상단 Select Study 메뉴 - `catalog.js`의 프로젝트 목록/URL
참조 - 선택한 프로젝트로 이동

원칙:

`navigator.js` 자체에 Bible/License/Easy Learning URL을 다시 적지
않는다.

## 3. Shared Home

관리 파일:

`BIBLEOFGONGBOO/gongboo-shared/index.html`

원칙:

프로젝트 URL을 직접 하드코딩하지 않고 `catalog.js`를 참조한다.

## 4. Bible Supabase

관리 파일:

`BIBLEOFGONGBOO/bible/supabase/app/supabase-config.js`

수정 항목: - `url` - `publishableKey` - `questionFunction` - `enabled`

형식:

``` javascript
window.BIBLE_SUPABASE_CONFIG = Object.freeze({
  url: "...",
  publishableKey: "...",
  questionFunction: "bible-content",
  enabled: true
});
```

원칙: - Bible 브라우저 Supabase URL/Publishable Key는 여기서 관리 -
`main.js`, `bible-explorer.js` 등에 중복 저장 금지 - Service Role Key는
GitHub에 저장 금지

## 5. Shared Authentication

관리 위치:

`BIBLEOFGONGBOO/gongboo-shared/auth/`

주요 파일: - `supabase-config.js` - `supabase-auth.js`

현재 Shared 기준 주소:

`https://bibleofgongboo.github.io/gongboo-shared/`

현재 시스템 실행 파일에서는 옛 원본 주소:

`https://biblegongboo.github.io/`

를 사용하지 않는다.

## 6. Bible Edge Function

Supabase:

`Edge Functions -> bible-content`

역할: - Bible question delivery - OT/NT access - Catalog - Source
lookup - Storage/content access

Service Role Key는 Supabase server-side secrets/environment에만 둔다.

## 7. Bible Multilingual FINAL

테이블:

`bible_question_translations`

기본 식별:

`record_id + lang`

주요 컬럼: - `record_id` - `lang` - `question` - `passage` -
`passage_kjv` - `option_1` \~ `option_4` - `explanation` - `chunk_1` \~
`chunk_5`

데이터: - EN OT - EN NT - KO NT - KO OT - 향후 다른 언어도 같은 구조에
추가

## 8. CHUNK

데이터:

`bible_question_translations`

같은 `record_id`에서 EN/KO CHUNK를 짝지어 사용한다.

표시 예:

`beginning — 시작  Good News — 복음  Jesus Christ — 예수 그리스도`

UI 위치:

**English Passage 바로 아래 / Korean Passage 바로 위**

`?` 버튼으로 표시/숨김.

## 9. Legacy Bible Tables

기존: - `bible_ot_questions` - `bible_nt_questions`

새 FINAL 구조 검증이 끝날 때까지 삭제하지 않는다.

확인 후 Legacy 처리: - EN/KO FINAL - answer - passage - CHUNK -
People/Places/Entity links - 문제 순서/로드

## 10. Shared Project Architecture

``` text
BIBLEOFGONGBOO/gongboo-shared
        |
        +-- Bible
        +-- License
        +-- Easy Learning
        +-- Hymns
        +-- History
        +-- Conversation
        +-- future projects
```

프로젝트가 늘어나도 공통 메뉴는 shared에서 관리한다.

## 11. Original System Isolation

현재:

`BIBLEOFGONGBOO`

옛 원본:

`biblegongboo`

현재 시스템의 실행 파일에서 다음 문자열이 남지 않도록 관리:

`biblegongboo.github.io`

필요 시 GitHub 전체 검색:

`biblegongboo`

찾기/바꾸기 기본:

`biblegongboo` -\> `bibleofgongboo`

단, 변경 후 실제 목적지가 현재 프로젝트인지 확인한다.

## 12. 프로젝트 이동/URL 변경 시 확인 순서

1.  `gongboo-shared/catalog.js`
2.  해당 프로젝트 Supabase config
3.  `gongboo-shared/auth/supabase-config.js`
4.  Supabase Edge Function 설정/Secrets
5.  Mobile/Capacitor config
6.  GitHub 전체에서 옛 hostname 검색

## Quick Reference

  ------------------------------------------------------------------------------
  설정                                수정 위치
  ----------------------------------- ------------------------------------------
  Project URLs                        `gongboo-shared/catalog.js`

  Select Study menu                   `gongboo-shared/navigator.js`

  Shared home                         `gongboo-shared/index.html`

  Bible Supabase URL/Publishable Key  `bible/supabase/app/supabase-config.js`

  Shared Auth Supabase                `gongboo-shared/auth/supabase-config.js`

  Bible API                           Supabase `Edge Functions -> bible-content`

  Bible FINAL multilingual data       `bible_question_translations`

  Mobile live URL                     `bible/mobile/capacitor.config.json`
  ------------------------------------------------------------------------------

## 최종 원칙

**설정은 중앙화하고, 기능 코드는 단순하게 유지하며, 한 곳에서 참조할 수
있는 값을 여러 파일에 중복 저장하지 않는다.**
