# Loader 和 Plugin 的区别？编写 Loader，Plugin 的思路？

## Loader 和 Plugin 的区别

- loader 运行在打包文件之前，plugins 在整个编译周期都起作用
- 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果
- 对于 loader，实质是一个转换器，将 A 文件进行编译形成 B 文件，操作的是文件，比如将 A.scss 或 A.less 转变为 B.css，单纯的文件转换过程

## 编写 loader

编写原则：

- 单一原则: 每个 Loader 只做一件事，保持功能单一，避免做多种功能
- 链式调用: Webpack 会按顺序链式调用每个 Loader；
- 统一原则: 遵循 Webpack 制定的设计规则和结构，输入与输出均为字符串，各个 Loader 完全独立，即插即用；

loader 的本质为函数，函数中的 this 作为上下文会被 webpack 填充，因此我们不能将 loader 设为一个箭头函数。函数接受一个参数，为 webpack 传递给 loader 的文件源内容

```javascript
// 导出一个函数，source为webpack传递给loader的文件源内容
module.exports = function (source) {
  const content = doSomeThing2JsString(source);

  // 如果 loader 配置了 options 对象，那么this.query将指向 options
  const options = this.query;

  // 可以用作解析其他模块路径的上下文
  console.log("this.context");

  /*
   * this.callback 参数：
   * error：Error | null，当 loader 出错时向外抛出一个 error
   * content：String | Buffer，经过 loader 编译后需要导出的内容
   * sourceMap：为方便调试生成的编译后内容的 source map
   * ast：本次编译生成的 AST 静态语法树，之后执行的 loader 可以直接使用这个 AST，进而省去重复生成 AST 的过程
   */
  this.callback(null, content); // 异步
  return content; // 同步
};
```

## 编写 plugin

如果自己要实现 plugin，需要遵循一定的规范：

- 插件必须是一个函数或者是一个包含 apply 方法的对象，这样才能访问 compiler 实例
- 传给每个插件的 compiler 和 compilation 对象都是同一个引用，因此不建议修改
- 异步的事件需要在插件处理完任务时调用回调函数通知 Webpack 进入下一个流程，不然会卡住

```javascript
class MyPlugin {
  // Webpack 会调用 MyPlugin 实例的 apply 方法给插件实例传入 compiler 对象
  apply(compiler) {
    // 找到合适的事件钩子，实现自己的插件功能
    compiler.hooks.emit.tap("MyPlugin", (compilation) => {
      // compilation: 当前打包构建流程的上下文
      console.log(compilation);

      // do something...
    });
  }
}
```
