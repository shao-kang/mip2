# im 与后端通信websocket 接口
## 概述
与后端通信主要包括几个阶段的：
1. 建立websocket 链接，并进行身份认证， 拉取已存在信息
2. 保持链接连接状态， 等待后端推送数据
3. 发送消息
4. 关闭websocket 链接

### 后端认证个人信息拉取已存在消息
```
认证成功
data: {
    msgList:[],
    inputConfig: {

    }
}
认证失败： data：{

}

```
### 保持链接收后端推送信息
```
data: {
    msgList:[]，
    inputConfig: {}
}
```

### 向后端发送信息
向后端发送信息，现阶段只能发送两种信息，文字信息，和图片信息
只需填写content 内容和type字段 ，其他字段由后端返回
```
data: {
    msg: {}
}
```
### 关闭 websocket 链接
### 问题
有没有优化策略的必要，比如10分钟没有接触界面是不是可以主动关闭连接

# im 消息类型接口
## im 通用接口
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|type       | 消息类型 具体类型见下方|       | 无   | 是|
|messageId| 消息id|  String|        | 是|
|timestamp| 消息时间戳（秒）| Number| 0 |是|
|avatar    | 消息头像链接| String|     | 否|
|avatarLink |头像跳转链接| Object| null| 否|
|avatarLink.url| 跳转地址| String| |是
|avatarLink.type| 链接类型| 'mip' ''|''| 否
|avatarLink.title| 跳转链接标题，当type为mip时可用|''|''| 否
|align| 消息对齐方式| 'left' 'right' 'middle'|  | 是
|content| 消息内容，每个消息类型其中包含字段不同详情看具体类型|Object| | 是|
### mip-im-item-text
说明： 文字类型，包含带链接的文字
属性
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|messageId| 消息id|  String|        | 是|
|timestamp| 消息时间戳（秒）| Number| 0 |是|
|avatar    | 消息头像链接| String|     | 是|
|avatarLink |头像跳转链接| Object| null| 否|
|avatarLink.url| 跳转地址| String| |是
|avatarLink.type| 链接类型| 'mip' ''|''| 否
|avatarLink.title| 跳转链接标题，当type为mip时可用|''|''| 否
|align| 消息对齐方式| 'left' 'right'|  | 是
|content| 消息内容|Object| | 是|
|content.text| 消息内容 字段有两种类型 String： 直接展示， Array： item为{url：'跳转链接'， type： 跳转链接类型可填mip或空， title： 跳转后标题 type 为mip时可用，text： 文字}|String Array| '' | 是|
### mip-im-item-img
说明： 图片类型
属性
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|messageId| 消息id|  String|        | 是|
|timestamp| 消息时间戳（秒）| Number| 0 |是|
|avatar    | 消息头像链接| String|     | 是|
|avatarLink |头像跳转链接| Object| null| 否|
|avatarLink.url| 跳转地址| String| |是
|avatarLink.type| 链接类型| 'mip' ''|''| 否
|avatarLink.title| 跳转链接标题，当type为mip时可用|''|''| 否
|align| 消息对齐方式| 'left' 'right'|  | 是
|content| 消息内容|Object| | 是|
|content.img| 图片地址 |String | '' | 是|

### mip-im-item-text-btns
说明： 文字按钮混合类型
属性
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|messageId| 消息id|  String|        | 是|
|timestamp| 消息时间戳（秒）| Number| 0 |是|
|avatar    | 消息头像链接| String|     | 是|
|avatarLink |头像跳转链接| Object| null| 否|
|avatarLink.url| 跳转地址| String| |是
|avatarLink.type| 链接类型| 'mip' ''|''| 否
|avatarLink.title| 跳转链接标题，当type为mip时可用|''|''| 否
|align| 消息对齐方式| 'left' 'right'|  | 是
|content| 消息内容|Object| | 是|
|content.title|消息标题|string|''|否|
|content.btns| btns 中一个对象的格式为{url：'aaa', title: 'nihao', type: 'mip', text: 'btn1'}  url 为将要跳转的链接 title 为跳转页面的title type为url类型取值为mip 或空, text: 为按钮中文字 |Array | [] | 是|
|content.text| 消息内容 字段有两种类型 String： 直接展示， Array： item为{url：'跳转链接'， type： 跳转链接类型可填mip或空， title： 跳转后标题 type 为mip时可用}|String Array| '' | 是|


### mip-im-item-list-btns
说明： 纵向排列的按钮类型
属性
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|messageId| 消息id|  String|        | 是|
|timestamp| 消息时间戳（秒）| Number| 0 |是|
|avatar    | 消息头像链接| String|     | 是|
|avatarLink |头像跳转链接| Object| null| 否|
|avatarLink.url| 跳转地址| String| |是
|avatarLink.type| 链接类型| 'mip' ''|''| 否
|avatarLink.title| 跳转链接标题，当type为mip时可用|''|''| 否
|align| 消息对齐方式| 'left' 'right'|  | 是
|content| 消息内容|Object| | 是|
|content.title|消息标题|string|''|否|
|content.links| links 中一个对象的格式为{url：'aaa', title: 'nihao', type: 'mip', text: 'btn1'}  url 为将要跳转的链接 title 为跳转页面的title type为url类型取值为mip 或空, text: 为按钮中文字 |Array | [] | 是|
|content.btns| btns 中一个对象的格式为{url：'aaa', title: 'nihao', type: 'mip', text: 'btn1'}  url 为将要跳转的链接 title 为跳转页面的title type为url类型取值为mip 或空, text: 为按钮中文字 |Array | [] | 是|


### mip-im-item-text-img
说明： 文字图片混合类型
属性
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|messageId| 消息id|  String|        | 是|
|timestamp| 消息时间戳（秒）| Number| 0 |是|
|avatar    | 消息头像链接| String|     | 是|
|avatarLink |头像跳转链接| Object| null| 否|
|avatarLink.url| 跳转地址| String| |是
|avatarLink.type| 链接类型| 'mip' ''|''| 否
|avatarLink.title| 跳转链接标题，当type为mip时可用|''|''| 否
|align| 消息对齐方式| 'left' 'right'|  | 是
|content| 消息内容|Object| | 是|
|content.img| 图片地址 |String | '' | 是|
|conent.position| 图片位置| 'bottom' 'top'| 'bottom'| 否|
|content.text| 消息内容 字段有两种类型 String： 直接展示， Array： item为{url：'跳转链接'， type： 跳转链接类型可填mip或空， title： 跳转后标题 type 为mip时可用}|String Array| '' | 是|
### mip-im-item-audio
说明： 音频类型
属性
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|messageId| 消息id|  String|        | 是|
|timestamp| 消息时间戳（秒）| Number| 0 |是|
|avatar    | 消息头像链接| String|     | 是|
|avatarLink |头像跳转链接| Object| null| 否|
|avatarLink.url| 跳转地址| String| |是
|avatarLink.type| 链接类型| 'mip' ''|''| 否
|avatarLink.title| 跳转链接标题，当type为mip时可用|''|''| 否
|align| 消息对齐方式| 'left' 'right'|  | 是
|content| 消息内容|Object| | 是|
|content.totalTime| 音频播放时长|Number|0| 是|
|content.audioSource| 音频来源链接属性对应原生audio source 属性对应item 为{src: 音频地址, type: 音频格式} 设计为数组是为了满足音频可能同时存在多种格式情况，type 为可选参数（举例： 'audio/mp3'） |Array |  | 是|
### mip-im-item-system
说明： 系统消息
属性
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|messageId| 消息id|  String|        | 是|
|timestamp| 消息时间戳（秒）| Number| 0 |是|
|align| 消息对齐方式| 'middle'| 'middle' | 是
|content| 消息内容|Object| | 是|
|content.text| 消息内容|String | '' | 是|


# input 消息接口
说明 input 接口
示例
属性
| 属性      | 说明      | 类型| 默认值| 是否必填
|---------  | ---------| ----| ---- |------|
|placeholder| 提示|    | String| 请出入| 否|
|disabled   | 是否可用|  Boolean| false| 否
| value     | input 值|String| |  否
|extraList  | input 下方额外输入框 item为{type：'', content:{}}| Array| [] | 否
> extraList item 具体内容为三种
的类型： mip-im-extra-base 、mip-im-extra-link 、mip-im-extra-upload
type 为 mip-im-extra-base 时
content为 {img： 图片地址， text： 文字说明}
type 为mip-im-extra-upload时
content 为{img： 图片地址， text： 文字说明， uploadUrl： 上传文件地址， ~~filename： 文件名称~~， accept： 文件类型 默认为图片类型， limit： 限制大小}
type 为 mip-im-extra-link 时
content为 {img： 图片地址， text： 文字说明, link: {type: 'mip或不填'， url： '链接'， title： 'mip 页面标题' }}
