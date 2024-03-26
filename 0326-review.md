# :fire: 03/26 배운 내용 정리

## Node.js에서의 라우팅

요청이 날아왔을 때, 원하는 경로에 따라 적절한 방향으로 경로를 안내해주는 것.


```
//blog 사용자 관리 루트 파일

const express = require("express");
const router = express.Router();

router.use(express.json());

//app을 router로 수정해준다.
```


```
// 다른 파일로 분리된 루트를 모듈로 만들어 한 파일 안에 소환시켜준다. 
const express = require('express');
const app = express();
app.listen(8888);

const bloggerRouter = require('./bloggers');
const blogRouter = require('./blogs');

// ./blogger 파일 밑에, 모듈화하는 명령문인 module.exports를 작성해준다.

app.use("/",bloggerRouter);
app.use("/blogs",blogRouter);
```