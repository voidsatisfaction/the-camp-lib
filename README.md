# THE CAMP Unofficial Library

대국민 국군 소통 서비스 [더 캠프](https://www.thecamp.or.kr)를 사이트 외부에서 이용하기 위해 만든 비공식 라이브러리입니다. 현재 인터넷 편지 발송을 중심으로 구현되어 있습니다.

# Installation

```bash
$ npm install the-camp-lib --save
```

# Usage

```js
const thecamp = require('the-camp-lib');
// or
import thecamp from 'the-camp-lib';

async function main() {
  const cookies = await thecamp.login('user@email.com', 'password');
}
```

> 더 캠프는 로그인할 때 생성한 쿠키를 기반으로 세션을 식별합니다. 로그인이 필요한 서비스를 이용하려면 HTTP 요청 헤더에 로그인 시 생성된 쿠키를 포함해야 합니다.

# Development

저장소를 클론하고 패키지를 설치합니다.

```bash
$ git clone https://github.com/ParkSB/the-camp-lib.git
$ cd the-camp-lib
$ npm install
```

모든 코드는 src 디렉토리 아래에 있습니다. 차후 테스트 환경을 구축할 예정입니다. 그 전까지는 임의의 파일(`test.ts`)을 만들어 테스트할 코드를 작성하고 `ts-node test.ts`를 실행하는 방식으로 테스트 합니다.

쉘에서 `npm run build`를 실행하면 dist 디렉토리에 빌드된 파일이 만들어집니다. example 디렉토리에 예시 디렉토리를 만든 뒤 쉘에서 `npm init`, `npm install ../../ --save`를 실행하면 해당 디렉토리에 패키지가 설치됩니다. 이렇게 하면 로컬에서 배포 버전을 테스트할 수 있습니다.

# Specifications

> 라이브러리를 사용하기 전에 더 캠프에 가입된 계정이 필요합니다.

## Models

### `Cookie`

세션 식별을 위한 쿠키.

* `scouter: stirng`
* `jsessionid: string`

### `interface Trainee`

훈련병 정보.

* `unitCode: string` - 훈련병이 소속된 연대/사단 식별 코드 (ex. `00연대`, `00사단`)
* `groupId: string` - 카페 식별 코드
* `traineeName: string` - 훈련병 이름 (ex. `박뫄뫄`)
* `birth: number` - 훈련병 생년 월일 (ex. `19981129`)
* `relationship: Relationship` - 훈령병과의 관계

### `enum Relationship`

훈련병과의 관계.

* `MOTHER = '어머니'`
* `FATHER = '아버지'`
* `SPOUSE = '배우자'`
* `GRANDPARENTS = '조부모'`
* `SIBLING = '형제/남매'`
* `FRIEND = '친구'`
* `LOVER = '애인'`
* `RELATIVE = '친척'`

### `interface Message`

인터넷 편지 정보.

* `title: string` - 편지 제목
* `content: string` - 편지 내용 (2000자 이하)
* `boardId?: string` - 구현 예정.
* `fileInfo?: any[]` - 구현 예정.

### `interface Group`

가입 카페 정보.

* `unitName: string` - 훈련병이 소속된 연대/사단 이름 (ex. `00연대`, `00사단`) 
* `fullName: string` - 카페 전체 이름 (ex. `00연대 0교육대 00중대`)
* `enterDate: string` - 훈련병 입소 날짜 (ex. `20190829`)
* `groupId: string` - 카페 식별 코드.
* `groupName: string` - 카페 이름 (ex. `0교육대 00중대`)
* `groupImage: string` - 카페 대표 이미지.
* `accessDate: string` - 요청 날짜
* `unitCode: string` - 연대/사단 식별 코드.
* `unitType: number` - 육군 훈련소(`1`)/사단신교대(`2`) 여부.
* `grade: number`

## Services

### `login(id: string, password: string)`

더 캠프에 로그인해 세션 쿠키를 얻는다.

* Parameters
  * `id: string` - 더 캠프 계정 이메일.
  * `password: string` - 더 캠프 계정 비밀번호.
* Return value
  * `Cookie` - 세션 식별을 위한 쿠키.

### `fetchGroups(cookies: Cookie, unitName?: string, enterDate?: string)`

가입 카페 정보를 가져온다.

* Parameters
  * `cookies: Cookie` - 세션 식별을 위한 쿠키.
  * `unitName?: string (Optional)` - 훈련병이 소속된 연대/사단 이름 (ex. `00연대`, `00사단`)
  * `enterDate?: string (Optional)` - 훈련병 입소 날짜 (ex. `20190829`)
* Retrun value
  * `Group[]` - 가입한 카페 목록. `unitName` 또는 `enterDate`가 주어지면 해당 훈련병이 소속된 카페만 `Group[]` 타입으로 반환한다.

### `sendMessage(cookies: Cookie, trainee: Trainee, message: Message)`

인터넷 편지를 전송한다. 

* Parameters
  * `cookies: Cookie` - 세션 식별을 위한 쿠키.
  * `trainee: Trainee` - 훈련병 정보.
  * `message: Message` - 인터넷 편지 정보. `message.boardId`가 `null`이면 빈 문자열을, `message.fileInfo`가 `null`이면 빈 배열을 할당한다. 
* Return value
  * `request.Response` - 요청에 대한 HTTP 응답.

# Examples

* [send-message](examples/send-message): 인터넷 편지 전송 예시.

# License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.