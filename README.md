# Ajax数据请求防错完整机制

## 请求成功

### 情况一
> 返回码正常，返回数据正常

- 这类情况不需要处理

```js
{
	returnCode:"0",// 字符串
	errorMessage:"参数错误",// 错误信息
	// 常规数据
	data:{
		a:1,
		b:2,
		c:[1,2,3]
	}
}
```

### 情况二
> 返回码正常，但是返回入口的data无数据

- 需要进行无数据渲染（考虑无数据图片进行填充）

```js
{
	returnCode:"0",// 字符串
	errorMessage:"参数错误",// 错误信息
	// 常规数据
	data: null || [] || {} || undefined
}
```

### 情况三
> 返回码正常，但是返回入口的data无数据

- 需要进行无数据渲染（考虑无数据图片进行填充）

```js
{
	returnCode:"0",// 字符串
	errorMessage:"参数错误",// 错误信息
	// 常规数据
	data:{
		a:1,
		b:2,
		c:[] || null || undefined
	}
}
```

> 总结上述要点：先判断返回码，返回码returnCode:"0"正常,再判断data是不是一个有内容的对象，最后在data有内容的情况下是否有需要的字段（字段不一这个需要特殊处理）
```js
if () {
}
```

## 请求失败

# 情况一

> 普通请求失败返回给我一个对象体，我输出错误码

```js
{
    data: null
    errorMessage: "Invalid bound statement (not found)"
    returnCode: "E99999"
}
```

# 情况二

> 返回了一个字符串，这个直接弹出字符

```
系统出现异常，请联系管理员! 
```

> 总结上述要点： 如果是一个不为空的对象（建议用try-catch包裹）并弹窗处错误信息
```js
    .fail(function(response) {
        var message = "错误 ";
        try {
            message += response.responseJSON.returnCode + " " + response.responseJSON.errorMessage;
            notie.alert({text: message, time: 5});
        } catch(e) {
            message += response.responseText;
            notie.alert({text: message, time: 5});
        }
    });
```

### Ajax 自己总结了一下原生ajax的写法以及
需要注意的要点 https://github.com/Kelichao/Ajax/issues
