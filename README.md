## 主仓库地址: https://github.com/zero205/JD_tencent_scf/tree/main
## 不推荐多号用户使用云函数,请转战青龙.
有能力的用户可以看下云函数明年最新收费政策.分析一下.看不懂的话就别看了,到时候群里会发通知.
## 为了您的数据安全,仓库必须为私有!!!
## 配置文件部署方式说明:
1. 需要配合配置文件分支config使用.(请运行对应action获取分支,获取后点击左上方 main 来切换分支.)
2. TENCENT开头的NAME/REGION(SCF_REGION)/MEMORYSIZE/TIMEOUT依然使用Secrets
3. PAT依然使用Secrests
4. TENCENT的SECRET_ID/SECRET_KEY请填入config分支的.env文件.
5. 其余所有环境变量填入config分支的config.yml格式请照第一行填写(追加在后面,每个一行,英文冒号,注意冒号后方空格.bool值(true/false),请带引号, 如XXXX: 'false').

## 云函数部分常用变量说明(默认值见仓库serverless.yml):
1. TENCENT_NAME(Secret): 函数名字,修改后注意删除原来名字的函数,名字强烈建议自己搞一个,别用默认的.
2. TENCENT_MEMORYSIZE(Secret): 运行内存64,128,256.大内存会加快配额消耗.单位Mb.不建议修改.
3. TENCENT_TIMEOUT(Secret): 云函数超时时间,单位秒.不建议修改.简单解决超时问题就是加这个了.不是很推荐,但是也可以.所以多号最简单就是拉长超时.
4. SCF_REGION(Secret): 云函数部署地区,注意修改后删除原地区旧函数.[地区费用表](https://cloud.tencent.com/document/product/583/12281).
5. NOT_RUN(环境变量): 不运行的脚本.如: jd_cfd&jd_joy&jd_jdfactory. 无默认值
6. FS_LEVEL(环境变量):无默认值. car仅加购;card:开卡+加购.
## 云函数常用功能:
### 云函数如何调试/手动运行脚本
@hshx123大佬教程测试那步怎么写的你就怎么用.

如果需要重新触发某个小时内的脚本:

timer触发器TriggerName为'config'.

Message写某个小时(如13,则为13点),就可以现在触发一次那个小时的所有脚本.(建议当前没有任务在跑的时候再触发!)

**触发参数结尾别带.js**
### 云函数如何直接执行单个仓库文件
按上一步,但修改timer触发器TriggerName为'remote'.

会自动远程拉取仓库内对应脚本并运行一次(但并不会更新云函数).

面向场景:适用于比如仓库刚刚更新,但是定时部署还没到更新脚本到云函数.我需要立即运行一次最新版本的这个脚本.

存在问题:加密脚本可能不能远程执行
## FAQ(常见问题):
### 脚本什么时候执行?
配合下方'面向高级用户'第1和3条,阅读config.json文件.不难的,尝试读一下就能理解.
### 异步执行日志都乱的,怎么看日志?
看清每行开头打印格式: '脚本名称:代码行号:日志内容'

无论你是愿意用网页搜索,或者下载下来直接用编辑器过滤,都行.

如果有特殊需求的话,单独运行一次脚本看日志也行.
### TG推送请求错误
好好思考下你怎么上的TG
### Github部署日志:ETIMEOUT ERROR
1. 请尝试手动删除/添加config分支.env文件中的GLOBAL_ACCELERATOR_NA=true,尝试关闭/或开启境外加速.(默认开启,且建议开启)
2. 启用随机分钟部署(默认开启,在定时的小时随机抽取分钟进行执行).关闭请直接禁用Random Cron workflow
### Github部署日志:未找到函数执行入口文件
云函数面板删除函数,更改Secret函数名字TENCENT_NAME,重新运行action部署.
### 云函数流量异常高,触发日志与流量日志对不上
解决方法同上
### 部署日志timeout 900 问题
看教程看教程看教程.没让你手动创建函数就别创建.删除函数,重新运行部署.
### 脚本一些奇怪网络错误,比如推送请求失败,请求太快,没信号
可能是异步导致,考虑使用同步运行.

如何查看是否是异步导致?单独运行一次脚本看是否报错.

云函数从有到现在,一直都是异步的,并不是这次更新以后才变成异步执行的. 异步有问题是难免的.

为什么默认不同步? 怕你跑不完说脚本有问题.
### 有问题怎么解决?
入群讨论.之前自己去云函数看日志.光说不好使,没人解决的了.

比如某个脚本跑不完,或者运行次数太少,反馈一下.我就调,放到哪个小时好一点,你哪个小时外网流量少.反馈一下我就加了,光天天不跑不跑不跑.解决不了.

**需要反馈问题的,入群后看置顶反馈流程!**
## 其他通知(有时会有临时性通知):

**云函数问题可以入群寻求帮助(仔细阅读教程,需要有一定基础,进群后看置顶反馈流程后再提问).入群地址见主仓库.**

**群内回复'云函数扣费'可以获得云函数扣费分析方法和正常扣费水平.**

**额外说一句: 云函数只是一个运行环境,不存在跑不了或者脚本少.自己觉得不合适抓紧换,别在那跟个睿智一样.**
***
**从此向下有一定门槛,需要一定基础/动手能力/理解能力!**

说是门槛,其实也不难,整体来讲云函数难度小于青龙,下面的操作也就相当于青龙自定义脚本的难度.
***
## 面向且仅面向 高级用户 :
1. 金融签到可以在config分支新建JRBODY.txt文件,按jd_bean_sign开头格式写入JRBODY.(请将JRBODY.txt按下一条放入diy文件夹)
2. config分支内diy文件夹内所有内容(如果有的话)会覆盖/加入仓库文件并部署上去(当然包含serverless.yml).
3. config分支diy.sh如果有的话会自动运行.[部分例子](https://github.com/Ca11back/scf-experiment/tree/master/examples)
4. 支持config_diy.json自定义运行规则,见下文.[部分例子](https://github.com/Ca11back/scf-experiment/tree/master/examples)
5. 之前添加Secret:EXPERIMENT.请删除.避免下一次测试开启自动载入.
### config_diy.json使用说明/例子:
**前排提醒,diy文件夹下confg_diy.json中的内容会对config.json也就是官方默认配置的规则是:有则覆盖,无则合并.比如config.json中某脚本4小时运行一次,我在confg_diy.json中3小时运行一次,则规则覆盖.如果那个脚本官方配置中没有,则使用confg_diy.json的配置**

1. 在diy文件夹下新加入config_diy.json来自定义脚本运行,格式与config.json相同. 比如我想自定义运行xx_aaa脚本,从每天6点到23点(另一种时间写入方式见下方第3条)则config_diy为:
```json
{
    "xx_aaa": [6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23],
}
```
2. 同步执行(顺序执行,推荐 总体运行时间短的用户 开启):
- 默认异步运行
- 异步运行脚本运行时间更短,同步是顺序运行,时间更长.
- 同步运行解决了异步运行有潜在的脚本同时运行产生冲突可能性
- 理论上更安全
- 脚本有严重错误的时候,同步依然可以继续执行.异步则会终止.
- 异步运行不支持timeout参数,也就是单脚本有异常也不会在自定义超时时间停止,只能靠TENCENT_TIMEOUT整体超时参数结束.
```json
{
    "params":
    {
        "global":
        {
            "exec": "sync"
        }
    }
}
```
3. 自定义每个脚本的超时时常为15分钟(同步执行才生效).注意:TENCENT_TIMEOUT为全局超时,也就是每次整个函数运行时候的超时时间,建议使用默认值3600.(此处例子我想要xx_aaa脚本每3小时运行,注意3两侧没有方括号).
```json
{
    "xx_aaa": 3,
    "params":
    {
        "global":
        {
            "exec": "sync",
            "timeout": 15
        }
    }
}
```
4. 自定义某个脚本(例子中是jd_speed_sign)超时时常为50分钟(同步执行才生效),在执行这个脚本时,超时会使用50分钟,而不是15分钟:
```json
{
    "xx_aaa": 3,
    "params":
    {
        "global":
        {
            "exec": "sync",
            "timeout": 15
        },
        "jd_speed_sign":
        {
            "timeout": 50
        }
    }
}
```
5. 部分兑换类脚本单独使用一个触发器,且不受timeout参数控制.详见:serverless.yml
