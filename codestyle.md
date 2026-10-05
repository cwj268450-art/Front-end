# JavaScript / HTML / CSS 代码规范

本项目代码规范遵循 [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)。

以下是主要规范要点：

## 1. JavaScript

### 缩进
- 使用 2 个空格进行缩进

### 行长度
- 每行不超过 80 个字符

### 命名规范
| 类型 | 风格 | 示例 |
|------|------|------|
| 变量 | 驼峰命名 | `userName` |
| 函数 | 驼峰命名 | `calculateResult()` |
| 类 | 大驼峰命名 | `CalculatorDisplay` |
| 常量 | 全大写下划线 | `MAX_SIZE` |
| 文件 | 小写驼峰 | `calculatorUtils.js` |

### 变量声明
- 使用 `const` 声明常量，`let` 声明可变变量
- 禁止使用 `var`

```javascript
// 推荐
const API_BASE = 'http://localhost:5000';
let currentExpression = '';

// 不推荐
var currentExpression = '';
```

### 字符串
- 使用单引号 `'` 包裹字符串
- 需要插值时使用模板字符串 `` ` ``

```javascript
// 推荐
const message = 'Hello World';
const result = `Result: ${value}`;

// 不推荐
const message = "Hello World";
```

### 函数
- 函数名使用驼峰命名
- 函数参数后加空格，再加大括号

```javascript
function calculate(expression) {
    // ...
}
```

### 注释
- 复杂逻辑必须添加注释说明
- 行内注释使用 `//`
- 函数上方使用多行注释说明功能

## 2. HTML

### 基本规范
- 文档必须声明 `<!DOCTYPE html>`
- 使用语义化标签（如 `<header>`, `<div>`, `<button>`）
- 属性值使用双引号

```html
<!-- 推荐 -->
<button class="btn-primary" onclick="calculate()">计算</button>

<!-- 不推荐 -->
<button class='btn-primary' onclick=calculate()>计算</button>
```

### 嵌套
- 嵌套元素缩进 2 个空格
- 每个块级元素单独一行

## 3. CSS

### 命名规范
- 使用 kebab-case（短横线分隔）
- 类名要有语义，不要凭样式命名

```css
/* 推荐 */
.calculator-display { }
.history-item { }

/* 不推荐 */
.red-text { }
.big-font { }
```

### 格式
- 选择器和 `{` 之间加空格
- 属性名冒号后加一个空格
- 每个属性单独一行

```css
/* 推荐 */
.calculator {
    background: #1e1e2e;
    border-radius: 12px;
}

/* 不推荐 */
.calculator{background:#1e1e2e;border-radius:12px;}
```

### 颜色
- 统一使用十六进制颜色值
- 避免使用命名颜色（如 `red`），使用十六进制值

## 4. 编程建议

- 避免全局变量污染，尽量使用模块化
- DOM操作前先获取元素引用，避免重复查询
- 异步操作使用 async/await，避免回调地狱
- 错误处理使用 try/catch 捕获
