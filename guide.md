# IFTTT Webhook 配置指南

###  简介

IFTTT is the free way to get all your apps and devices talking to each other. Not everything on the internet plays nice, so we're on a mission to build a more connected world.

### 链接
> https://ifttt.com/

## 配置主要分为以下两个部分

* IFTTT
* cocoBlockly

## IFTTT
* 步骤一
  *  在官网注册一个账号

* 步骤二

  * 点击 **New Applet**

  ![step2-1](./img/ifttt/step2-1.png)

  * 点击 **this**

  ![step2-2](./img/ifttt/step2-2.png)
  * 在search service中 输入 **webhook** 并点击 **webhook**

  ![step2-3](./img/ifttt/step2-3.png)

  * 选择 trigger  **Receive a web request**

  ![step2-4](./img/ifttt/step2-4.png)

  * 设置对应的 **Event** 并点击 **Create trigger**

   一个event 名称对应一个 trigger ，如果有多个trigger，请命名多个event
   > 范例中我们生成一个 temp_data_generate的event，由此来追踪温度产生的新数据

  ![step2-5](./img/ifttt/step2-5.png)

  * 点击 **that**

  ![step2-6](./img/ifttt/step2-6.png)

  * 在 Service search 中填入 **google sheets** 并点击 **Google Sheets**

  ![step2-7](./img/ifttt/step2-7.png)

  * 点击 **Connect** 并登入你的google 账号

  ![step2-8](./img/ifttt/step2-8.png)

  * 在 Choose action 中选择 **Add row to spreadsheets**

  ![step2-9](./img/ifttt/step2-9.png)

  * 在 Complete action fields中填入以下几项  
    * SpreadSheet name

      > 档案的名称

    * Formatted row (用 ||| 来分隔， value1，2，3对应要添加的数据)

      >每一行要添加的资料

    * Driver folder path
      > 文件的路径 （不能为空 ，否则会失败）

  ![step2-10](./img/ifttt/step2-10.png)
  * 点击 **Create action**

  * 在 Review and finish 中 点击 Finish

  ![step2-11](./img/ifttt/step2-11.png)

## CocoBlockly
* 步骤一

  * 打开 [Blockly](http://dev.cocorobo.hk/t/cocoblockly/dev/)

* 步骤二

  * 点击 在Upload Area 处 的 **切换按钮** （接下来会看到界面变成红色)

  ![step2-1](./img/blockly/step2-1.png)

* 步骤三

  * 在[积木功能栏](https://cocorobolabs.gitbooks.io/cocoblockly/content/chapter-1-introduction.html)中选择 WiFi Module -> Network -> Wifi connect setup

  ![step2-2](./img/blockly/step2-2.png)

  * 在[积木功能栏](https://cocorobolabs.gitbooks.io/cocoblockly/content/chapter-1-introduction.html)中选择 WiFi Module -> Network -> if isConnected do

  ![step2-3](./img/blockly/step2-3.png)

  * 在[积木功能栏](https://cocorobolabs.gitbooks.io/cocoblockly/content/chapter-1-introduction.html)中选择 WiFi Module -> Web Service -> Web Service IFTTT

  ![step2-4](./img/blockly/step2-4.png)

  * 将if isConnected do 与 Web Service IFTTT 拼接在一起

  ![step2-5](./img/blockly/step2-5.png)

  * 在 wifi connect setup 中填入
    * ssid （wifi的名称）
    * password (wifi的密码)
  * 在 web Service IFTTT 中填入以下

    > key 2，3 以及 value 2，3 为 optional

    * [Key](https://ifttt.com/services/maker_webhooks/settings)
      * 在 URL 在 https://maker.ifttt.com/use/ 之后的文字

      ![step2-5-1](./img/blockly/step2-5-1.png)
    * event
      * 先前在IFTTT中创建的事件 *temp_data_generate*
    * value1 [*,value2, value3*]

      > 填入欲传输的数据 与 IFTTT Action中设置的 **ingredient** 相呼应

      * 假设欲传入的数据为主控板上传来的数据
      则 在[积木功能栏](https://cocorobolabs.gitbooks.io/cocoblockly/content/chapter-1-introduction.html)中选择
      以下四个块
        * WiFi Module -> Data transfer -> Data transfer setup
        * WiFi Module -> Data transfer -> Receive data to [dataIn], data length [1]
        * Lists -> set [item] to create text with [1,1,1]
        * Lists -> from [item] get item at [0]
      * 将 set [item] to create text with [1,1,1] 与  from [item] get item at [0] **拼接** 在一起，然后再同if is connected **拼接** 在一起
      * 将 [item] 变量修改为 [**dataIn**]
      ![step2-5-2](./img/blockly/step2-5-2.png)

      上述步骤将Main controller 生成的数据通过数组的方式传送到WiFi板中
* 步骤三
  * 设置时间
    * 在[积木功能栏](https://cocorobolabs.gitbooks.io/cocoblockly/content/chapter-1-introduction.html)中选择 Time -> wait [1000] milliseconds
    并将其拼接在 if connected do 块的 底下.

    ![step3-1](./img/blockly/step3-1.png)

## 结果查看
  此时打开Google sheets便可看到新生成的数据

  ![step1-1](./img/googleSheets/step1-1.png)
