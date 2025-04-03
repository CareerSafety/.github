# Next.js Folder Convention (App Router)
작성자 : 최락현  
최종 수정: 2025.04.03

### 개요  
-	목적: 프로젝트 구조를 일관되고 효율적으로 관리하여, 코드 가독성 및 유지보수성을 높인다.

-	대상: Next.js + TypeScript/JavaScript 프로젝트  
현재 문서가 수정된 2025.04.03 기준으로 github organization repository `careerAdmin`, `careerClient` 프로젝트를 대상으로 한다.

<br>
본 문서는 Next.js App Router 기반 프로젝트에서, pages 폴더 및 containers 폴더를 구성하여 작업하는 방식을 예시로 들어 설명합니다.

<br>
또한, 전역/공통으로 사용하는 컴포넌트와, 특정 페이지에 종속적인 컴포넌트를 구분하여 작업하는 방법을 안내합니다.


### 전체 구조 예시

```
my-project/  
├── app/  
│   ├── pages/  
│   │   ├── (safetyhealthmanagementofficevaluation)/  
│   │   │   ├── shmoe/  
│   │   │   │   ├── list/  
│   │   │   │   │   └── page.tsx  
│   │   │   │   ├── detail/  
│   │   │   │   │   └── page.tsx  
│   │   │   │   └── ... (기타 라우트)  
│   │   └── ...  
│   ├── containers/  
│   │   ├── safetyHealthManagementOfficeEvaluation/  
│   │   │   ├── list/  
│   │   │   │   └── index.tsx  
│   │   │   ├── detail/  
│   │   │   │   └── index.tsx  
│   │   │   ├── hooks/  
│   │   │   │   └── useSomeHook.ts  
│   │   │   └── ... (기타 폴더 및 파일)  
│   ├── components/  
│   │   ├── Header.tsx  
│   │   ├── Aside.tsx  
│   │   ├── MyDatePicker.tsx  
│   │   ├── RangeSlider.tsx  
│   │   └── ...  
│   ├── hooks/  
│   │   └── useUserSession.ts  
│   └── ...  
├── public/  
├── ... (기타 설정 파일들)  
└── README.md  
```

위 구조에서 주요 포인트는 다음과 같습니다

#### 1.	app/pages/ 폴더:
• 실제 로직이 담긴 컴포넌트들을 라우팅 시킵니다.  
• 현재 폴더구조는 `app/pages/(safetyhealthmanagementofficevaluation)/shmoe/list/page.tsx`   
같은 형식으로 해당 페이지를 영어로 번역하여 ()괄호로 표기하고 그 약자를 폴더로 만들어 작업하고 있습니다.

#### 2.	app/containers/ 폴더:
•	특정 페이지(라우트)에 대한 실제 로직이 담긴 컴포넌트들을 저장합니다.  

•	예:
`app/pages/(safetyhealthmanagementofficevaluation)/shmoe/list/page.tsx` 에서 사용할 주요 UI/로직을
`app/containers/safetyHealthManagementOfficeEvaluation/list/index.tsx` 에 작성합니다.  
•	추후 동일한 그룹(safetyHealthManagementOfficeEvaluation) 내에 detail, create, hooks, utils 등 필요한 폴더를 자유롭게 생성할 수 있습니다.  
•	향후 컴포넌트를 세분화해서 작성해야 할 경우, 이 컨테이너 폴더 내부에서 폴더 구조를 마음대로 확장할 수 있습니다.

#### 3.	app/components/ 폴더:
•	프로젝트 전역에서 재사용 가능한, 공통 컴포넌트를 저장합니다.  
•	예: Header, Aside, MyDatePicker, RangeSlider 등  
•	초기에는 해당 containers 폴더 내부에서만 쓰던 컴포넌트가 점차 확장되어 전역 공통으로 쓰일 때, 이 폴더로 옮길 수도 있습니다.

#### 4.	app/hooks/ 폴더:
•	전역적으로 사용되는 Custom Hook 모음.  
•	페이지나 기능(컨테이너) 전용 훅이 아니라면 여기에서 관리합니다.

---

### 폴더/파일 네이밍 컨벤션

####  1.	폴더명

•	기능/도메인 이름을 직관적으로 표시.  
•	긴 영어 표현인 경우, 약자를 따서 표현하거나, 별도의 작은 단위 폴더로 쪼개서 관리.  
•	예) `(safetyhealthmanagementofficevaluation)/폴더` → 실제 라우트 경로로 사용 가능.

#### 2.	파일명

•	페이지 컴포넌트: page.tsx (Next.js 13 App Router 규칙)  
•	컨테이너 컴포넌트: index.tsx 혹은 해당 역할에 맞는 이름 (예: list.tsx, detail.tsx 등)  
•	일반 컴포넌트: PascalCase(MyButton.tsx, DatePicker.tsx)
•	hooks: useSomeHook.ts (React Hook 명명 규칙)  

#### 3.	Import/Export

•	컨테이너 폴더 내의 메인 컴포넌트는 index.tsx로 export 하는 경우가 많다.  
•	그 외 분리된 컴포넌트는 폴더 분리 후 개별 파일로 분리 가능.  
• 해당역할에 맞게 직관적으로 표시.  
• 예) `import shmoeList from '/containers/safetyHealthManagementEvaluation/list/index.tsx';`
  
  ---

### 예시: Page ↔ Container 연결

아래는 page.tsx에서 컨테이너 컴포넌트를 가져와 연결하는 예시입니다.

#### 1) Page 컴포넌트 (app/pages/…/list/page.tsx)
   
```
import React from 'react';
import ShmoeList from '../../../../containers/safetyHealthManagementOfficerEvaluation/list/index';

const ShmoeListPage = () => {
  return (
    <>
      <ShmoeList />
    </>
  );
};

export default ShmoeListPage;
```

•	경로 설명:  
`app/pages/(safetyhealthmanagementofficevaluation)/shmoe/list/page.tsx`에서
`../../../containers/safetyHealthManagementOfficerEvaluation/list/index.tsx` 파일을 import.  

•	역할: Next.js에서 해당 파일(page.tsx)은 라우팅 엔트리 역할을 하며, 여기서 필요한 컨테이너/컴포넌트를 렌더링한다.


#### 2) Container 컴포넌트 (app/containers/…/list/index.tsx)

```
'use client';

import React, { useState, useEffect, useCallback } from 'react';
import Link from 'next/link';
import Select from 'react-select';
import Header from '../../../components/Header';
import Aside from '../../../components/Aside';
import MyDatePicker from '../../../components/SingledatePicker';
import RangeSlider from '../../../components/RangeSlider';
import useUserSession from '../../../hooks/useUserSession';
import { useRouter } from 'next/navigation';
import moment from 'moment';

export default function ShmoeList() {
  // ...
  // 실제 로직, API 호출, 상태관리, 이벤트 핸들러 등 구현
  // ...

  return (
    <div>
      <Header />
      <div className="contents">
        <div className="contents_wrap">
          <Aside />
          <div className="content">
            {/* 상단 네비게이션, 검색 영역, 테이블, 페이지네이션 등 */}
            ...
          </div>
        </div>
      </div>
    </div>
  );
}
```

•	`'use client';` : Next.js App Router에서 클라이언트 컴포넌트로 사용하기 위한 선언.  

#### • 역할
- 페이지에서 렌더링되는 실제 UI와 비즈니스 로직을 담당.  
- API 연동, 상태 관리, 이벤트 핸들링 등.  

#### • 구조
-	프로젝트 전역에서 쓰는 컴포넌트(Header, Aside)는 app/components/ 폴더에서 import.  
-	특정 컨테이너(ShmoeList 내부)에서만 쓰이는 컴포넌트가 있다면, 동일 컨테이너 폴더 또는 하위 폴더에 생성.

---
### 컨테이너 내부 컴포넌트 분리

작업하다 보면 한 컨테이너 파일(index.tsx)이 너무 커질 때가 있습니다.  
예를 들어, list/index.tsx 내부 로직이 방대해지면 다음과 같이 구조를 세분화할 수 있습니다.

```
containers/
└── safetyHealthManagementOfficeEvaluation/
    ├── list/
    │   ├── index.tsx
    │   ├── TableSection.tsx
    │   ├── SearchForm.tsx
    │   └── Pagination.tsx
    ├── detail/
    │   └── index.tsx
    └── hooks/
        └── useExampleHook.ts
```


#### •	index.tsx   
- 전체 페이지(컨테이너) 관장.  
- 필요한 섹션 컴포넌트(TableSection, SearchForm, Pagination 등)를 모아서 하나의 UI로 구성.  
#### •	TableSection.tsx
- 테이블만 담당.  
#### •	SearchForm.tsx 
- 검색 영역만 담당.  
#### •	Pagination.tsx 
- 페이징 UI만 담당.  

<br>
이런 식으로 세분화하면 가독성과 재사용성이 올라갑니다.

---
### 공통 컴포넌트 / 훅

•	전역적으로 사용될 가능성이 높은 컴포넌트는 app/components/ 폴더로 이동하여 관리.  
•	전역적으로 사용될 가능성이 높은 Custom Hook은 app/hooks/ 폴더에서 관리.

#### 예: 공통 컴포넌트 이동
```
app/components/
├── Header.tsx
├── Aside.tsx
├── MyDatePicker.tsx
├── RangeSlider.tsx
└── ...
```

#### 예: 공통 훅 이동
```
app/hooks/
└── useUserSession.ts
```
•	safetyHealthManagementOfficeEvaluation/hooks/ 폴더에 있던 훅이 프로젝트 전역에서 쓰인다 판단되면, app/hooks/로 옮긴다.

---

### 주의 및 참고사항
#### 1. 라우트 그룹 (Route Group)
- Next.js 13에서는 괄호(())로 감싸인 폴더가 실제 URL 세그먼트로 노출되지 않는 라우트 그룹으로 동작합니다.  
- 예: `app/(marketing)/about/page.tsx` → /about으로 접근  
이 문서 예시처럼 (safetyhealthmanagementofficevaluation) 폴더를 사용하면 실제 URL 경로에 문자열이 드러나지 않습니다.

<!-- #### 2. 파일 경로

-	import ... from '...' 구문에서 상대 경로로 접근 시, 경로가 헷갈릴 수 있으므로, tsconfig.json에 baseUrl 등을 설정하여 절대 경로 import를 사용하는 방법도 있습니다.
-	예) import ShmoeList from '@/containers/safetyHealthManagementOfficeEvaluation/list'; -->

#### 2. 서버 컴포넌트 vs 클라이언트 컴포넌트

-	Next.js 13 App Router에서는 기본적으로 서버 컴포넌트가 디폴트입니다.  
-	useState, useEffect, useCallback 등을 사용하는 컴포넌트는 반드시 use client를  선언해야 합니다.
-	컨테이너의 역할상 대부분 클라이언트 컴포넌트로서 상태나 로직을 다루게 되므로, containers 폴더에 있는 컴포넌트들은 보통 use client 선언이 필요합니다.

---

### 결론

#### •	app/pages 폴더: 
- 실제 라우트 구조를 담는 폴더.  
-	page.tsx를 각 페이지 진입점으로 사용.  

####	• app/containers 폴더: 
- 비즈니스 로직, 복잡 UI, 상태 관리를 담당하는 핵심 컨테이너 컴포넌트.  

#### • app/components 폴더: 
- 전역적으로 공유하는 단일 책임 컴포넌트.  

#### •	app/hooks 폴더: 
- 전역 Custom Hook 모음.  

<br>
이런 식으로 폴더를 명확히 분리하면, 유지보수나 협업 시에 특정 부분의 코드를 쉽게 찾아갈 수 있고, 코드가 복잡해졌을 때도 구조적으로 확장하기가 수월해집니다.  

<br>
<br>
이 외 추가 문의사항이 있다면 문의 바랍니다.
