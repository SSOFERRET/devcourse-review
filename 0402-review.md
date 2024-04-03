# :fire: 04/02 배운 내용 정리

## :one: 유효성 검사

validation 

"사용자가 입력한 값" 유효성(=타당성)을 확인하는 것.

외부 모듈 express-validator

```
const {body, param, validationResult} = require('express-validator');
// express-validator에 존재하는 변수들

//함수 파라메터 앞에 유효성 검사 메소드 입력 가능 
.post(
    [
        body('userId').notEmpty().isInt().withMessage('숫자 입력하자'),
        body('name').notEmpty().isString().withMessage('문자 입력하자')
    ],
    (req, res) => {
        const err = validationResult(req);

        if (!err.isEmpty()) return res.status(400).json(err.array());
    //...
    // 유효성 검사 위해 사용했던 if 문 삭제 가능.
    conn.query(sql, values, (err, results) => {
        if (err) return res.status(400).end();
        res.status(201).json(results);
    });
})

//...

.get(
    param('id').notEmpty().withMessage('채널id 필요'),
    //...
)

```

## :two: 파일 내부에 미들웨어 만들기

function() 형태의 함수가 아니라 화살표 함수 사용하면 미들웨어로 사용할 수 있다!
