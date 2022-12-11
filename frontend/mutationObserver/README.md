## 监听dom元素变化的api
```javascript
 const dom = document.querySelector('domelement') as Element;
    const observer = new MutationObserver(() => {
      // 触发监听时间后的回调
    });
    observer.observe(msgcenter, { childList: true }); // 监听dom变化

```