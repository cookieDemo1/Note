###### 1.Hook使用state

```jsx
import React from 'react'

export default function Hook1(props){
  // 声明num, 操作的方法为setNum, 初始值为0
  const [num, setNum] = React.useState(0)

  return(
    <div>
      <p>you click {num} click</p>
      <button onClick={()=>setNum(num+1)}>点击</button>
    </div>
  )
}
```

###### 2useEffct

```jsx
import React, { useEffect } from 'react'
export default function Hook1(props){
  const [num, setNum] = React.useState(0)
  
  // useEffect将会在React完成对DOM的更改后执行,并且在每次num改变之后会执行
  useEffect(()=>{
    document.title = `you click ${num}次`
  })

  return(
    <div>
      <p>you click {num} click</p>
      <button onClick={()=>setNum(num+1)}>点击</button>
    </div>
  )
}
```

###### 3.hook组合的方式

```jsx
import React from 'react'

const ChildHook = {
  Child1: (props)=>{
    return(
      <h2>Child1</h2>
    )
  },

  Child2: (props)=>{
    return(
      <h2>Child2</h2>
    )
  }
}

export default function Hook4(props){
  return(
    <div>
      <ChildHook.Child1/>
      <ChildHook.Child2/>
    </div>
  )
}
```

###### 属性为true或者false时，只写属性名，默认为true

```jsx
// 以下两个为等价的
<route exact />
<route exact={true}/>
```

