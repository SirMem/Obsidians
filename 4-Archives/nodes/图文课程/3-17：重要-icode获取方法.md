# icode 获取方法

如果希望调用课程的接口数据，那么需要配置 `icode` 。本图文节就是告诉大家如何获取并配置 `icode`

## 获取 `icode`

获取 `icode` 的步骤如下：

1. 打开 [课程列表](https://coding.imooc.com/learn/list/542.html)
2. 在右侧可查看到 **获取校验码** 按钮
   ![image-20220111154644345](3-17：重要-icode获取方法.assets/image-20220111154644345.png)
3. 点击该按钮即可获取 **校验码**。（注意：校验码有效期为 **30天** ，过期之后需要重新获取）

## 配置 icode

`icode` 需要被配置到每次请求的 **请求头** 中，以 `icode` 为属性名。

具体配置方案如下：

1. 打开项目的 `axios` 配置文件（`src/utils/request.js`）

2. 为 `service` 添加请求拦截器，并在请求拦截器中配置 `icode`

   ```js
   // 请求拦截器
   service.interceptors.request.use(
     config => {
       // 添加 icode
       config.headers.icode = '你的 icode '
       // 必须返回 config
       return config 
     }
   )
   ```




至此，`icode` 配置完成。