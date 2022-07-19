### vue3动态组件
```html
<component :is="component.get(activeKey)"></component>
```
```js
import { defineAsyncComponent, ref, shallowRef } from 'vue';
const activeKey = ref('screenLock');
const component = shallowRef(new Map<string, any>());
const menuComponent = ['screenLock'];
menuComponent.forEach((item) => {
  component.value.set(
    item,
    defineAsyncComponent(
      () => import(`@/views/${item}/components/IndexView.vue`)
    )
  );
});
```
### input输入后，点击按钮不触发click
按钮添加`mouseenter`事件，当鼠标移入后，让input失焦
### props多个类型
```js
type: [String, Boolean]
```
### vue less更换主题
创建一个`theme.css`放在`public`文件夹下，然后在`index.html`引入
```css
 /* theme.css */
.app-wrap-black {
  --bg-top: #2e3135;
}
.app-wrap-white {
  --bg-top: #fff;
}
.bg-top {
  background-color: var(--bg-top);
}
```
根元素的默认类名为`app-wrap-black`，更换主题时，将类名更换为`app-wrap-white`即可
### flex:1 元素overflow:auto不生效
将元素的`height`设为0
### 当有`teleport`存在，切换路由时会报错
```html
<teleport to="#container" v-if="hasLoad">
</teleport>
```
store中设置全局状态`hasLoad`，但切换路由的时候，将它设置为`false`，可在路由钩子中实现
### vue动态加载图片
```html
<img :src="require('')">
```
### git多人协作
方式1：
```bash
git stash
git pull
git stash pop
解决冲突
git add .
git commit -m ''
git push
```
方式2：
```bash
git add .
git commit -m ''
git pull --rebase
解决冲突
git add .
git rebase --continue
git push
```
方式3：
```bash
git add .
git commit -m ''
git log # 获取commit id
git reset --hard HEAD~1 # 强制恢复到上一个提交
git pull # 拉取最新代码
git cherry-pick commitId
解决冲突
git cherry-pick --continue
git push
```
### 八位数颜色
获取透明度和颜色
```js
    alpha = Math.round(parseInt(val.slice(1,3),16) / 255  * 100);
    color = '#' + val?.slice(3)
```
透明度+颜色转换为八位数颜色
```js
let alpha = Math.round(res.alpha / 100 * 255).toString(16);
if (alpha.length < 2) {
    alpha = '0' + alpha;
}
newXmlJson.value[key] = '#' + alpha + res.color.slice(1);
```
八位数颜色转rgba
```js
const val = newXmlJson.value[key] ? newXmlJson.value[key] : xmlJson.value[key]
const argb = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(val);
if(argb && argb.length > 0) {
    const r = parseInt(argb[2], 16);
    const g = parseInt(argb[3], 16);
    const b = parseInt(argb[4], 16);
    const a = parseInt(argb[1], 16) / 255;
    return 'rgba(' + r + ',' + g + ',' + b + ',' + a.toFixed(0) + ')';
}
```
