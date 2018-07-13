# pages

> 此目录放置页面内容

每一个页面对应一个文件夹，文件夹命名采用连字符命名规则，文件夹内必定包含一个 index.vue 文件表示页面类型组件。一般页面的组成都是积木组件的拼装，所以页面文件夹内应该还包含一些页面业务需求的组件。因为我们已经使用了 element-ui 提供的组件，所以我们只需要对页面内容进行组件分层即可。

```bash
# 文件夹命名示例
foo-bar # 不应该是 fooBar 或 FooBar 或 foo_bar 或其它
```

```bash
# 目录结构
pages/
|── foo-bar/                  # 页面文件夹
|   |── index.vue/            # 必定包含的页面页面类型组件
|   |── TabFirst.vue/         # 页面业务需求组件
|   |── DialogAbout.vue/      # 页面业务需求组件
|   └── ...                   # 其它业务需求组件
|
└── ...                       # 其它页面文件夹
```

页面是独立的，与菜单没有直接关系，菜单只是对用户的体现。

前期项目的不确定因素太多，不应该过早的对页面进行分组分层，应该循序渐进，等迭代几个版本、项目趋于稳定、页面数量较大时再进行分组优化。