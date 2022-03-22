![](https://images.velog.io/images/vgihan/post/c8ac4d2d-baf8-4c90-9d60-400bf3b0ab55/image.png)

SPA 개발에 있어서 빼놓을 수 없는 Webpack에 대해 알아보자. 아래부터 "번들링" 이라는 용어는 여러 개로 분리된 파일들을 같은 역할을 하는 하나의 파일로 묶어준다라는 의미로 쉽게 받아들이고 가면 될 것이다.

### Webpack 이란?

자바스크립트 모듈 번들러이다. entry point로 부터 연결된 모든 파일들을 하나의 번들로 묶어 실제 개발 시 여러 개의 모듈로 분리하여 개발할 수 있도록 도와준다.

### Webpack을 사용하지 않으면?

기본적으로 자바스크립트는 html 파일에 script 태그를 통해 외부 script 파일을 가져온다. 여러 개의 자바스크립트를 가져와 사용해야하는 경우 역시 html 파일 내부 script 태그를 여러 개 사용하여 추가할 수 있다. 하지만, script 태그를 통해 삽입하는 방식으로는 파일마다 독립적인 스코프를 가지고 있지 않기 때문에 전역 변수를 공유한다는 문제가 존재한다. 전역 변수를 공유할 경우, 함수명 혹은 변수명이 겹치는 문제가 발생할 수 있다.

IIFE(즉시 실행 함수)를 이용하여 네임 스페이스를 분리하여 사용할 수 있는데, 아래와 같다.

```javascript
(function () {
  var a = 1;
  var b = 2;
})(function () {
  var a = 4;
  var b = 6;
});
```

이런식으로 스코프를 분리할 수 있다. webpack은 entry point로 부터 import 된 모든 모듈을 하나로 번들링 하여 이러한 문제를 해결한다.

### type="module" vs webpack

웹 페이지를 개발할 때, js와 또 한가지가 존재한다. 바로 css다. css를 적용하기 위해선, js와 마찬가지로 html에 css 파일 경로를 link 태그를 이용하여 연결시켜 주어야 한다. 이렇게 되면 컴포넌트 단위로 개발을 진행하면서 css 역시 하나의 컴포넌트 단위로 묶여 파일 분리가 일어나야 하는데, webpack을 사용하지 않고 파일 분리를 위해선 html에 link 태그를 추가하여 개발하는 방법 밖에 떠오르지 않는다.

결론부터 말하자면, webpack은 css 번들링도 도와준다. 이 말은 즉 css도 import/export를 통해 참조가 가능하다는 뜻이고, 이는 컴포넌트 단위로 css 코드 역시 분리가 가능하게 된다는 것이다. 그렇기 때문에 Webpack의 js가 아닌 나머지 파일의 번들링은 아주 강력한 SPA의 도구가 된다.

### import/export

- 자바스크립트는 html의 script 태그를 통해 외부 스크립트 파일을 가져옴.
- 파일마다 독립적인 파일 스코프를 갖지 않고 전역 객체를 공유함.
- 파일 별 함수/변수 명이 겹칠 경우 전역 환경을 오염시킬 수 있음.

### IIFE

- 즉시 실행 함수를 통해 스코프를 분리하려고 시도함.
- 하지만, 즉시 실행해버리기 때문에 모듈화의 해결책은 아님.

### CommonJS

- require로 의존성 관리
- 필요한 모듈을 모두 내려받을 때까지 아무것도 할 수 없음.
- 치명적인 단점.

### ES6 Module

- export/import 를 통해 의존성 관리.
- script 태그에 type="module" 속성을 추가하면 사용가능하다.
- 하지만, 모든 브라우저에서 지원하지 않는다.

⇒ webpack을 통한 번들링 필요

### Webpack devserver

개발 시에 임시로 서버를 열어서 결과물을 확인하면서 개발할 수 있게 해주는 도구이다. 결국 웹 서비스는 유저가 브라우저에 접속하면, 브라우저가 웹 서버에 요청을 보내어 웹 서버에 존재하는 html, css, js를 응답받아 브라우저의 파싱에 의해 보여지는 것이기 때문에, 실제로 devserver 없이 화면이 잘 구성되었는지 확인하기 위해선, node.js, express, spring 등과 같은 html 파일을 제공해주는 웹 서버를 열어서 테스트 해야한다. 이러한 수고를 webpack devserver가 대신 해줄 수 있다.

- 웹 서버가 열려있을 때
  ![](https://images.velog.io/images/vgihan/post/b4157812-e4cf-4899-8b30-75cfc6aacdd8/image.png)

- 웹 서버가 없으면 ..?
  ![](https://images.velog.io/images/vgihan/post/0273c1d4-255a-445f-bf03-b988bcc187ee/image.png)

### Webpack config

**entry**

- 최초 진입점. entry point 부터 의존된 파일들을 번들링하기 시작함.

**output**

- 번들링 결과물을 위치할 경로
- [name].js를 이용하여 entry point와 같은 이름의 output 을 만들 수 있다.

**Loader**

- webpack이 웹 애플리케이션을 해석할 때 자바스크립트 파일이 아닌 웹 자원 (HTML, CSS, Images, font)들을 변환할 수 있도록 도와주는 속성.

**Plugin**

- 해석, 변환이 끝난 결과물의 형태를 바꾸는 역할.

**실제 예시**

```javascript
const path = require("path");

module.exports = {
  entry: {
    main: "./frontend/public/js/components/app.js",
  },
  output: {
    path: path.resolve(__dirname, "frontend/assets"),
    filename: "js/index.js",
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: ["babel-loader"],
      },
      {
        test: /\.s[ac]ss$/i,
        exclude: /node_modules/,
        use: [
          "style-loader",
          {
            loader: "css-loader",
            options: {
              esModule: false,
            },
          },
          "sass-loader",
        ],
      },
      {
        test: /\.(svg|gif|png|otf)$/,
        loader: "file-loader",
        options: {
          publicPath: "./assets/",
          name: "img/[name].[ext]?[hash]",
        },
      },
    ],
  },
  devtool: "inline-source-map",
  mode: "development",
};
```

### 리액트는 사실 webpack을 갖고있다

리액트의 개발, 배포 단계를 살펴보면 모든 동작이 webpack을 통해 이루어지는 것을 알 수 있다. 우리가 열심히 로직을 작성하고 yarn start 나 npm start를 통해서 화면을 확인할 것이다. 이는 webpack devserver를 실행시켜 번들링 한 파일을 devserver 위에서 작동시키게 한다음 보여지게 되는 것이다.

![](https://images.velog.io/images/vgihan/post/f45b51eb-255c-48b6-8a2c-b71af53a3fa4/image.png)

또한, 개발을 마치고 실제 서비스에 배포가 진행될 때를 생각해보자. 우리는 클라우드 서버를 빌리거나, 직접 소유한 서버에 배포를 진행하게 될 것이다. 웹 서비스를 제공하기 위한 express, spring과 같은 웹 서버가 해당 서버에서 동작할 것이고, SPA의 특성상 API 요청에 대한 경로가 아닌 모든 요청에 대한 응답으로는 index.html과 js, css 파일일 것이다. 그렇기 때문에 리액트로 진행한 개발에 대한 결과물로써 빌드를 하게 되면, webpack이 우리가 작성한 코드들을 번들링하여 해당 웹 서버가 작동하는 폴더 안에 넣어주기만 하면 실제 서비스가 이루어진다.

정리하자면, 리액트를 이용해서 열심히 개발한 이후에 배포할 때 사용하는 yarn run build라는 명령이 사실 webpack의 번들링이라는 이야기다

- 열심히 리액트로 개발해서 웹 서버에 끼워넣는다.
  ![](https://images.velog.io/images/vgihan/post/fb6f99bf-83dd-4245-8135-bca86b66739e/image.png)
