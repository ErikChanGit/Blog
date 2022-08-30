在 react 项目中实现国际化的方式有很多种，这篇文章中，我简述一下我平时最常用的国际化的库： `react-i18next`。

#### 依赖安装

```js
// 安装 react-i18next
npm install react-i18next i18next i18next-browser-languagedetector
```

#### 配置多语言 json

在 src 目录下新建 locales 目录， 并新建 `src/locales/en.json` 文件和 `src/locales/zh.json `, 用于配置多种语言。

配置内容示例如下， 您可以根据自己的需求配置：

```json
// src/locales/en.json
{
    "hello":"hello",
    "world":"world"
}
```

```json
// src/locales/zh.json
{
    "hello":"您好",
    "world":"世界"
}
```

#### 定义 i18n.js

在 src 目录下新建一个 `i18n.js` 文件，引入国际化文件

```js
import i18n from "i18next";
import enUsTrans from "./locales/en.json";
import zhCnTrans from "./locales/zh.json";
import {
  initReactI18next
} from 'react-i18next';

i18n
.use(initReactI18next)
.init({
  resources: {
    en: {
      translation: enUsTrans,
    },
    zh: {
      translation: zhCnTrans,
    },
  },
  //默认语言
  fallbackLng: "zh",
  debug: false,
  interpolation: {
    escapeValue: false,
  },
})


export default i18n;
```

如果想自动根据当前环境设置语言，可以使用 LanguageDetector, 示例如下:

```js
import LanguageDetector from 'i18next-browser-languagedetector';

i18n.use(LanguageDetector)
```

#### 对内容进行国际化

```js
import './i18n';
import { useTranslation } from 'react-i18next'

function App() {
  const { t } = useTranslation();
  return (
    <div className="App">
      <p>{t('hello')}</p>
	  <p>{t('world')}</p>
    </div>
  );
}

export default App;
```

