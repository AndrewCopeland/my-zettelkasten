# 1618519549 js-alertify-enter-prompt

```javascript
alertify.prompt( 'New note', 'Title', ''
			, function(evt, value) { 
				alertify.success('You entered: ' + value) 
				apiNewCode(value)
			}
			, function() { 
				alertify.error('Cancel') 
});
```


## Links
