# airbnb-clone-project-front

## tailwind와 NextJS, React, JavaScript 사용하여 페이지 별로 나눠서 기능 구현

🏷️ 저는 주로 모달 생성과 세부 기능 구현 + room 상세 페이지에서 review 데이터를 전송하는 역할을 맡았습니다.

## Problem

### dummyData 위치

![Untitled](https://github.com/lusate/airbnb-clone-project-front/assets/95400441/4a2eb55b-e06f-4d36-a228-5d1289c921aa)


- src 하단에 dummyDatas 폴더를 생성해서 더미로 사용할 JSON 파일들을 넣어두었다.
- ~~Data는 그냥 복수다. Datas라니…~~

- 해당 더미 JSON을 import를 통해 가져오려 했는데, 이를 가져오지 못하는 문제가 발생했다.

### 문제 발생 코드

![Untitled (3)](https://github.com/lusate/airbnb-clone-project-front/assets/95400441/0b4319e3-1387-4472-8c1d-9c5ef4feb259)


위와 같이 import를 통해 바로 가져와서 사용하려 했는데 데이터가 반환되지 않는다는 오류 발생.

후에 console.log(roomsData)를 찍어보니 roomsData는 제대로 가져와 지는 것을 확인했고, 검증 부분에서 걸린 것으로 보인다. → 이 부분은 후에 찾아보도록 하자.

---

## Problem solution

- **Route Handler**를 통해서 API를 fetch하는 방식을 통해 데이터를 가져오는 방식으로 해결했다.
- 이를 위한 공식 문서는 [여기](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)를 참조

### api 파일 생성

![Untitled (4)](https://github.com/lusate/airbnb-clone-project-front/assets/95400441/e4a3e7c9-57ab-4c97-9c95-6711faf557d7)


1. src 하단에 api 폴더를 생성
2. 원하는 경로에 맞게 폴더를 생성
    1. id 등의 특정 파라미터를 사용한다면, 원하는 페이지의 경로와 같게 구성해주면 된다.
3. 원하는 위치에서 `route.js` 파일을 생성

### 2. route.js 파일 구현

원하는 메서드를 구현해 사용한다.

```jsx
// route.js

import roomsData from "../../../../dummyDatas/roomsData.json";

/**
 * 
 * @param {*} request 
 * @param {*} param1 
 * @returns 
 */
export async function GET(request, { params }) {
  const id = params.id
  const room = roomsData.Room.find((room) => room.id === parseInt(id));

  if (!room) {
    return new Response("Not Found", { status: 404 });
  }

  return Response.json({ data: room });
}
```

- 해당 코드는 GET 방식으로 데이터를 읽어오는 데이터이며, params로 id값을 받아서 해당 id에 맞는 Room 데이터를 반환해준다.
- 이때, 원하는 데이터만 골라서 가져올 수도 있다.

### 3. Component에서 API fetch

위에서 생성한 API를 컴포넌트에서 사용하는 방법은 다음과 같다.

![Untitled (5)](https://github.com/lusate/airbnb-clone-project-front/assets/95400441/74be505f-c989-4fc8-95b1-492adce519b4)


1. await fetch()를 통해 생성한 파일경로의 API를 가져온다.

    1. 이때, 사용할 id 파라미터를 넣어준다.
2. 받아온 result를 json으로 변환하고, 그 안의 data 필드를 사용한다.

→ 받아온 JSON 확인

![Untitled (6)](https://github.com/lusate/airbnb-clone-project-front/assets/95400441/f34e3fc6-f9b2-4f2b-89c4-26a56e7037a1)

![Untitled](https://github.com/lusate/airbnb-clone-project-front/assets/95400441/44f241b8-43ec-47e6-898f-6111bd00425a)


![Untitled (1)](https://github.com/lusate/airbnb-clone-project-front/assets/95400441/def85c23-50f9-4604-b981-bb397c5d91b2)

