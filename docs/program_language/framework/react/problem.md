## Ant Design 中 input 无法输入的问题
Ant Design 是阿里巴巴开发的一套企业级 UI 设计语言和 React 组件库， 极大地提高了前端开发人员的开发效率。

在我的开发过程中，使用过 ant design 中的 *input* 组件，遇到了 input 框无法输入的问题。

### 错误示例

```html
<input className='input_obj' onChange={} value="test"> </input>
```

在浏览器中打开，在 input 框中输入数据时,  发现内容一致都不变，内容都为 test， 如同被禁止修改一样。

原因是因为在 ant design 中，input 组件的 value 属性赋值后, input 的值会一直保持 value ， 修改后仍然会被重新赋值为 value。

### 正确示例

```html
<input className='input_obj' onChange={} defaultValue="test"> </input>
```

将 value 修改成 defaultValue, 默认值可以被修改

