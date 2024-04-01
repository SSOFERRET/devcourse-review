# :fire: 04/01 배운 내용 정리

## :one: mysql createConnection을 모듈화 하기

```
// mariadb.js

const mysql = require('mysql2');

const connection = mysql.createConnection({
    host: 'localhost',
    user: -,
    password: -,
    database: 'Blog',
    dateStrings : true
})

module.exports = connection;
```

```
// blogger.js

const express = require("express");
const router = express.Router();
const conn = require("./mariadb");
router.use(express.json());
```
route 파일에 mariadb 모듈 파일을 연결해주었다.

---

## :two: 회원 정보 조회
```
router
    .route('/bloggers')
    .get((req, res) => {
        const {email} = req.body;
        conn.query(
            `SELECT * FROM Blog.users where email=?`, email,
            (err, results) => {
                if (results.length) res.status(200).json(results);
                else res.status(404).json({
                    message : "회원 정보가 없습니다."
                });
            }
        )
    })
```

body로 email 정보를 받아 db로 select query를 날려주었다.

---

## :three: 회원 정보 삭제
```
router
    .route('/bloggers')
    .delete((req, res) => {
        const {email} = req.body;

        conn.query(
            `delete FROM Blog.users where email=?`, email,
            (err, results) => {
                const {_, affectedRows} = results;
                if (affectedRows === 1) res.status(200).json({
                    message : "안녕히 가십시오."
                });
                else res.status(400).json({
                    message : "가입된 회원이 아닙니다."
                });
            }
        )
    })
```

body로 email 정보를 받아 db로 delete query를 날려주면, 다음과 같이 status json 정보가 날아온다.

```
{
    "fieldCount": 0,
    "affectedRows": 1,
    "insertId": 0,
    "info": "",
    "serverStatus": 2,
    "warningStatus": 0,
    "changedRows": 0
}
```

이 중에서 affectedRows가 delete query에 대한 결과값이다.
affectedRows가 1일 때 회원 계정 삭제가 잘 실행되었음을, 
0일 때에는 회원 정보 불일치 등의 문제로 실행이 안되었음을 뜻한다.

---

## :four: 회원 가입

```
router
    .route('/join')
    .post((req, res) => {
        const {email, name, password, contact} = req.body;

        conn.query(
            `SELECT * FROM Blog.users where email=?`, email,
            (err, results) => {
                if (!results.length) {
                    conn.query(
                        `INSERT into Blog.users (email, name, password, contact) 
                        VALUES (?,?,?,?)`, [email, name, password, contact]
                    )
                    res.status(201).json({
                        message : "회원가입이 완료되었습니다."
                    })
                }
                else res.status(404).json({
                    message : "중복된 이메일이 있습니다. 다른 이메일 주소를 입력해주시오."
                });
            }
        )  
    });
```
body로 가입자 정보를 받아, 일단 먼저 db에 중복되는 이메일 정보가 있는지 확인하기 위해 select query를 보냈다.
없음을 확인한 후, insert query를 전송하여 회원가입을 했다. insert query에 대한 결과값을 따로 보내주진 않지만 workbench로 확인하면 테이블에 추가되어있음을 확인할 수 있다. 

---

## :five: 로그인

```
router
    .route('/login')
    .post((req, res) => {
        const {email, password} = req.body;
        let loginUser;

        conn.query(
            `SELECT * FROM Blog.users where email=?`, email,
            (err, results) => {
                loginUser = results[0];
                if (loginUser) {
                    if (password === loginUser.password) res.status(200).json({
                        message : "로그인 되었습니다."
                    });
                    else res.status(400).json({
                            message : "잘못된 이메일 또는 비밀번호입니다."
                        });
                }
                else res.status(404).json({
                    message : "회원정보가 없습니다."
                });
            }
        )
    })
```

body로 이메일과 비밀번호 데이터를 받은 후, select query를 전송하여 해당 이메일에 대한 정보값을 얻는다. db 상의 비밀번호와 body에 기입된 비밀번호가 일치하는지 확인한 후 결과 메시지를 전송해준다.
