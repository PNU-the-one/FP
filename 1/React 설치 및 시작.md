# React 설치 및 시작

준비물

- Node.js

- npm (or yarn)

- create-react-app





## Node.js

[Node.js](https://nodejs.org/ko/)

위 링크를 통해서 LTS(Long Term Support)버전을 다운받으면 된다. 



   

## NPM (Node Package Manager)

Node.js 설치시 자동으로 설치 될것이다. 



NPM은 node.js에서 사용되는 모듈들을 패키지로 만들어 npm을 통하여 관리하고 배포한다.

> 파이썬 pip와 비슷하다.



> Yarn은 조금 더 좋은(?) 패키지 매니저라고 생각하면 된다.

## create-react-app

```bash
$ npx create-react-app [프로젝트명]
$ cd [프로젝트명]
$ npm start (yarn start)
```

위 명령어를 통해서 서버를 열고

http://localhost:3000

를 통해서 내가 작성한 App을 확인 할 수 있다.

> 포트번호는 설정에 따라 변경될 수 있다.





#### npx란...?

처음은 npm이라고 했는데 npx가 갑자기 나온이유는 npm을 좀 더 편하게 사용하기 위한 도구이다. npm으로 설치한 라이브러리들은 전역으로 설치되던가 특정 프로젝트에만 설치되던가 하는데, 이런 라이브러리들이 업데이트가 되면 두 가지를 따로 업데이트 해줘야한다. 굉장히 번거롭다. 가령 우리가 npm install 을 사용하여 create-react-app을 설치했다고 치자 한달뒤에 create-react-app을 사용하려면 업데이트를 해줘야한다. 근데 업데이트가 되었는지 확인부터 해야한다. npx는 이런 귀찮은 과정을 생략해준다. 다운로드가 아닌 최신버전을 잠깐 가져다 쓰는거기 때문에.



> 물론 npm install로 설치하는게 좋은 것들도 있을거다.




