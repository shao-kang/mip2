# `mip-im-container`


## 说明

组件功能说
im 组件为即时通信组件，支持图片，文字， 语音（只支持服务器发给客户端，暂不支持客户端主动发送语音）， 图文混合，等消息类型， 具体通讯需要与后端配合联调[相关文档](http://agroup.baidu.com/question-anwser/md/article/1092040)

## 示例
```
<mip-im-container>
    <script type="application/json">
    {
        socketUrl:"ws://10.52.184.3:8237?vn=house&token=123&question_id=123&resource_id=123",
        imConfig: {
            left: {
                bgColor: '#fff',
                bdColor: '#f0f',
                textColor: '#f0f',
                linkColor: '#0ff'
            },
            right: {
                bgColor: '#fff',
                bdColor: '#f0f',
                textColor: '',
                linkColor: '#0ff'
            },
            middle: {
                bgColor: 'transparent',
                textColor: '#555'
            },
            systemTime: {
                bgColor: '#d9d9d9',
                textColor: '#fff',
                timeInterval: 60
            }
        }
    }
    </script>
</mip-im-container>

```
## 属性
### socketUrl
说明：socket 链接接口url， 链接地址由rd 给出 需要包含: 
1. vn 服务垂类(百度给出) 
2. rsource_id 资源方ID（百度给出）
3. question_id 问题ID（百度返回此数据）
4. token 权限验证，前三个字段值合并字符串MD5值，该字段由后端计算输出到前端，不能由前端计算 字符串组合示例：MD5（vn + '-' + rsource_id + '-' + question_id），分割线为- 

socketUrl 举例： ws://10.52.184.3:8237?vn=house&token=123&question_id=123&resource_id=123  
必选项： 是   
类型： 字符串  


### imConfig
说明： 消息列表风格颜色自定义配置, 可以为空，不为空的话下方描述属 性必须都有  
必选项： 否  
类型： 对象  

```
{
    left: {                     // 左侧消息配置
        titleColor: '#999',     // 左侧标题颜色
        bgColor: '#fff',        // 左侧msg 消息体背景色
        bdColor: '#ebebeb',     // 左侧msg 消息体边框色
        textColor: '#333',      // 左侧msg 消息体文本颜色
        linkColor: '#06356b'    // 左侧msg 消息体链接颜色
    },
    right: {                    // 右侧消息配置
        titleColor: '#999',     // 右侧标题颜色
        bgColor: '#fff',        // 右侧msg 消息体背景色
        bdColor: '#ebebeb',     // 右侧msg 消息体边框色
        textColor: '#333',      // 右侧msg 消息体文本颜色
        linkColor: '#06356b'    // 右侧msg 消息体链接颜色
    },
    middle: {                   // 中间系统消息配置
        bgColor: 'transparent', // 系统消息背景色
        textColor: '#999'       // 系统消息文本颜色
    },
    systemTime: {               //  系统时间配置
        bgColor: '#d9d9d9',     //  系统时间背景色
        textColor: '#fff',      //  系统时间文本色
        timeInterval: 300       //  两条消息间隔多久会出现时间消息提示
    },
    mainBgColor: '#f1f1f1'      //  im主体背景色
}

```
## 事件
### countdown 事件
说明 由于倒计时栏设计是吸顶的，而组件不知道页面顶部布局故将数据导出组件使用方自己实现
事件抛出对象 
```
 {
     text:['', ''], // 倒计时前后文字（第一个为倒计时钱文字， 第二个为倒计时后文字）
     time: 23123,   // 倒计时 单位秒
     end: ''        // 倒计时结束后显示文字
 }
```
### inputExtra 事件
说明： input 输入框扩展部分触发事件
触发传值为{index: '索引'， event{name：'click', data: {}}}  具体类型及值见mip-im-extra 部分文档
### statusEvent 事件
说明： im 状态发生变化触发事件， 触发事件取值为 {name: eventName} eventName 取值见下方  
问题创建 : Q_STATUS_NEW  
等待支付: Q_STATUS_WAIT_PAY  
有新的专家回复: Q_STATUS_NEW_ANSWER  
问题关闭: Q_STATUS_CLOSE  
问题关闭且评论完成: Q_STATUS_CLOSE_COMMENTED  
退款中: Q_STATUS_CLOSE_REFUNDING  
退款完成: Q_STATUS_CLOSE_REFUND_SUCC  

## im 组件结构
im 组件分为两部分：
1. 与后端交互封装主要在 mip-im-container组件中
2. 图形样式部分封装在 mip-im组件部分 其中又分为两部分
    - mip-im-msg im消息类型部分
    - mip-im-input im 输入消息部分


## 高级特性
使用高级属性需了解组件的
### customMsgType
说明：此属性为高级属性，自定义消息类型，属性名为消息类型， 值为自定义组件，使用此属性时， 需要将此组件包含在新的mip组件中， 直接在html 中使用此属性无效， 具体使用方法在本文最后给出。
必选项： 否  
类型： 对象  
使用方法  
```
// file im-page.vue
<template>
  <mip-im-container 
    :customInputMsgType="customInputExtraType"
  ></mip-im-container>
</template>

<script>
import CustomInputType1 from 'xxx/xxx/xxx'
import CustomInputType2 from 'xxx/xxx/yyy'

export default {
  data() {
      return {
          customInputMsgType: {
              custom-msg-type: CustomMsgType1,
              custom-msg-type2: CustomMsgType2

          }
      }
  }
}
</script>
```

### customInputExtraType
说明：自定义额外输入类型，属性名为消息类型， 值为自定义组件，使用此属性时注意， 需要将此组件包含在新的mip组件中， 直接在html 中使用此属性无效， 具体使用方法在本文最后给出。
必选项： 否  
类型： 对象  
```
// file im-page.vue
<template>
  <mip-im-container 
    :customInputExtraType="customInputExtraType"
  ></mip-im-container>
</template>

<script>
import CustomInputType1 from 'xxx/xxx/xxx'
import CustomInputType2 from 'xxx/xxx/yyy'

export default {
  data() {
      return {
          customInputExtraType: {
              custom-input-type: CustomInputType1,
              custom-input-type2: CustomInputType2

          }
      }
  }
}
</script>
```
#
