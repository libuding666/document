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

#### 教程

##### 2025年最新React全精通教程，5小时吃透react
https://www.bilibili.com/video/BV11uK3zqEP8?spm_id_from=333.788.player.switch&vd_source=611c52b27c89f48d96d94e3dac50a33d&p=19

##### React Router V6 中 useRoutes() 报错的解决方案https://dev-solve.com/zh/posts/3e651dc

##### Create React App https://create-react-app.dev/docs/documentation-intro

##### 菜鸟教程 https://www.runoob.com/react/react-lists-and-keys.html

##### 40分钟学会React18组件通信与插槽
https://www.bilibili.com/video/BV1xM41197cZ/?spm_id_from=333.1387.collection.video_card.click&vd_source=611c52b27c89f48d96d94e3dac50a33d

```
https://wiki.mcy.im/#/zh-cn/started/cli-install
https://www.jutuike.com/doc/48?act_id=80
https://cargo.wboat.cn/admin/#/login
https://business.mhdyp.com/#/business
```