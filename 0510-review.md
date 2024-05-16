# :fire: 05/10 학습 내용

## :one: 클래스형 컴포넌트와 함수형 컴포넌트

### :memo: 클래스형 컴포넌트
```javascript
import {Component} from "react";

class ClassCom extends Component{ // 반드시 Component 클래스로부터 상속을 받아야한다.
    render(){
        return(
            <div>
                클래스형 컴포넌트
            </div>
        )
    }
}

export default ClassCom;
```

### :memo: 함수형 컴포넌트
```javascript
function FuncCom1() {
    return (
        <div>
            함수형 컴포넌트
        </div>
    )
}

const FuncCom2 = () => (
    <div>
        함수형 컴포넌트2
    </div>
)

export default FuncCom1;
```

함수형 컴포넌트가 더 권장되고 있다.

---

## :two: state

- React에서는 일반 변수 대신 state를 사용하기도 한다.
- useState()함수로써 state를 사용할 수 있다.
```javascript
const TodoList : React.FC= () => { // Function Component의 약자.
    const title : string = "오늘 할 일";
    const [todos] = useState(['잠자기', '공부하기', '숨쉬기']); 
    return (
        <div>
            <h1>{title}</h1>
            <ul>
                <li>{todos[0]}</li>
                <li>{todos[1]}</li>
                <li>{todos[2]}</li>
            </ul>
        </div>
    )
}
```

- state 변수의 변경은 별도의 변경 함수를 사용해야한다. state 변수 선언 시에 호출할 수 있다.
```javascript
const [todos, setTodos] = useState(['잠자기', '공부하기', '숨쉬기']);

// setTodos가 setter 함수다.
// setTodos 함수를 따로 선언해줘야한다.
```

---

## :three: 데이터 반복 처리하기 + :four: 체크박스 기능 추가

- 고유한 데이터 타입 생성 후, map()함수를 이용하여 반복 처리를 한다.
```javascript
import React, { useState } from 'react';

type Todo = {
    id : number;
    text : string;
    isChecked : boolean;
}

const TodoList : React.FC= () => { // Function Component의 약자.
    const title : string = "오늘 할 일";
    const [todos, setTodos] = useState<Todo[]>([
        {id: 1, text: '공부하기', isChecked: false},
        {id: 2, text: '잠자기', isChecked: false},
        {id: 3, text: '미팅하기', isChecked: false}
    ]);
    // 체크박스 이벤트 함수
    const handleCheckedChange = (itemId : number) => {
        setTodos((prevItems) => 
            prevItems.map((item) => 
                item.id === itemId ? {...item, isChecked : !item.isChecked} : item
            )
        )
    }
    return (
        <div>
            <h1>{title}</h1>
            <div className='board'>
                <ul>
                    {
                        todos.map(todo => (
                            <li key={todo.id}>
                                <input type = 'checkbox'
                                // 체크박스에 이벤트를 걸어준다.
                                onChange={() => {
                                    handleCheckedChange(todo.id);
                                }}/>
                                <span>
                                    {
                                        todo.isChecked ? 
                                        <del>{todo.text}</del>
                                        : <span>{todo.text}</span>
                                    }
                                </span>
                                
                            </li>
                        ))
                    }
                </ul>
            </div>
            
        </div>
    )
}

export default TodoList;
```
