# es module 和 commonJS 的区别

- 导入导出语法不同：commonjs 是 module.exports，exports 导出，require 导入；ES6 则是 export 导出，import 导入
- commonjs 是运行时加载模块，ES6 是在静态编译期间就确定模块的依赖
- commonJs 输出的是值的浅拷贝，可以修改导出的值，这在代码出错时，不好排查引起变量污染；esModule 输出值的引用，之前都存在映射关系，并且值都是可读的，不能修改
- commentJs 具有缓存。在第一次被加载时，会完整运行整个文件并输出一个对象，拷贝（浅拷贝）在内存中。下次加载文件时，直接从内存中取值
