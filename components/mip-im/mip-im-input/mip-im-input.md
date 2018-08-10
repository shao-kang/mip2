# `mip-im-input` 

## 说明  
im 组件下方输入消息内容组件   
   
## 示例  
   
## 属性  
### placeholder  
说明 输入框提示  
必选项： 否  
默认值： 请输入回复内容  
类型： 字符串  
   
### disabled  
说明 输入框提示  
必选项： 否  
默认值： false  
类型： 字符串  
### customTypes  
说明 自定义额外输入类型  
必选项： 否  
默认值： {}  
类型： 对象  
### value  
说明 输入框提示  
必选项： 否  
默认值： false  
类型： 字符串  
### extraList  
说明 拓展输入类型列表 具体类型及数据结构看下方 mip-im-input-extra-xxx 组件  
必选项： 否  
默认值： 【】  
类型： 数组  
      
## 事件  
### extra-event
说明： 扩展输入框触发事件， 具体取值见下方对应扩展类型描述
# `mip-im-input-extra-base`  
## 说明  
拓展输入框 基础类型  
## 属性  
### img  
说明： 扩展输入框图片  
必选： 是  
类型： 字符串  
### text  
说明： 扩展输入框文字  
必选: 是  
类型： 字符串  
### index  
说明： 扩展输入框索引  
必选： 是  
类型: 数字  
## 事件  
### extra-event   
说明： 扩展输入框触发点击事件  
```
{
    index: 1,           // 扩展输入框触发事件的索引  
    event: {            // 触发事件  
        name: 'click'   // 事件名称 点击  
    } 
}
```
# `mip-im-input-extra-link`  
## 说明  
拓展输入框 链接类型  
## 属性  
### img  
说明： 扩展输入框图片  
必选： 是  
类型： 字符串  
### text  
说明： 扩展输入框文字  
必选: 是  
类型： 字符串  
### index  
说明： 扩展输入框索引  
必选： 是  
类型: 数字  
### link  
说明：链接参数 包含三个参数 type 是否为mip 跳转   url 跳转链接 title 标题（详情参考mip 链接组件）  
必选：是  
类型：对象  

# `mip-im-input-extra-upload`
## 属性
### img
说明： 扩展输入框 上传图片  
必选： 是  
类型： 字符串  
### text  
说明： 扩展输入框文字  
必选: 是  
类型： 字符串  
### index  
说明： 扩展输入框索引  
必选： 是  
类型: 数字  
### uploadUrl
说明： 上传地址  
必选： 是   
类型： 字符串  
### filename
说明： 上传文件名称 
必选： 否  
类型： 字符串  
默认： picFile  
### accept
说明： 接受类型  
必选： 否  
类型： 字符串  
默认值： image/*  
### limit
说明：图片大小限制 单位M  
必选：否  
类型：数字  
默认值： 50   

## 事件
### extra-event
组件触发四种事件 包含上传图片之前， 图片预览， 图片上传成功， 图片上传失败
图片上传之前 触发 extra-event 传递消息值为{ index：'索引', event: {name: 'breforeUpload'}}
图片上传预览 触发 extra-event 传递消息值为{ index：'索引', event: {name: 'preview'，content：{}}} content 为具体图片信息包含图片二进制，类型等信息
图片上传成功 触发 extra-event 传递消息值为{ index：'索引', event: {name: 'uploadSuccess'， data： {}}} data 为服务器返回消息
图片上传失败 触发 extra-event 传递消息值为{ index：'索引', event: {name: 'uploadFailed'， data： {}}} data 为服务返回消息

# `mip-im-input-extra-phone`
## 说明
拓展输入框 拨打电话类型  
## 属性
### img
说明： 扩展输入框图片  
必选： 是  
类型： 字符串  
### text
说明： 扩展输入框文字  
必选: 是  
类型： 字符串  
### index
说明： 扩展输入框索引  
必选： 是  
类型: 数字  
### phone
说明：电话信息  
必选：是  
类型：字符串  
