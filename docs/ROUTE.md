# 系统路由
系统路由使用 [react-router](https://reacttraining.com/react-router/web/guides/quick-start)。
路由使用最多的场景就是一个path对应一个页面，但是开发人员不得不去编写配置文件，多人协作时，配置文件还经常冲突。
系统通过脚本，简化路由配置。

## 路由配置方式
通过脚本，自动抓取，生成`/src/pages/page-routes.js`文件，支持两种写法：

1. 常量方式
    ```js
    export const PAGE_ROUTE = '/path';
    ```
1. 页面config装饰器
    ```js
    @config({
        path: '/path',
    })
    export default class SomePage extends React.Component {
        ...
    }
    ```

比如SomePage.jsx中有上面其中任意一种写法，最终会在`/src/pages/page-routes.js`文件中生成如下路由配置：
```js
{
    path: '/path',
    component: () => import('/path/to/SomePage.jsx'),
},
```

## 二级页面
    
二级页面如果要保持父级菜单的选中状态，以父级path开始并以`/_/`作为分隔符即可：`parent/path/_/child/path`

```js
// parent page 
@config({
    path: '/parent/path'
})
export default class Parent extends React.Component {
    ...
}

// child page
@config({
    path: '/parent/path/_/child/path'
})
export default class Parent extends React.Component {
    ...
}
```

## 域名子目录发布项目
如果项目需要挂载到域名的一个子目录下，比如 `http://xxx.com/react-admin`，
可以修改`src/router/AppRouter.jsx`中的`ROUTE_BASE_NAME`常量。
生产构建时添加PUBLIC_URL参数（开发时不需要）。

项目在子目录react-admin下：
1. 修改常量
    ```js
    export const ROUTE_BASE_NAME = '/react-admin';
    ```
1. 生产构建添加参数
    ```bash
    PUBLIC_URL=/react-admin yarn build
    ```

项目直接挂载域名根目录：(无需PUBLIC_URL参数)
```js
export const ROUTE_BASE_NAME = '';
```

