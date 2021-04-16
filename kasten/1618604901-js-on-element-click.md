# 1618604901 js-on-element-click

The `code` in this case is an element.
```javascript
document.addEventListener('click', function (event) {
	if (!event.target.matches('code')) return;
	copyTextToClipboard(event.target.innerText)
}, false);
```


## Links
