[![Build Status](https://travis-ci.org/butterandfly/tk-api-behavior.svg?branch=master)](https://travis-ci.org/butterandfly/tk-api-behavior)
# \<tk-api-behavior\>

该组件提供制作tk-api元素的各种方法。
主要是将：`url + url params + query params + body（甚至headers）`的复杂性封装为`apiAction + apiArgs`。

继承者需要实现`actionMeta`属性，如：
```
get actionMeta() {
  return this.createRestfulActionMeta('table');
}
```

此外还提供
- lastStatusCode
- 从全局设置里面得到host、port等，将其自动拼接在url上
- 一些必要的属性如loading、lastResponse等（从iron-ajax绑定）

### Action Meta
该对象标明了每个Action的行为，格式如下：
```
{
  find: ['GET', '/users', ''],
  findById: ['GET', '/users/:id', 'url=id']
}
```

Key是action名，Value必须是一个数组，该数组标明了其行为。
数组的第一个元素是请求方法；第二个元素是url，可使用参数格式；第三个元素的apiArgs的参数放置表达式。
如`['GET', '/users', '']`，就是发一个url为`/users`的GET请求；
`['GET', '/users/:id', 'url=id']`，发一个GET请求，url为`/users/:id`，后面的`:id`会被apiArgs对象里id的值覆盖。

### 参数放置表达式
`url=id,body=table`，意思是把apiArgs里的id作为url里的参数，table作为body，剩下其它的作为query里的参数；
例如若url为/table/:id，apiArgs为`{id: 1, table: {content: 'hihi'}, limit: 2}`，则最后生成的url是`/table/1?limit=2`，body是`{content: 'hihi'}`。
