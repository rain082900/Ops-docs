#### **1.通过部署平台替换**

a.下载最新license文件

使用个人公司账号 登录并下载![](/assets/3.png)

b.依据项目交付文档中标明的应用部署平台地址，打开应用部署平台，找到许可管理

![](/assets/D8TWT5~MFPQQ3`{E_LKSEJF.png)

c.管理当前租户共用的许可凭证 上传license许可![](/assets/TIM图片20180702094121.png)

d.点击上传License许可,输入备注信息，上传license文件

![](/assets/22.png)

e.上传后刷新页面，出现之前填写的备注信息，替换license更新成功![](/assets/23.png)

#### **2.针对run.sh部署的项目，通过本地目录替换**

a.进入公司网站,下载划红线最新license

![](/assets/1.png)点击文档

b. 通知![](/assets/2.png)c.license下载![](/assets/3.png)

d.更新替换license.lic

部署docker服务器主机目录下的/opt/aip-license/conf/license.lic以及/usr/license/license.lic文件， 替换为上面下载的license.lic 文件并且文件名保持一致，替换更新后应用不用重启.

