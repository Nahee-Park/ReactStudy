# 🙈 Intro

> 프로젝트를 진행하다 보면, 서버와 데이터를 주고받기 위해 HTTP 통신을 해야합니다.
> 오늘은 **Axios를 통해 HTTP 통신하는 방법**에 대해 상세하게 알아 보려고 합니다!
> 그 이전에 왜 React에서 주로 Axios를 이용해 통신하는지, HTTP 통신을 가능하게 하는 **ajax**와 **fetch**와 비교해서 보려고 합니다!

# 1. Why Axios?

ajax? fetch? axios?
API호출에는 다양한 방법들이 있습니다.
왜 React에서는 Axios를 가장 많이 이용할까요?

## 1-1. ajax

Ajax는 **Asynchronous JavaScript And XML**의 약자입니다.
말 그대로, 자바스크립트에서 비동기적 통신을 가능하게 합니다.
순수 ajax는 `XMLHttpRequest()`생성자를 통해 자바스크립트에서 구현될 수 있지만,
JQuery를 통해 ajax를 보다 쉽게 쓸 수 있기 때문에 주로 JQuery와 함께 쓰입니다.
**그러나 promise 기반이 아니고, JQuery를 사용하지 않으면 쉽게 구현하기 어렵다는 단점이 있습니다.**

> React에서 async, await과 사용될 때 **promise**의 위력이 어마어마하다는 사실을 생각해보면 ajax는 썩 매력적이지 않습니다.

## 1-2. fetch

fetch는 ES6부터 자바스크립트의 내장 라이브러리로 들어왔습니다.

> 1. **promise기반**으로 만들어졌다는 것
> 2. **내장 라이브러리**이기 때문에 별도의 모듈 설치가 필요하지 않다는 것

크게 두 가지 장점을 갖고 있습니다!
또한 코드도 단순합니다.

```javascript
//fetch
fetch(url, options)
  .then((response) => console.log("response:", response))
  .catch((error) => console.log("error:", error));
```

그러나 **브라우저 호환성이 떨어지고** response timeout 처리 방법이 없는 등 **기능적인 부분이 상대적으로 부족합니다.**
그럼에도 내장라이브러리인만큼 **안정적이지 않은 프레임워크**에선 유용하게 사용하기 좋습니다!

## 1-3. axios

~~결국 다 앞의 것들은 axios를 사용하기 위한 당위들이었을뿐~~
axios는 node.js와 브라우저를 위한 http 통신 라이브러리입니다.
fetch처럼 promise를 지원한다는 공통점이 있지만, fetch와는 달리 **브라우저 호환성이 좋고** 편리하며 **기능이 많습니다!**
라이브러리 설치가 필요하다는 단점이 있지만 fetch에 비해 기능상으로 더 디테일하다는 큰 장점이 있습니다.

> 그래서 **React에서 http통신**을 할 때엔 주로 **axios**를 이용합니다!
> **promise기반**에 **호환성이 좋고**, **디테일한 기능들**을 사용할 수 있기 때문입니다:)

# 2. Axios 설치하기

axios를 이용하기 위해선 라이브러리를 설치해야 합니다.

**npm 사용하기**
`npm install axios`

**yarn 사용하기**
`yarn add axios`

# 3. Axios 사용하기

전반적으로 axios를 통해 서버와 소통하는 과정은

> **1. 서버에 요청을 보내고(request)** > **2. 서버로부터 응답이 오면(response) 제대로 응답이 왔을 때와 못 왔을 때를 구분하여 처리**

이렇게 두 단계로 이뤄집니다.

서버에 요청을 보냈을 때 응답이 오기까지 시간이 걸리므로 서버에 보내는 요청은 **비동기 처리**를 해주며, 그 이후에 응답을 바탕으로 처리하는 과정은 **`.then`** 이나 **`await`**를 이용합니다.

우선 서버에 요청을 보내는, axios의 **request 메소드**에는 다음과 같은 네가지 메소드들이 있습니다!

**1. GET : 서버에서 어떤 데이터를 가져와서 보여줌 2. POST : 서버로 데이터를 보냄 3. PUT : 데이터베이스 내부 내용 갱신 4. DELETE : 데이터베이스 내부 내용 삭제**

이 네 가지 메소드를 사용하기 위해서는 아래와 같이 보내야하는 정보들이 있습니다.

**1. 어떤 메소드를 사용할 것인지 2. url 주소 3. data (선택적) 4. params(선택적) **

그래서 정석적으로 axios를 이용하는 방식은

```javascript
axios({
  //request
  method: "get",
  url: "url",
  responseType: "type",
}).then(function (response) {
  // response Action
});
```

이런 식으로 각각의 정보들을 직접 전달하여 사용합니다.
그러나 각각의 메소드별로 **단축형**을 사용하면 보다 편리하게 axios를 활용할 수 있습니다 :)
각 메소드 별로 단축형을 어떻게 이용할 수 있는 지 알아보겠습니다.

## 3-1. GET

> **axios.get(url[,config])**

서버에서 데이터를 가져오는 GET 메소드는 두 가지 상황이 존재합니다.
**1. 지정된 단순 데이터 요청을 수행하는 경우 -> url만 파라미터로 넘김 **

```javascript
async function getData() {
  try {
    //응답 성공
    const response = await axios.get("url주소");
    console.log(response);
  } catch (error) {
    //응답 실패
    console.error(error);
  }
}
```

**2. 사용자 번호에 따라 다른 데이터를 불러오는 경우-> url과 함께 prams:{} 객체도 파라미터로 넘김**

```javascript
async function getData() {
  try {
    //응답 성공
    const response = await axios.get("url주소", {
      params: {
        //url 뒤에 붙는 param id값
        id: 12345,
      },
    });
    console.log(response);
  } catch (error) {
    //응답 실패
    console.error(error);
  }
}
```

## 3-2. POST

> **axios.post(url, data[,config])**

서버에 데이터를 보내는 POST 메소드에서는 보내고자 하는 데이터를 **message body**에 포함시켜 보냅니다!
보내는 방식은 GET 메소드에서 params를 보내는 것과 유사합니다.

```javascript
async function postData() {
  try {
    //응답 성공
    const response = await axios.post("url주소", {
      //보내고자 하는 데이터
      username: "devstone",
      password: "12345",
    });
    console.log(response);
  } catch (error) {
    //응답 실패
    console.error(error);
  }
}
```

## 3-3. PUT

> ** axios.put(url,data[,config]) **

DB의 내용을 수정하는 PUT 메소드는 서버 내부적으로 get -> post 하는 과정을 거치기 때문에 **post메소드와 비슷한 형식**입니다.

```javascript
async function putData() {
  try {
    //응답 성공
    const response = await axios.put("url주소", {
      //항목에 새롭게 넣을 데이터
      username: "stone",
      password: "123",
    });
    console.log(response);
  } catch (error) {
    //응답 실패
    console.error(error);
  }
}
```

## 3-4. DELETE

> **axios.delete(url[,config]) **

데이터베이스 내부의 데이터를 삭제하는 DELETE 메소드는 **일반적으로 body가 비어있는 형태**이지만, query나 params가 많아서 **헤더에 많은 정보를 담기 어려운 상황에 한해서만** 두번째 인자에 data를 추가해줍니다.

**1. 일반적인 delete**

```javascript
async function deleteData() {
  try {
    //응답 성공
    const response = await axios.delete("url주소");
    console.log(response);
  } catch (error) {
    //응답 실패
    console.error(error);
  }
}
```

**2. 헤더에 정보가 많이 포함된 경우**

```javascript
async function deleteData() {
  try {
    //응답 성공
    const response = await axios.delete("url주소", {
      //헤더에 포함된 정보들
      data: {
        post_id: 1,
        comment_id: 13,
        username: "foo",
      },
    });
    console.log(response);
  } catch (error) {
    //응답 실패
    console.error(error);
  }
}
```

# 🙉 Reference

레퍼런스틑 쿠키파킹을 통해 확인하실 수 있습니다~
https://www.cookieparking.com/share/U2FsdGVkX18z9Herg6NGr-hUgLeAIt-dnXOsjLDoR52lhd0O86C4oUIqSZ7vmNfRZpGZjTe72gTiocVzeuwtoXTqa3QJs74a9v8c0-BFBbc
