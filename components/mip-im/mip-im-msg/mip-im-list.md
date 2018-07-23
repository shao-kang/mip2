# `mip-im-list`

## 说明

im 组件列表

## 示例

示例说明

## 属性
### im-list
说明：消息列表 
必填：是  
格式：数组 
#### item 说明
##### type
说明：消息类型
必填：是  
格式：字符串
##### messageId
说明：消息Id
必填：是  
格式：字符串
##### avatar
说明：消息头型URL
必填：否  
格式：字符串
##### avatarLink
说明：头像链接  {url, type, title} url 为跳转链接， type 为链接类型枚举mip或者空， title 链接跳转标题
必填：否
格式：对象
##### content
说明：消息内容具体看下方具体消息类型
必填：是  
格式：对象

### config
说明： im 配置
必填：否
格式：对象
#### left
说明： 左侧消息配置
必填：否
格式：对象
##### bgColor
说明：背景色
必填：否
格式：字符串
##### bdColor
说明：边框颜色
必填：否
格式： 字符串
##### textColor
说明：文字颜色
必填：否
格式：字符串
##### linkColor
说明：链接颜色
必填：否
格式：字符串

#### right
说明：右侧消息配置
必填：否
格式：对象
##### bgColor
说明：背景色
必填：否
格式：字符串
##### bdColor
说明：边框颜色
必填：否
格式： 字符串
##### textColor
说明：文字颜色
必填：否
格式：字符串
##### linkColor
说明：链接颜色
必填：否
格式：字符串
#### middle
说明： 中间系统消息配置
必填：否
格式：对象
##### bgColor
说明：背景色
必填：否
格式：字符串
##### textColor
说明：文字颜色
必填：否
格式：字符串

#### systemTime
说明： 时间提示配置
必填：否
格式：对象
##### bgColor
说明：背景色
必填：否
格式：字符串
##### textColor
说明：文字颜色
必填：否
格式：字符串
##### timeInterval 
说明： 信息之前显示时间的时间差 单位秒 默认为300 s
必填： 否
格式： 数字


### custom-msgs
说明： 自定义消息类型，与消息列表中type 相对应
必填：否
格式：对象 {'custom-msg-name': customMsg， custom-msg-name-1': customMsg1}， custom-msg-name 指消息类型  customMsg指自定义消息类型


# `mip-im-item-triangle`
## 说明
对话气泡的边缘箭头，与具体的msg 配合使用
## 属性
### align
说明: 对齐方式
必填： 是
格式： 枚举：'left' 'right'
### bgColor
说明：背景色
必填：是
格式：字符串
### bdColor
说明：边框颜色
必填：是
格式：字符串

# `mip-im-item`
## 说明
msg 基础类型可在此基础上开发msg 类型
## 属性
### type
### align
说明: 对齐方式
必填： 是
格式： 枚举：'left' 'right'
### bgColor
说明：背景色
必填：是
格式：字符串
### bdColor
说明：边框颜色
必填：是
格式：字符串
### triangle

### avatar
说明：消息头型URL
必填：否  
格式：字符串
### avatarLink
说明：头像链接  {url, type, title} url 为跳转链接， type 为链接类型枚举mip或者空， title 链接跳转标题
必填：否
格式：对象


# `mip-im-item-text`
## 说明： 
im 消息类型： 文字性
## 示例
## 属性
### align
说明：对齐方式 
必填：否
格式：字符串 left或right
默认值： left


# `mip-im-item-text-img`
## 说明： im 消息类型： 图文混合型
## 示例
## 属性
必填：否
格式：对象

说明： 时间提示配置
必填：否
格式：对象
# `mip-im-item-text-btns`
# `mip-im-item-img`
# `mip-im-item-system`
# `mip-in-item-audio`
# `mip-in-audio-animation`


