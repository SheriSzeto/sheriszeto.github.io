---
title: 【GDMG】管理后台开发使用手册
urlname: vrapup
date: '2021-07-30 09:53:16 +0800'
tags: []
categories: []
---

# ant desgn react

# 一、基础使用

## 1. 文件夹结构介绍

```javascript
├── config                   # umi 配置，包含路由，构建等配置
├── mock                     # 本地模拟数据
├── public
│   └── favicon.png          # Favicon
├── src
│   ├── assets               # 本地静态资源
│   ├── e2e                  # 集成测试用例
│   ├── layouts              # 通用布局
│   ├── models               # 全局 dva model
│   ├── pages                # 业务页面入口
│   ├── services             # 后台接口服务
│   ├── shared/etc_bfplus                # 公共组件及工具库
│   ├── locales              # 国际化资源
│   ├── global.less          # 全局样式
│   └── global.tsx            # 全局页面
├── tests                    # 测试工具
├── README.md
└── package.json
```

## 2. 项目启动

在 terminal 执行命令（默认开启 mock）

```bash
npm run start
```

本地访问地址为[http://localhost:8005/user/login](http://localhost:8005/user/login)

如不需要 mock，执行命令

```bash
npm run start:no-mock
```

本地启动的端口默认是 8005，如果需要修改的话，更改.env 文件

## 3. 本地调试（proxy 的反向代理）

- 打开 config/config.ts，对应修改不同的接口服务所代理的服务器域名，如修改‘/api’前缀的接口服务为自己本地 IP，直接修改 target 的值

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21382958/1627610322577-099bd66e-c3f0-4bf1-bc55-f5af7f8f8695.png#align=left&display=inline&height=324&margin=%5Bobject%20Object%5D&name=image.png&originHeight=648&originWidth=585&size=67583&status=done&style=none&width=292.5)

## 4. 页面代码结构推荐（请遵循此守则开发）

```javascript
src
├── components
└── pages
├── Welcome        // 路由组件下不应该再包含其他路由组件，基于这个约定就能清楚的区分路由组件和非路由组件了
|   ├── components // 对于复杂的页面可以再自己做更深层次的组织，但建议不要超过三层
|   ├── index.tsx  // 页面组件的代码
|   └── index.less // 页面样式
├── Order          // 路由组件下不应该再包含其他路由组件，基于这个约定就能清楚的区分路由组件和非路由组件了
|   ├── index.tsx
|   └── index.less
├── user           // 一系列页面推荐通过小写的单一字母做 group 目录
|   ├── components // group 下公用的组件集合
|   ├── Login      // group 下的页面 Login
|   ├── Register   // group 下的页面 Register
|   └── util.ts    // 这里可以有一些共用方法之类，不做推荐和约束，看业务场景自行做组织
└── *              // 其它页面组件代码
```

# 二、页面开发

## 1. 快速创建 CRUD 页面

1. 创建路由组件
   1. 要生成模块，模块下包含多个路由组件，在 terminal 执行命令 npm run temp 目录（强烈要求这种方式）

```bash
eg:
npm run temp test/Table
```

会在 src/pages 目录下创建 test 文件夹，内包含 Table 文件夹，文件夹里面包含 index.tsx 和 d.ts 两个文件。
b. 要生成单路由组件，在 terminal 执行命令 npm run temp 组件名

```bash
eg:
npm run temp Order
```

会在 src/pages 目录下创建 Order 文件夹，内包含 index.tsx 以及 d.ts 两个文件，d.ts 文件是该模块的 typescript 描述文件，用来指定对象的类型。

2. 将文件加入菜单和路由

a. 在 config/routes 文件夹内创建一个有关该模块的所有路由文件，命名规则是模块名.routes.ts，比如售后模块，after-sale.routes.ts，像上面我们新增的 test 模块，我们就创建一个 test.routes.ts 文件。

```javascript
// test.routes.ts
export default {
  path: "/test", // 表示模块，创建一个新模块的时候一定要名字匹配
  component: "../layouts/BasicLayout", // 默认布局
  routes: [
    // 每新增一个路由组件，就在这里定义一个路由对象
    {
      path: "/test/table", // 访问路由的路径
      name: "表格", // 显示在菜单上的名字
      component: "./test/Table/index", // 组件所在文件夹
    },
  ],
};
```

b. 创建完路由文件之后，要在 config/routes.js 文件中引入

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21382958/1627610882195-117c240a-559c-4c01-9af5-9e76c9bfc911.png#align=left&display=inline&height=133&margin=%5Bobject%20Object%5D&name=image.png&originHeight=266&originWidth=944&size=75704&status=done&style=none&width=472)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/21382958/1627610889744-a9e7ed71-0fb2-4ab1-96dc-7406ded23494.png#align=left&display=inline&height=323&margin=%5Bobject%20Object%5D&name=image.png&originHeight=645&originWidth=611&size=60429&status=done&style=none&width=305.5)

```javascript
// 引入文件，再把文件输出
import testRoutes from "./routes/test.routes";
```

3. 定义接口请求文件
   在 services 文件夹创建一个该模块的接口请求文件，如 test.ts，用于该模块的所有请求。

```javascript
// test.ts
import request from "@bfp/utils/request";
import downloadData from "@bfp/utils/downLoad"; // 这个是用于导出文档流的请求方法

// 获取表格中列表数据
export const apiTestGetList = async (data: any): Promise<any> =>
  request("/api/template/getList", { data, method: "POST" });
```

创建完 test.ts 之后，在 services/index.ts 文件中引入该模块接口文件

```javascript
export * from "./test";
```

4. 打开浏览器（已登录状态），访问 http://localhost:8005/test/table，可以看到已经能成功访问该页面了

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21382958/1627611067401-51b4b516-9b54-46ec-aa1b-e075ca496506.png#align=left&display=inline&height=316&margin=%5Bobject%20Object%5D&name=image.png&originHeight=631&originWidth=1901&size=61503&status=done&style=none&width=950.5)

## 2. 介绍 CRUD 页面中的自定义组件 Grid 用法

```html
<Grid />组件由自定义组件<SearchForm />和<TableComponent />两部分组成
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21382958/1627611103435-34ec2984-35cf-41a7-b8b8-2110bb891b45.png#align=left&display=inline&height=222&margin=%5Bobject%20Object%5D&name=image.png&originHeight=443&originWidth=977&size=82727&status=done&style=none&width=488.5)

该组件暴露两个实例，一个是表单的实例 form*，*一个是表格的实例 tableCom，当需要操作该表单或者表格的一些方法，可通过这两个实例操作！

```javascript
eg: const form = $table.current._form // 获取到表单实例
```

当定义`columns`的时候，设置`query`属性，Grid 会根据`query`的定义自动生成表单，query 对应的 type 配置：

```typescript
type QueryType = {
  // 表单的label，对应 Form.Item#label
  // 如空则使用 column.title
  label?: string;

  // 表单的name，对应 Form.Item#name
  // 如空则使用 column.dataIndex
  name: string;

  // 表单控件类型
  type: "select" | "datePicker";

  // 自定义渲染
  render: () => React.ReactNode;

  // colOptions:ColProps,
  colProps: ColProps;

  // formItemOptions
  formItemProps: FormItemProps;

  // select 配置
  selectConfig?: {
    // Options 对应的标题 key，默认 value
    label?: string;

    // Options 对应的值 key，默认 key
    value?: string;

    // 选项列表数据源，支持同/异步方法，直接直接定义数据源对象或者列表
    options?:
      | Promise<object | { key: string; value: string | number }[]>
      | object
      | { key: string; value: string | number }[];

    // 是否显示所有选项
    showAll?: Boolean;
  } & SelectProps<string | number>;

  // RangePickerProps
  pickerConfig?: RangePickerProps;

  elementProps?: any; // 表单元素的属性
};
```

页面都自动生成默认示例了，可根据需求自行调整：

```javascript
const columns = [
  {
    title: "常规字段",
    dataIndex: "field1",
  },
  {
    title: "文本框查询",
    dataIndex: "field2",
    query: true,
  },
  {
    title: "下拉框查询",
    dataIndex: "field3",
    query: {
      type: "select",
      selectConfig: {
        options: testOptions,
        showAll: true,
      },
    },
  },
  {
    title: "日期期间查询",
    dataIndex: "field4",
    query: {
      type: "datePicker",
      name: "startField4-endField4",
    },
  },
  {
    title: "表单有表格隐藏",
    dataIndex: "field5",
    hideInTable: true, // 是否在表格中隐藏该列
    query: true,
  },
  {
    title: "操作",
    dataIndex: "opt",
    fixed: "right",
    render: () => (
      <Space>
        <Button type="primary">编辑</Button>
        <Button type="primary" danger onClick={() => onDelete()}>
          删除
        </Button>
      </Space>
    ),
  },
];
```

该组件同时暴露了两个操作表格的函数，一个是查询表格 handleTableChange()，一个是导出表格 handleExportTable()，当执行了新增或者编辑之后需要重刷新表格时，可执行如图代码：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/21382958/1627613216863-1b3c12c3-a645-4710-a4f8-5df3308d9e37.png#align=left&display=inline&height=59&margin=%5Bobject%20Object%5D&name=image.png&originHeight=118&originWidth=397&size=12472&status=done&style=none&width=198.5)

```javascript
$table.current.handleTableChange();
```

# 三、数据及状态管理

## 1. 常用 hooks

- useState--状态钩子

```javascript
eg:
// 控制模态框的显示隐藏
const [modalVisible, setModalVisible] = useState(false);
```

- useEffect--副作用钩子

```javascript
// 一进入页面默认异步请求枚举接口
eg: useEffect(() => {
  async function fetchData() {
    const { data } = await getIncomeTypesEnums();
    setEnums(data);
  }

  fetchData();
}, []);
```

- useContext--共享状态钩子

```javascript
export const GridContext = React.createContext();

export default () => {
  <GridContext.Provider value={enums}>
    <Grid
      ref={childRef}
      renderForm={onSearchForm}
      columns={columns}
      getDataApi={getTypeManageList}
    >
      <div style={{ marginBottom: 20 }}>
        <Button
          type="primary"
          onClick={() => {
            setAddVisible(true), setCurrentDetail({});
          }}
        >
          添加
        </Button>
      </div>
    </Grid>
  </GridContext.Provider>;
};

// 在需要调用的子组件中引入
import { GridContext } from "../index";

const { cardCategoryEnumMap } = useContext(GridContext);
```

- 自定义 hooks--utils/hooks 目录

## 2. 简易数据流交互

中后台场景下，绝大多数页面的数据流转都是在当前页完成，在页面挂载的时候请求后端接口获取并消费，这种场景下并不需要复杂的数据流方案。但是也存在需要全局共享的数据，如用户的角色权限信息或者其他一些页面间共享的数据。为了实现在多个页面中的数据共享，以及一些业务可能需要的简易的数据流管理的场景，我们基于 hooks & umi 插件实现了一种轻量级的全局数据共享的方案。
a. 如何使用
在 src/models 目录下新建文件，文件名会成为 model 的 namespace。

```javascript
eg. demo.ts 的 namespace 是 demo
```

一个 model 的内容需要是一个标准的 JavaScript function，并被默认导出，可以在 function 中使用 hooks.
例如下面的例子就是一个合法的 model:

```typescript
// demo.ts
export default () => "Hello World";
```

在实际使用场景中，model 可以包含其他 hooks，例如下面的计数器的例子：

```typescript
// counter.ts
import { useState, useCallback } from "react";

export default () => {
  const [counter, setCounter] = useState(0);
  const increment = useCallback(() => setCounter((c) => c + 1), []);
  const decrement = useCallback(() => setCounter((c) => c - 1), []);
  return { counter, increment, decrement };
};
```

b. 使用 model
在代码中使用 model，需要从 umi 中导出 useModel。useModel 是一个 React Custom Hook，传入 namespace 即可获取对应 model 的返回值。

```typescript
import { useModel } from "umi";

export default () => {
  const message = useModel("demo");
  return <div>{message}</div>;
};
```

上述例子中的 message 会包含 demo model 的返回值，即：Hello World；

## 3. 如何进行 mock 数据调试

在 mock 文件夹下新建一个关于该模块的 mock 请求文件，如 user.ts，如果执行启动命令 npm run start 默认开启 mock 接口优先，如果 mock 文件夹下有定义改接口请求，则走这里定义的返回数据，如果没有定义则请求真实的 service 请求。

```javascript
// 操作示例
'POST /api/template/getList': (req: Request, res: Response) => {
    res.status(200).send({
      code: 0,
      data: {
        current: 1,
        orders: [],
        pages: 0,
        records: [
          {
            field1: 't1',
            field2: 't2',
            field3: 't3',
            field4: '2020-02-11',
            field5: 't5'
          },
        ],
        searchCount: true,
        size: 10,
        total: 0
      },
      msg: '成功操作'
    });
  },
```
