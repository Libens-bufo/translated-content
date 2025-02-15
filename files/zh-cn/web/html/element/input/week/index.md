---
title: <input type="week">
slug: Web/HTML/Element/Input/week
---
{{HTMLRef("Input_types")}}

{{HTMLElement("input")}} 类型为 **`week`** 的元素会创建输入字段，以便轻松输入年份以及该年（即第 1 周到[52 or 53](https://en.wikipedia.org/wiki/ISO_8601#Week_dates)周）的[ISO 8601 week number](https://en.wikipedia.org/wiki/ISO_8601#Week_dates)

{{EmbedInteractiveExample("pages/tabbed/input-week.html", "tabbed-shorter")}}

控件的用户界面因浏览器而异； 跨浏览器的支持目前受到限制，目前只有 Chrome / Opera 和 Microsoft Edge 支持。 在不支持的浏览器中，该控件会正常降级，使其功能与 [`<input type="text">`](/zh-CN/docs/Web/HTML/Element/input/text)相同。

在 Chrome / Opera 中，在`week`控件提供了用于填写周和年值的插槽，弹出式日历界面（可以更直观地选择它们）以及 “ X” 按钮以清除控件的值。

![](week-control-chrome.png)

Edge 的`week`控制更加精细，使用滚动的滚轮打开周和年的选择器。

![](week-control-edge.png)

<table class="properties">
 <tbody>
  <tr>
   <td><strong><a href="#值">值</a></strong></td>
   <td>A {{domxref("DOMString")}} representing a week and year, or empty</td>
  </tr>
  <tr>
   <td><strong>事件</strong></td>
   <td>{{domxref("HTMLElement/change_event", "change")}} and {{domxref("HTMLElement/input_event", "input")}}</td>
  </tr>
  <tr>
   <td><strong>支持的常用属性</strong></td>
   <td>{{htmlattrxref("autocomplete", "input")}}, {{htmlattrxref("list", "input")}}, {{htmlattrxref("readonly", "input")}}, and {{htmlattrxref("step", "input")}}</td>
  </tr>
  <tr>
   <td><strong>IDL 属性</strong></td>
   <td><code>value</code>, <code>valueAsDate</code>, <code>valueAsNumber</code>, 和 <code>list</code>.</td>
  </tr>
  <tr>
   <td><strong>方法</strong></td>
   <td>{{domxref("HTMLInputElement.select", "select()")}}, {{domxref("HTMLInputElement.stepDown", "stepDown()")}}, and {{domxref("HTMLInputElement.stepUp", "stepUp()")}}</td>
  </tr>
 </tbody>
</table>

## 值

一个 {{domxref("DOMString")}} 代表输入到输入中的星期/年的值。{{SectionOnPage("/en-US/docs/Web/HTML/Date_and_time_formats", "Format of a valid week string")}} 中描述了此输入类型使用的日期和时间值的格式。

您可以通过在 {{htmlattrxref("value", "input")}} 属性中包含一个值来为输入设置默认值，如下所示：

```html
<label for="week">What week would you like to start?</label>
<input id="week" type="week" name="week" value="2017-W01">
```

{{EmbedLiveSample('Value', 600, 60)}}

要注意的一件事是，显示的格式可能与`value`不同，后者始终采用 `yyyy-Www`格式。 例如，当将上述值提交给服务器时，浏览器可能会将其显示为 `Week 01, 2017`, 但是提交的值始终看起来像`week=2017-W01`.

您还可以使用输入元素的 {{domxref("HTMLInputElement.value", "value")}} 属性获取并设置 JavaScript 中的值，例如：

```js
var weekControl = document.querySelector('input[type="week"]');
weekControl.value = '2017-W45';
```

## 其他属性

除了 {{HTMLElement("input")}} 元素共有的属性外，周输入还提供以下属性：

| 属性                    | 描述                                                 |
| ----------------------- | ---------------------------------------------------- |
| [`max`](#max)           | 接受为有效输入的最新年份和星期                       |
| [`min`](#min)           | 最早的年和周接受为有效输入                           |
| [`readonly`](#readonly) | 一个布尔值（如果存在），指示用户无法编辑字段的内容   |
| [`step`](#step)         | 用于用户界面和约束验证的步进间隔（允许值之间的距离） |

### max

接受以上 [值](#值) 部分中讨论的字符串格式的最新（按时间）年份和星期数。 如果输入到元素中的 {{htmlattrxref("value", "input")}} 超过此值，则元素将无法通过[约束验证](/zh-CN/docs/Web/Guide/HTML/HTML5/Constraint_validation)。 如果`max`属性的值不是有效的周字符串，则该元素没有最大值。

此值必须大于或等于`min` 属性指定的年和周。

### min

最早接受的年和周。 如果元素的 {{htmlattrxref("value", "input")}}小于此值，则该元素将无法通过 [约束验证](/zh-CN/docs/Web/Guide/HTML/HTML5/Constraint_validation)。 如果为`min`指定的值不是有效的星期字符串，则输入没有最小值。

该值必须小于或等于 `max` 属性的值。

{{page("/en-US/docs/Web/HTML/Element/input/text", "readonly", 0, 1, 2)}}

### step

{{page("/en-US/docs/Web/HTML/Element/input/number", "step-include")}}

对于`week` 输入， `step` 的值以周为单位，比例因子为 604,800,000（因为基础数值以毫秒为单位）。 `step`的默认值为 1，表示 1 周。 默认的步进基数是-259,200,000，这是 1970 年第一周的开始 (`"1970-W01"`).

_目前，尚不清楚当与`week`输入一起使用时，`"any"` 的值对`step` 意味着什么。 确定该信息后，将对其进行更新。_

## 使用星期输入

乍看之下，周输入听起来很方便，因为它们提供了用于选择周的简单 UI，并且它们标准化了发送到服务器的数据格式，而与用户的浏览器或区域设置无关。 但是， `<input type="week">` 存在问题，因为不能保证所有浏览器都支持浏览器。

我们将研究 `<input type="week">`的基本和更复杂的用法，然后在以后提供有关缓解浏览器支持问题的建议（请参阅 [处理浏览器支持](#处理浏览器支持)).

### 周的基本用途

`<input type="week">` 的最简单用法涉及基本的 `<input>` 和{{htmlelement("label")}} 元素组合，如下所示：

```html
<form>
  <label for="week">What week would you like to start?</label>
  <input id="week" type="week" name="week">
</form>
```

{{EmbedLiveSample('Basic_uses_of_week', 600, 40)}}

### 控制输入大小

`<input type="week">` 不支持 {{htmlattrxref("size", "input")}}. 您必须依靠[CSS](/zh-CN/docs/Web/CSS)来确定大小 。

### 使用 step 属性

您应该能够使用{{htmlattrxref("step", "input")}} 属性来更改每次递增或递减的跳转周数，但是这似乎对支持浏览器没有任何影响。

## 验证方式

默认情况下，`<input type="week">` 不会对输入的值应用任何验证。 UI 实现通常不允许您指定不是有效的周/年的任何内容，这很有用，但是仍然可以在字段为空的情况下提交，并且您可能希望限制可选择的周数范围。

### 设置最大和最小星期

您可以使用 {{htmlattrxref("min", "input")}} 和 {{htmlattrxref("max", "input")}} 属性来限制用户可以选择的有效周数。 在以下示例中，我们设置了 `Week 01, 2017` 的最小值和 `Week 52, 2017`的最大值：

```html
<form>
  <label for="week">What week would you like to start?</label>
  <input id="week" type="week" name="week"
         min="2017-W01" max="2017-W52">
  <span class="validity"></span>
</form>
```

{{EmbedLiveSample('Setting_maximum_and_minimum_weeks', 600, 40)}}

这是上面示例中使用的 CSS。 在这里，我们利用{{cssxref(":valid")}} and {{cssxref(":invalid")}} CSS 属性根据当前值是否有效来设置输入的样式。 我们必须将图标放在输入旁边的 {{htmlelement("span")}} 上，而不是输入本身上，因为在 Chrome 中，生成的内容位于表单控件内，无法设置样式或显示 有效。

```css
div {
  margin-bottom: 10px;
  position: relative;
}

input[type="number"] {
  width: 100px;
}

input + span {
  padding-right: 30px;
}

input:invalid+span:after {
  position: absolute;
  content: '✖';
  padding-left: 5px;
}

input:valid+span:after {
  position: absolute;
  content: '✓';
  padding-left: 5px;
}
```

结果是，在 2017 年 W01 到 W52 之间只有几周才被视为有效，并且可以在支持的浏览器中选择。

### 使周值成为必需

另外，您可以使用 {{htmlattrxref("required", "input")}} }属性来强制填写星期。 因此，如果您尝试提交空白的星期字段，则支持的浏览器将显示错误。

让我们看一个例子； 在这里，我们设置了最小和最大周数，并设置了必填字段：

```html
<form>
  <div>
    <label for="week">What week would you like to start?</label>
    <input id="week" type="week" name="week"
         min="2017-W01" max="2017-W52" required>
    <span class="validity"></span>
  </div>
  <div>
      <input type="submit" value="Submit form">
  </div>
</form>
```

如果您尝试提交不带任何值的表单，浏览器将显示错误。 现在尝试使用示例：

{{EmbedLiveSample('Making_week_values_required', 600, 120)}}

这是不使用支持的浏览器的屏幕截图：

![](week-validation-chrome.png)

> **警告：** HTML 表单验证不能代替脚本来确保输入的数据采用正确的格式。 对于某人来说，对 HTML 进行调整以使其绕过验证或完全删除验证太容易了。 有人也可以完全绕开您的 HTML 并将数据直接提交到您的服务器。 如果您的服务器端代码无法验证其接收到的数据，则在提交格式不正确的数据（或太大，类型错误的数据等）时，灾难可能会发生。

## 处理浏览器支持

如上所述，现在使用星期输入的主要问题是浏览器支持：Safari 和 Firefox 在桌面上不支持它，而旧版本的 IE 不支持它。

诸如 Android 和 iOS 之类的移动平台确实很好地利用了这种输入类型，提供了专门的 UI 控件，使在触摸屏环境中选择值变得非常容易。 例如，Android 版 Chrome 上的周选择器如下所示：![](week-chrome-android.png)

不支持的浏览器会优雅地降级为文本输入，但这在用户界面的一致性（所提供的控件将有所不同）和数据处理方面都产生了问题。

第二个问题是更严重的。 如前所述，在输入一个`week` 的情况下，实际值始终会归一化为 `yyyy-Www`格式。当浏览器退回通用文本输入时，没有什么可以指导用户正确格式化输入的格式（这肯定是不直观的）。 人们可以通过多种方式来写星期值。 例如：

- `2017 年第 1 周`
- `2017 年 1 月 2 日至 8 日`
- `2017-W01`
- 等等。

目前，以跨浏览器方式处理星期/年的最佳方法是让用户在单独的控件中输入星期数和年份 ({{htmlelement("select")}} 元素很流行； 参见下面的示例），或使用 JavaScript 库（例如 [jQuery 日期选择器](https://jqueryui.com/datepicker/)。

## 例子

在此示例中，我们创建了两组用于选择星期的 UI 元素：使用 `<input type="week">`，创建的本机选择器，以及两个用于选择星期/年的 {{htmlelement("select")}} 元素的集合 不支持`week`输入类型的旧版浏览器。

{{EmbedLiveSample('Examples', 600, 140)}}

HTML 看起来像这样：

```html
<form>
  <div class="nativeWeekPicker">
    <label for="week">What week would you like to start?</label>
    <input id="week" type="week" name="week"
           min="2017-W01" max="2018-W52" required>
    <span class="validity"></span>
  </div>
  <p class="fallbackLabel">What week would you like to start?</p>
  <div class="fallbackWeekPicker">
    <div>
      <span>
        <label for="week">Week:</label>
        <select id="fallbackWeek" name="week">
        </select>
      </span>
      <span>
        <label for="year">Year:</label>
        <select id="year" name="year">
          <option value="2017" selected>2017</option>
          <option value="2018">2018</option>
        </select>
      </span>
    </div>
  </div>
</form>
```

周值由下面的 JavaScript 代码动态生成。

```css hidden
div {
  margin-bottom: 10px;
  position: relative;
}

input[type="number"] {
  width: 100px;
}

input + span {
  padding-right: 30px;
}

input:invalid+span:after {
  position: absolute;
  content: '✖';
  padding-left: 5px;
}

input:valid+span:after {
  position: absolute;
  content: '✓';
  padding-left: 5px;
}
```

该代码中可能感兴趣的另一部分是特征检测代码。 要检测浏览器是否支持`<input type="week">`, 我们创建一个新的 {{htmlelement("input")}} 元素，尝试将其 `type` 设置为 `week`, 然后立即检查其 `type` 设置为。 不支持的浏览器将返回`text`，因为`week`类型回退为`text`类型。 如果不支持 , `<input type="week">` 我们将隐藏本机选择器并显示后备选择器 UI ({{htmlelement("select")}}s) .

```js
// define variables
var nativePicker = document.querySelector('.nativeWeekPicker');
var fallbackPicker = document.querySelector('.fallbackWeekPicker');
var fallbackLabel = document.querySelector('.fallbackLabel');

var yearSelect = document.querySelector('#year');
var weekSelect = document.querySelector('#fallbackWeek');

// hide fallback initially
fallbackPicker.style.display = 'none';
fallbackLabel.style.display = 'none';

// test whether a new date input falls back to a text input or not
var test = document.createElement('input');

try {
  test.type = 'week';
} catch (e) {
  console.log(e.description);
}

// if it does, run the code inside the if() {} block
if(test.type === 'text') {
  // hide the native picker and show the fallback
  nativePicker.style.display = 'none';
  fallbackPicker.style.display = 'block';
  fallbackLabel.style.display = 'block';

  // populate the weeks dynamically
  populateWeeks();
}

function populateWeeks() {
  // Populate the week select with 52 weeks
  for(var i = 1; i <= 52; i++) {
    var option = document.createElement('option');
    option.textContent = (i < 10) ? ("0" + i) : i;
    weekSelect.appendChild(option);
  }
}
```

> **备注：** 请记住，有些年份有 53 周（请参阅 [每年的周数](https://en.wikipedia.org/wiki/ISO_week_date#Weeks_per_year))！ 在开发生产应用程序时，您需要考虑到这一点。

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 参见

- 通用 {{HTMLElement("input")}} 元素和用于操作该元素的接口 {{domxref("HTMLInputElement")}}
- [HTML 中使用的日期和时间格式](/zh-CN/docs/Web/HTML/Date_and_time_formats)
- [`<input type="datetime-local">`](/zh-CN/docs/Web/HTML/Element/input/datetime-local), [`<input type="date">`](/zh-CN/docs/Web/HTML/Element/input/date), [`<input type="time">`](/zh-CN/docs/Web/HTML/Element/input/time), 和 [`<input type="month">`](/zh-CN/docs/Web/HTML/Element/input/month)
