## 操作场景
云数据库 SQL Server 支持重置实例密码，在使用云数据库 SQL Server 过程中，如果忘记数据库帐号密码，可以通过控制台重新设置密码。

>?
>- 云数据库 SQL Server 的重置密码功能已纳入 [CAM](https://intl.cloud.tencent.com/document/product/236/14469) 权限管理，建议对重置密码接口或云数据库 SQL Server 实例敏感资源权限收紧，只授权给应该授权的人员。
>- 为了数据安全，建议您定期更换密码，最长间隔不超过3个月。


## 操作步骤
1. 登录 [SQL Server 控制台](https://console.cloud.tencent.com/sqlserver)，在实例列表，单击实例名或“操作”列的【管理】，进入实例管理页面。
2. 选择【帐号管理】页，选择所需帐号，在“操作”列单击【重置密码】。
![](https://main.qcloudimg.com/raw/77a9ce4d8ec4ea21ebea829d6d3d3f18.png)
3. 在重置密码对话框，输入新密码和确认密码，单击【确定】即可完成密码重置。
>?数据库密码需要8-32个字符，至少包含英文、数字和符号 _+-&=!@#$%^*()[] 中的2种。

