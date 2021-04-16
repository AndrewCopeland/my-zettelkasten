# 1618596430 js-html-santize

DOMPurify seems like a simple to use solution:
https://github.com/cure53/DOMPurify

```html
<script type="text/javascript" src="/static/purify.min.js"></script>
```

```javascript
parent.innerHTML = DOMPurify.sanitize(marked(content))
```



## Links
