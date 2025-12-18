### React
##### 路由

```javascript
安装
npm i react-router-dom --save

案例
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom'
import Home from './pages/home'
import Demo from './pages/demo'

function App() {

  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/home">Home</Link>
            </li>
            <li>
              <Link to="/demo">Demo</Link>
            </li>
          </ul>
        </nav>
        <Routes>
          <Route path="/home" element={<Home />} />
          <Route path="/demo" element={<Demo />} />
        </Routes>
      </div>
    </Router>
  )
}

export default App
```



#### 复习
```jsx
1.babel将jsx代码翻译为js代码 
// 1.创建一个虚拟DOM
let vDOM = <h2>hello,react</h2> // jsx语法不需要加引号
// console.log(vDOM,typeof vDOM)

// 2.渲染虚拟DOM到页面
ReactDOM.render(vDOM, document.getElementById('test'))

2.虚拟DOM的两种创建方式
    2.1 方式一： 使用原生js创建虚拟DOM
    let vDOM = React.createElement('h2', { id: myID }, React.createElement('span', {}, myData)) 
    // React.createElement(标签名, 属性, 内容)
    ReactDOM.render(vDOM, document.getElementById('test1'))

    2.2 方式二： 使用jsx语法创建虚拟DOM
    let vDOM2 =
    <h2 id={myID.toUpperCase()}>
    <span className="red">
      {myData.toUpperCase()}
    </span>
    </h2>
    ReactDOM.render(vDOM2, document.getElementById('test2'))

3.JSX练习
    // 定义一个数组，存放数据，存放的是纯数据
    let arr = ['A', 'B', 'C', 'D']
    // let arr2 = [<li>A</li>, <li>B</li>, <li>C</li>, <li>D</li>]

    // 1.创建虚拟DOM
    let vDOM = (
      <div>
        <h2>前端框架</h2>
        { /* 将数组中的数据进行加工 */}
        {
          /* 暂时使用index作为key，但这种方式会出现某些问题 */
          arr.map((item, index) => {
            return <li key={index}>{item}</li>
          })
        }
      </div>
    )

    // 2.渲染虚拟DOM到页面
    ReactDOM.render(vDOM, document.getElementById('example1'))

4.2种创建组件方式 (定义一个组件(组件名大写))
 4.1 方式1: 工厂函数定义组件(适用简单组件/无状态)
 
http://127.0.0.1:5500/18-React/01-React%E5%9F%BA%E7%A1%80/07-%E7%BB%84%E4%BB%B6%E7%9A%84%E7%BB%84%E5%90%88%E4%BD%BF%E7%94%A8/01-%E7%BB%84%E4%BB%B6%E7%BB%84%E5%90%88%E4%BD%BF%E7%94%A8.html
```



#### 教程

##### 2025年最新React全精通教程，5小时吃透react
https://www.bilibili.com/video/BV11uK3zqEP8?spm_id_from=333.788.player.switch&vd_source=611c52b27c89f48d96d94e3dac50a33d&p=19

https://github.com/TaoLoading/01-WebStudy/tree/master/18-React

##### React Router V6 中 useRoutes() 报错的解决方案 https://dev-solve.com/zh/posts/3e651dc

##### Create React App https://create-react-app.dev/docs/documentation-intro

##### 菜鸟教程 https://www.runoob.com/react/react-lists-and-keys.html

##### 40分钟学会React18组件通信与插槽
https://www.bilibili.com/video/BV1xM41197cZ/?spm_id_from=333.1387.collection.video_card.click&vd_source=611c52b27c89f48d96d94e3dac50a33d

##### 前端React面试精讲

 https://www.bilibili.com/video/BV1AHUbBrETg/?spm_id_from=333.1387.homepage.video_card.click&vd_source=611c52b27c89f48d96d94e3dac50a33d1

##### React实战

https://www.bilibili.com/video/BV1tebmziEbF?spm_id_from=333.788.player.switch&vd_source=611c52b27c89f48d96d94e3dac50a33d&p=2