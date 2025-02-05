# 免费注册与续订 E5 开发者订阅，畅享 Office 365 全家桶

![Office 365](https://bbtdd.com/img/37938180437.webp)

随着微软将 AI 功能整合到 Office 365 全家桶，这款办公工具的生产力得到了进一步提升。与买断制的 Office 2022 不同，Office 365 采用订阅制，需要每年付费。不过，微软为开发者提供了 **E5 开发者订阅**，可以免费使用 Office 365，并享受 5TB 的 OneDrive 存储空间。只要定期续订，即可长期免费使用。

E5 开发者订阅是微软为开发者推出的福利，每次有效期为 90 天。如果未能在 90 天内完成续订，账户将被注销。为了确保续订成功，可以通过调用 API 等开发行为来保持账户活跃。此外，每个订阅最多可支持 25 个账户，每个账户可同时在五台设备上登录 Office 365，并拥有 5TB OneDrive 存储空间（可用于搭建分享网盘）。

**注意：任何续订方法都无法保证 100% 成功，建议不要使用 OneDrive 存储重要数据！若账户被注销，可以重新注册一个。**

---

## 一、注册开发者账户

1. 访问 [Microsoft 365 开发者计划](https://developer.microsoft.com/en-us/microsoft-365/dev-program)，使用微软账号登录并注册成为开发者。
2. 建议选择中国作为国家（OneDrive 速度更快），公司名称随意，语言选择适合的即可。
3. 在注册过程中，选择 **Configurable Sandbox**，国家依然建议选择中国，域名、名称和密码可根据个人喜好设置。
4. 注册成功后，系统会生成一个格式为 `xxx@xxx.onmicrosoft.com` 的账号，记住你的账号信息。
5. 完成手机号验证后，即可查看账户订阅剩余天数。此时，账户注册已完成，你可以根据需要调整 OneDrive 的存储容量。

---

## 二、添加 API 以实现自动续订

1. 登录 [Azure 门户](https://portal.azure.com/#home)，使用刚注册的 `onmicrosoft` 账号登录。
2. 在搜索框中输入并点击 **应用注册**，然后点击左上角的 **新注册**。
3. 填写应用名称，选择 **任何组织目录中的账户** 作为受支持的账户类型，无需填写重定向 URL，点击注册。
4. 注册完成后，复制左侧的 **应用程序 (客户端) ID**，然后点击右侧的 **添加重定向 URL**，选择 **移动和桌面应用程序**，并配置重定向 URL。
5. 打开 **允许公共客户端** 选项并保存。
6. 在左侧菜单栏中选择 **API 权限**，点击 **添加权限**，选择 **Microsoft Graph**，并添加以下 API 权限：
   plaintext
   BookingsAppointment.ReadWrite.All;
   Calendars.Read
   Contacts.Read
   Directory.Read.All
   Files.Read.All
   Files.ReadWrite.All
   Group.Read.All
   Mail.Read
   Mail.Send
   MailboxSettings.Read
   Notes.Read.All
   People.Read.All
   Presence.Read.All
   Sites.Read.All
   Tasks.ReadWrite 
   User.Read.All
   
7. 点击 **代表管理员同意**，在弹出的确认框中点击 **是**，几秒后即可看到所有权限已成功授予。

---

## 三、部署自动续订服务

1. 使用 Docker 部署自动续订服务（具体安装教程可参考相关文档）。
   bash
   docker run -d -p 1066:1066 -e TZ=Asia/Shanghai -v /root/Docker_Microsoft365_E5_Renew_X/Microsoft365_E5_Renew_X/app:/app hanhongyong/ms365-e5-renew-x:slim
   
2. 如果是威联通 NAS，在 Docker 中搜索 `hanhongyong/ms365-e5-renew-x`，选择 `slim` 版本（ARM 设备请选择 ARM 版），添加端口映射、环境变量，并映射文件夹到 `/app`，最后创建容器。

---

## 四、配置自动续订服务

1. 在浏览器中输入 `IP + 1066`，进入管理面板，默认密码为 `123456`。
2. 登录后，点击 **添加账号**，输入你的 `onmicrosoft` 账号、密码以及之前获取的 **客户端 ID**，选中 **登录调用**，点击 **添加运行账号**。
3. 如果配置正确，即可看到 API 被成功调用。之后，你可以在账户快过期前几天收到续订邮件。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bbtdd.com/WildCard)

---

通过以上步骤，你可以免费获得 Office 365 的使用权限，并实现自动续订。记得定期检查账户状态，确保续订成功！