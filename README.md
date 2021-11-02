<div align="center"> 
<h1 align="center">
🌈YCGY健康打卡
</h1>

![](https://img.shields.io/github/stars/ChishFoxcat/YcteiHealth?style=social "Star数量")
![](https://img.shields.io/github/forks/ChishFoxcat/YcteiHealth?style=social "Fork数量")
![](https://img.shields.io/github/license/ReaJason/17wanxiaoCheckin-Actions "协议")

源项目->17wanxiaoCheckin

</div>

## ⚠️声明

* 仅用于日常健康打卡,禁止使用于其他用途
* 非盈利项目,严禁倒卖及获取利益,任何责任与二次编辑和原作者无关

## ✨项目介绍

&emsp;&emsp;伴随着疫情的到来，学校为了解在校师生的健康状况，全校师生都规定在特定的时间进行健康打卡 or 校内打卡，本项目旨在帮助使用完美校园打卡的在校师生提供帮助，每天指定时间进行自动打卡，从每天指定时间打卡的压力中解放出来，全身心地投入到社会主义建设之中去。

&emsp;&emsp;本项目使用了 `requests`、`json5`、`pycryptodome` 第三方库，2.0 版本迎来项目重构，打卡数据错误修改方法，不再是以前的修改代码（不懂代码容易改错或无法下手），而是通过直接修改配置文件即可。



## 🔰项目功能

* [x] 完美校园模拟登录获取 token
* [x] 自动获取上次提交的打卡数据，也可通过配置文件修改
* [x] 支持健康打卡和校内打卡
* [x] 支持多人打卡配置，可单人自定义推送，也可统一推送
* [x] 支持粮票签到收集，自动完成查看课表和校园头条任务
* [x] 支持邮箱、Qmsg、Server 酱、PipeHub 推送打卡消息




## 🎨配置文件



### 💃用户配置

- 打卡用户配置文件位于：`conf/user.json`
- 整个 json 文件使用一个 `[]` 列表用来存储打卡用户数据，每一个用户占据了一个 `{}`键值对，初次修改务必填写的数据为：`phone`、`password`、`device_id`（获取方法：[蓝奏云](https://lingsiki.lanzous.com/iQamDmt165i)，下载解压使用）、健康打卡的开关（根据截图判断自己属于哪一类[【1】](https://cdn.jsdelivr.net/gh/ReaJason/17wanxiaoCheckin/Pictures/one.png)、[【2】](https://cdn.jsdelivr.net/gh/ReaJason/17wanxiaoCheckin/Pictures/two.png)），校内打卡开关（有则开），推送设置 `push`。
- 关于 `post_json`，如若打卡推送数据中无错误，则不用管，若有 null，或其他获取不到的情况，则酌情修改即可，和推送是一一对应的。
- 如果多人打卡，则复制单个用户完整的 `{}`，紧接在上个用户其后即可。



### 🤝统一推送配置

- 统一推送配置文件位于：`conf/push.json`
- 若多用户打卡使用统一推送而不是个别单独推送则在此文件下进行推送的配置



## 💦使用方法（云函数）

> 详细图文教程请前往：[博客](https://reajason.top/2021/03/19/17wanxiaocheckinscf/)

- 云函数 — 函数服务 — 新建云函数

- 自定义创建 — 本地上传 zip 包（17wanxiaoCheckin-SCF v2.1.zip：[蓝奏云](https://lingsiki.lanzous.com/b0ekhmcxe)，密码：2333）

- 上传之后往下滑 — 触发器配置 — 自定义创建 — 触发周期：自定义触发 — Cron 表达式：0 0 6,14 * * * * — 完成 — 立即跳转

- 函数管理 — 函数配置 — 编辑 — 执行超时时间：900 — 保存

- 函数代码 — `src/conf/user.json` — 根据上方的用户配置文件介绍以及里面的注释进行设置【第一次使用推荐 QQ 邮箱推送，数据推送全面】

- 测试 — 若弹框【检测到您的函数未部署......】选是 — 查看执行日志以及推送信息（执行失败请带上执行日志完整截图反馈）

- 第一类健康打卡成功结果：`{'msg': '成功', 'code': '10000', 'data': 1}`，显示打卡频繁也算

- 第二类健康打卡成功结果：`{'code': 0, 'msg': '成功'}`

- 校内打卡成功结果：`{'msg': '成功', 'code': '10000', 'data': 1}`

- 如果你们学校会记录打卡成功与否可直接在 **支付宝小程序** 查看是否记录上去（手机 app 登录的话之前获取的 device_id 就失效了）

- 最后检查推送数据，如果表格中有 None，请根据第二行的信息，搭配第一行推送信息的格式，修改配置文件

  - 打开第一行，找到 updatainfo 这个东西，下面的有 null 的对应就是表格中的 None，记住它的 propertyname

  - 打开第二行，找到对应 perpertyname 的部分，根据 checkValue 的 text 选择你需要的选项，温度自己填个值就可

  - 打开配置文件，找到 post_json 下的 updatainfo，在里面加入你需要修改的值，格式和第一行里面的打卡数据一样

  - ```
    "updatainfo":[
    	{
        	"propertyname": "temperature",  // 这个为第一行中找到值为 null 的那一项
            "value": "35.7"  // 这个值为你想改的值，第二行中获取，如果是温度，自己填自己想的即可
        },
        {
        	"propertyname": "wengdu",
        	"value": "36.4"
        }
    ]
    ```

     

- 由于前面使用软件获取了 device_id，所以请使用 **支付宝小程序** 查看打卡结果是否记录上去，以免手机登录 device_id 失效




## 🙋‍脚本有问题
* 有问题可提 [issue](https://github.com/ReaJason/17wanxiaoCheckin-Actions/issues)
* 也可加群反馈 [交流群](https://github.com/ReaJason/17wanxiaoCheckin-Actions/issues/30)



## 📜FQA

- 若第一类健康打卡或校内大卡打卡推送显示，需要修改对应位置下的 areaStr，修改格式为：`"areaStr": "{\"address\":\"天心区青园路251号中南林业科技大学\",\"text\":\"湖南省-长沙市-天心区\",\"code\":\"\"}"` ，`address`：对应手机打卡界面的下面一行，`text`：对应手机打卡界面的上面一行，根据自己的来，上面填什么就是什么，若是校内打卡的地址获取不到，可查看健康打卡的打卡数据推送里面的 areaStr 复制即可。
- 若打卡结果为 `{'msg': '参数不合法', 'code': '10002', 'data': ;'deptid can not be null'}`，初步认为你们学校打卡数据无法自动获取，每次需要自己填写数据，解决办法为手机登录打卡抓签到包，然后在配置文件的 `post_json` 中填下你的打卡数据。
- ~~若腾讯云函数测试失败，执行日志中出现 `......\nKeyError: 'whereabouts'","statusCode":430}`，这种情况就是选错健康打卡方式了，请选第一种健康打卡。~~，（已在代码中给出相应提示）
- 等待反馈......

