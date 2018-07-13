# components

> 此目录放置全局公用组件内容

组件名称遵循大驼峰命名规则，在只有单个文件情况下，直接使用 .vue 组件文件。当组件又包含其它子组件或其它，即内容不能包容在单个 .vue 组件文件情况下，使用文件夹命名组件名称，文件夹名称同样是大驼峰命名，文件夹内包含一个 index.vue 文件表示当前组件默认输出内容。

```bash
# 组件命名示例
FooBar.vue # 不应该是 fooBar.vue 或 foo-bar.vue 或 foo_bar.vue 或其它

# 文件夹命名同上去除 .vue 后缀
```