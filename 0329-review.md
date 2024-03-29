# :fire: 03/29 배운 내용 정리

## :one: GUI로 데이터베이스 접근하기! (workbench)

MySQL workbench를 사용해보았다.

#### [Connect to Database] setting 요소 

- Connection Method
    - TCP/IP
    - Local Socket/Pipe
    - standard TCP/IP over SSH

- Hostname: 데이터베이스를 갖고 있는 주소값. localhost로 작성하면 된다.

#### [Create table] setting 요소

- asc : ascending ↔ descending
- zerofill : 자릿수가 int(num)에서 num보다 작을 때, 자릿수를 0으로 채운다.

---

## :two: DB 연동

npm으로 mysql2를 다운로드 받는다.

```
npm install --save mysql2
```

서버 측에 전달할 로그인 정보와 쿼리를 json으로 작성하여 실행하면, 테이블의 내용의 json 배열의 형태로 날아온다.
```
const mysql = require('mysql2');

const connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'Blog'
})

connection.query(
    'select * from `users`',
    (err, results, fields) => {
        console.log(results);
        console.log(fields);
    }
)

/**
[ //results
  {
    id: 1,
    email: 'park@email.com',
    name: '박철수',
    password: '1111',
    contact: '010-0000-0000'
  },
  {
    id: 2,
    email: 'lee@email.com',
    name: '이영희',
    password: 'aaaa',
    contact: '010-1111-1111'
  },
  {
    id: 3,
    email: 'gwak@email.com',
    name: '곽곽철',
    password: 'ssss',
    contact: '010-2222-2222'
  }
]
[ // fields
  `id` INT NOT NULL PRIMARY KEY UNIQUE_KEY AUTO_INCREMENT,
  `email` VARCHAR(100) NOT NULL UNIQUE_KEY,
  `name` VARCHAR(45) NOT NULL,
  `password` VARCHAR(20) NOT NULL,
  `contact` VARCHAR(45)
]
 */
```

## :three: timestamp 칼럼 추가하기

datatype의 콤보박스에서 선택할 수 있다. 선택하면 timestamp()라고 괄호가 있는데, 시간의 소수점 몇자리까지 표기하는지를 입력하는 란이다. 소수점 아래 내용이 필요없으면 괄호를 지우면 된다.

{ 
    column name : created_at,
    datatype : timestamp,
    default/expression : current_timestamp()
}

---

## :four: timezone 설정

데이터베이스의 기본 시간 설정값은, 세계표준시인 협정세계시(UTC)로 되어있다.
시간 설정을 새로이 입력하는 법은 다음과 같다.

```
set global time_zone = 'Asia/Seoul'
```

아래의 코드로 설정 변경 여부를 확인할 수 있다.

```
select @@global.time_zone, @@session.time_zone;
```

여기까지만해도 workbench는 설정값대로 시간을 출력하지만, express로 날아오는 데이터는 그렇지 못하다.
이 부분은 connection 설정에 'dateStrings : true'를 추가함으로써 해결할 수 있다. 

```
const connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'Blog',
    dateStrings : true
})
```