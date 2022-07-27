# Study-Sharing
## node_modules
CRA를 구성하는 모든 패키지 소스 코드가 존재하는 폴더
## package.json
CRA 기본 패키지 외 추가로 설치된 라이브러리/패키지 정보(종류, 버전)가 기록되는 파일
모든 프로젝트마다 package.json이 하나씩 존재한다.

"dependencies"
리액트를 사용하기 위한 모든 패키지 리스트, 버전 확인이 가능.
실제 코드는 node.modules 폴더에 존재한다.

"scripts"
start : 프로젝트 development mode(개발 모드) 실행을 위한 명령어. npm run start
build : 프로젝트 production mode(배포 모드) 실행을 위한 명령어. 서비스 상용화.

❓❓ 근데 왜 node.modules과 package.json에서 이중으로 패키지를 관리할까?
- 실제 내가 작성한 코드, 내가 설치한 패키지는 내 로컬에만 존재.
- github에 올릴 때 내가 작성한 코드와 함께 pacakge.json(추가로 설치한 패키지)를 넘긴다.
- 다른 사람이 그것을 (pull) 받아서 npm install만 입력하면 package.json에 기록되어 있는 패키지의 이름과 버전 정보를 확인하여 자동으로 설치한다.
- 이 때, github에 올릴 때, node.modules는 올리면 안되는데 (불필요한 용량 차지)
- .gitignore 파일에 github에 올리고 싶지 않은 폴더와 파일을 작성할 수 있다. 