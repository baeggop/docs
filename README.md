# 설치법
1. 적당한 장소에 clone한다.  
   👉&nbsp;&nbsp;전체경로에 한글이 있으면 안된다.
```
git clone https://github.com/baeggop/backend.git
```
2. 라이브러리를 다운받는다.
```
npm install
```
3. 프로젝트 루트에서(/path/to/project/<b>backend</b>) 서버를 실행한다.
```
$ npm start
```
4. DB migration prisma/schema.prisma에 schema를 고치고 다음 명령어 실행한다.
```
npm run migrate --comment "init"

// npm run migrate --comment "migration comment"
```