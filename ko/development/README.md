# 코드 개발 방법



다음은 소피아에서 코드를 작성하는 방법들에 대해 설명합니다.

소피아는 대표적인 두 폴더가 존재합니다. `%appdata%/sopia`, `%appdata%/bundles`.

개인 소피아 코드를 수정하려면 `sopia` 폴더를, 번들을 설치하거나 제작할 땐 `bundles` 폴더를 사용합니다.



## 폴더 구조

### SOPIA 폴더


소피아가 실행될 땐 무조건 `index.js`를 [require](https://nodejs.org/api/modules.html#requireid)하므로, `index.js`파일이 존재해야 합니다.

이외에 어떤 파일이든 생성하고 편집할 수 있습니다.


### Bundles 폴더

폴더 아래 하위 번들 폴더가 필요합니다.

하위 폴더 생성시 소피아는 번들로 인식합니다.

번들에 대한 자세한 내용은 [번들 제작 및 배포](./bundle.md) 문서를 참고해 주세요.


<br>
<br>

## 이벤트 목록

소피아 3의 코드모듈은 [cjs](https://ko.wikipedia.org/wiki/CommonJS) 형식을 사용합니다.

각 폴더의 `index.js` 파일에서 추출된 함수들을 기반으로 스푼의 소켓을 받게 되면 해당 이벤트 함수를 호출합니다.

<br><br>
이벤트 리스트는 [이 선언체](https://github.com/sopia-bot/sopia-core/blob/v2/src/socket/enum/index.ts#L16-L40)를 참고하십시오.

소켓 구현체는 [이 주소](https://github.com/sopia-bot/sopia-core/blob/v2/src/socket/live-socket.ts)를 확인하세요.
<br><br>

채팅을 받았을 때에 대한 이벤트를 가져오려면 함수를 다음과 같이 추출하면 됩니다.
<br><br>
```js
exports.live_message = (evt /* LiveMessageSocket */, sock /* LiveSocket */)  => {
  const message = evt.update_component.message.value;
  if ( message === 'ping' ) {
    sock.message('pong!');
  }
}
```
<br><br>
누군가 방송에 들어왔을 때 `live_join` 이벤트가 발생합니다.
<br><br>
```js
exports.live_join = (evt /* LiveJoinSocket */, sock /* LiveSocket */)  => {
  sock.message(`Hello. ${evt.data.author.nickname}!`)
}
```

<br><br>

## 노드 모듈

Node.JS가 지원하는 모듈을 사용할 수 있습니다.

지원하는 모듈 정보는 [이곳](https://nodejs.org/docs/latest-v16.x/api/index.html)을 확인하세요.


<br><br>

파일을 읽고 쓰는 예제
```js
const fs = window.require('fs');
const path = window.require('path');

const TARGET_PATH = path.join(__dirname, 'file.txt');

exports.live_message = (evt /* LiveMessageSocket */, sock /* LiveSocket */)  => {
  const message = evt.update_component.message.value;
  if ( message === '.쓰기' ) {
    const nickname = evt.data.author.nickname;
    fs.writeFileSync(TARGET_PATH, nickname, 'utf8');
    sock.message(`파일을 썼습니다. 작성자: ${nickname}`);
  } else if ( message === '.읽기' ) {
    if ( fs.existsSync(TARGET_PATH) ) {
      const nickname = fs.readFileSync(TARGET_PATH, 'utf8');
      sock.message(`파일을 읽었습니다. 내용: ${nickname}`);
    } else {
      sock.message('읽을 파일이 없습니다.');
    }
  }
}
```

<br><br>

## Sopia API

스푼의 직접적인 정보를 가져오려면 [sopia-core](https://github.com/sopia-bot/sopia-core) 를 사용해 API를 호출할 수 있습니다.

생성된 클래스 객체는 `window.$sopia` 에 담겨있으며, 모든 객체 정보는 `window.$spoon`에 담겨있습니다.
<br><br>
언/팔로우 예제
```js
exports.live_message = async (evt /* LiveMessageSocket */, sock /* LiveSocket */)  => {
  const message = evt.update_component.message.value;
  if ( message === '.팔로우' ) {
    await window.$sopia.api.users.follow(evt.data.author);
    sock.message(`${evt.data.author.nickname} 님을 팔로우했습니다.`);
  } else if ( message === '.언팔로우' ) {
    await window.$sopia.api.users.unfollow(evt.data.author);
    sock.message(`${evt.data.author.nickname} 님을 언팔로우했습니다.`);
  }
}
```
