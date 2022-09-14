## REACT学习记录
### API
+ reactdom.createportal
  + 将子节点渲染到存在于父组件以外的 DOM 节点
  + 可获得上下文context
  + 仅能在组件中使用

+ Element.scrollIntoView
  + 滚动元素的父容器，使被调用 scrollIntoView() 的元素对用户可见

+ forwardRef
  + 获取react组件的dom
  ```javascript
  import { forwardRef, useRef } from 'react';
  const MyInput = forwardRef((props, ref) => {
    return <input {...props} ref={ref} />;
  });

  export default function Form() {
    const inputRef = useRef(null);

    function handleClick() {
      inputRef.current.focus();
    }

    return (
      <>
        <MyInput ref={inputRef} />
        <button onClick={handleClick}>
          Focus the input
        </button>
      </>
    );
  }

  ```

+ useImperativeHandle
  + 限制用ref获取的dom组件暴露的属性跟方法
  ```javascript
    import {
    forwardRef, 
    useRef, 
    useImperativeHandle
  } from 'react';
  const MyInput = forwardRef((props, ref) => {
    const realInputRef = useRef(null);
    useImperativeHandle(ref, () => ({
      // Only expose focus and nothing else
      focus() {
        realInputRef.current.focus();
      },
    }));
    return <input {...props} ref={realInputRef} />;
  });

  export default function Form() {
    const inputRef = useRef(null);

    function handleClick() {
      inputRef.current.focus();
    }

    return (
      <>
        <MyInput ref={inputRef} />
        <button onClick={handleClick}>
          Focus the input
        </button>
      </>
    );
  }
  ```
+ flushSync
  + 更新state之后同步渲染dom
  ```javascript
  import { useState, useRef } from 'react';
  import { flushSync } from 'react-dom';

  export default function TodoList() {
    const listRef = useRef(null);
    const [text, setText] = useState('');
    const [todos, setTodos] = useState(
      initialTodos
    );

    function handleAdd() {
      const newTodo = { id: nextId++, text: text };
      flushSync(() => {
        setText('');
        setTodos([ ...todos, newTodo]);      
      });
      listRef.current.lastChild.scrollIntoView({
        behavior: 'smooth',
        block: 'nearest'
      });
    }

    return (
      <>
        <button onClick={handleAdd}>
          Add
        </button>
        <input
          value={text}
          onChange={e => setText(e.target.value)}
        />
        <ul ref={listRef}>
          {todos.map(todo => (
            <li key={todo.id}>{todo.text}</li>
          ))}
        </ul>
      </>
    );
  }

  let nextId = 0;
  let initialTodos = [];
  for (let i = 0; i < 20; i++) {
    initialTodos.push({
      id: nextId++,
      text: 'Todo #' + (i + 1)
    });
  }

  ```
### 常用技巧
+ useRef的作用
  + 获得定时器的id
  + 不引起re-render的变量应该用ref保存

+ 用ref回调获取dom
```javascript
import { useRef } from 'react';

export default function CatFriends() {
  const itemsRef = useRef(null);

  function scrollToId(itemId) {
    const map = getMap();
    const node = map.get(itemId);
    node.scrollIntoView({
      behavior: 'smooth',
      block: 'nearest',
      inline: 'center'
    });
  }

  function getMap() {
    if (!itemsRef.current) {
      // Initialize the Map on first usage.
      itemsRef.current = new Map();
    }
    return itemsRef.current;
  }

  return (
    <>
      <nav>
        <button onClick={() => scrollToId(0)}>
          Tom
        </button>
        <button onClick={() => scrollToId(5)}>
          Maru
        </button>
        <button onClick={() => scrollToId(9)}>
          Jellylorum
        </button>
      </nav>
      <div>
        <ul>
          {catList.map(cat => (
            <li
              key={cat.id}
              ref={(node) => {
                const map = getMap();
                if (node) {
                  map.set(cat.id, node);
                } else {
                  map.delete(cat.id);
                }
              }}
            >
              <img
                src={cat.imageUrl}
                alt={'Cat #' + cat.id}
              />
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}

const catList = [];
for (let i = 0; i < 10; i++) {
  catList.push({
    id: i,
    imageUrl: 'https://placekitten.com/250/200?image=' + i
  });
}
```

+ custom-hooks
  ```javascript
  function useOnlineStatus() {
    const [isOnline, setIsOnline] = useState(true);
    useEffect(() => {
      function handleOnline() {
        setIsOnline(true);
      }
      function handleOffline() {
        setIsOnline(false);
      }
      window.addEventListener('online', handleOnline);
      window.addEventListener('offline', handleOffline);
      return () => {
        window.removeEventListener('online', handleOnline);
        window.removeEventListener('offline', handleOffline);      
      };
    }, []);
    return isOnline;
  }
  ```