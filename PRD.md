# 民商事司法协助系统登录认证产品需求文档 (PRD)

## 6. 详细功能说明

### 6.1 登录页

#### 6.1.1 邮箱、密码、图片验证码登录

| 字段               | 说明                                                                                        |
| ------------------ | ------------------------------------------------------------------------------------------- |
| **功能编号** | AUTH-01 / AUTH-02 / AUTH-03 / AUTH-04                                                       |
| **功能描述** | 用户输入邮箱、密码、图片验证码并点击 LOG IN，系统完成基础登录校验，通过后进入邮箱验证码步骤 |
| **前置条件** | 用户访问登录页，账号存在且未被禁用                                                          |
| **优先级**   | P0                                                                                          |

**页面元素**：

| 元素              | 类型   | 说明             | 校验规则                           |
| ----------------- | ------ | ---------------- | ---------------------------------- |
| Email address     | 输入框 | 用户登录邮箱     | 必填；需符合邮箱格式               |
| Password          | 密码框 | 用户登录密码     | 必填；生产环境后端校验密码正确性   |
| Verification code | 输入框 | 图片验证码       | 必填；生产环境后端校验验证码正确性 |
| 验证码图片        | 图片   | 展示图片验证码   | 应支持刷新                         |
| LOG IN            | 按钮   | 提交登录基础校验 | 点击后触发校验                     |

**交互逻辑**：

1. 用户输入邮箱、密码、图片验证码。
2. 用户点击 `LOG IN`。
3. 系统校验邮箱格式、密码非空、图片验证码非空。
4. 生产环境后端继续校验账号状态、密码正确性、图片验证码正确性。
5. 校验通过后，系统发送邮箱验证码并进入邮箱验证码页。
6. 校验失败时，停留当前页并展示对应错误提示。

**异常处理**：

| 异常场景             | 处理方式                               |
| -------------------- | -------------------------------------- |
| 邮箱为空或格式错误   | 提示用户输入有效邮箱地址               |
| 密码为空             | 提示用户输入密码                       |
| 图片验证码为空或错误 | 提示用户输入正确图片验证码             |
| 账号不存在或密码错误 | 提示账号或密码错误，不暴露账号是否存在 |
| 账号被禁用           | 提示账号不可用并联系管理员             |

### 6.2 邮箱验证码页

#### 6.2.1 验证码发送与倒计时

| 字段               | 说明                                                                                            |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| **功能编号** | MFA-01 / MFA-02 / MFA-03 / MFA-07 / MFA-08                                                      |
| **功能描述** | 系统向用户邮箱发送验证码，页面脱敏展示发送目标邮箱，发送按钮进入 60 秒倒计时，验证码 5 分钟有效 |
| **前置条件** | 登录基础校验通过                                                                                |
| **优先级**   | P0                                                                                              |

**页面元素**：

| 元素       | 类型     | 说明                       | 校验规则                             |
| ---------- | -------- | -------------------------- | ------------------------------------ |
| 发送提示   | 文本     | 展示已向指定邮箱发送验证码 | 邮箱需与登录邮箱一致；展示时必须脱敏 |
| 有效期提示 | 文本     | 展示验证码 5 分钟内有效    | 固定规则                             |
| Send code  | 按钮     | 重新发送邮箱验证码         | 发送后禁用 60 秒                     |
| 倒计时     | 文本状态 | 显示剩余秒数               | 从 60s 递减到 0s                     |

**交互逻辑**：

1. 用户通过基础校验后，系统自动发送邮箱验证码。
2. ***页面展示“已向邮箱发送验证码”，例如 `a***g@example.com`。***
   1. 邮箱内容：

      1. 法语：
         ```
         Bienvenue sur le système d'entraide judiciaire en matière civile et commerciale !
         Votre code de vérification est %s.
         Centre international de coopération juridique (ILCC)
         ----------------------------------------------------------------------------------------------------------------------------------------------------
         À propos de nous :
         Le Ministère de la Justice de la République populaire de Chine est l'Autorité centrale chinoise désignée par le gouvernement chinois pour mettre en œuvre la Convention de La Haye sur la signification, la Convention de La Haye sur l'obtention des preuves et 38 traités d'entraide judiciaire en matière civile et commerciale. Actuellement, le Centre international de coopération juridique (ILCC), placé sous l'égide du Ministère de la Justice, exerce les fonctions d'Autorité centrale chargée de l'entraide judiciaire en matière civile et commerciale.
         Ses principales fonctions sont les suivantes :
         Recevoir, examiner, transmettre et répondre aux diverses demandes d'entraide judiciaire en matière civile et commerciale, et assurer la coordination et la communication avec la partie requérante étrangère et les services exécutifs chinois pendant l'exécution des demandes.
         Participer aux négociations des traités d'entraide judiciaire en matière civile et commerciale et des conventions connexes.
         Assister aux réunions d'examen de la mise en œuvre des conventions pertinentes.
         Entretenir des échanges et coopérer avec les autorités centrales de divers pays pour les affaires judiciaires civiles et commerciales.
         Fournir des services de traduction en chinois (payants) sur demande.
         Nous contacter
         Adresse : 41A, Avenue Ouest de Ping’anli, District de Xicheng, Beijing, 100035, Chine
         Tél. : +86 10 6515 2763
         Fax : +86 10 6515 2773
         ```
   2. 俄语：

      ```
      Добро пожаловать в Систему правовой помощи по гражданским и торговым делам!
      Ваш код подтверждения: %s.
      Центр международного правового сотрудничества (International Legal Cooperation Center, ILCC)
      ----------------------------------------------------------------------------------------------------------------------------------------------------
      О нас:
      Министерство юстиции Китайской Народной Республики является Центральным органом Китайской Народной Республики, назначенным Правительством Китая для выполнения функций по применению Гаагской конвенции о вручении за границей судебных и внесудебных документов по гражданским или торговым делам, Гаагской конвенции о получении за границей доказательств по гражданским или торговым делам, а также 38 договоров о правовой помощи по гражданским и торговым делам. В настоящее время Центр международного правового сотрудничества (International Legal Cooperation Center, ILCC) при Министерстве юстиции осуществляет функции Центрального органа по вопросам правовой помощи по гражданским и торговым делам.
      К основным функциям относятся:
      прием, рассмотрение, направление и исполнение различных запросов о правовой помощи по гражданским и торговым делам, а также координация и взаимодействие с иностранной запрашивающей стороной и компетентными органами Китая в ходе исполнения таких запросов; 
      Участие в переговорах по заключению договоров и соответствующих конвенций о правовой помощи по гражданским и коммерческим делам.
      Участие в переговорах по договорам о правовой помощи по гражданским и торговым делам и соответствующим конвенциям; участие в заседаниях по вопросам рассмотрения исполнения соответствующих конвенций; 
      Осуществление обменов и сотрудничества с центральными органами различных государств по вопросам гражданских и торговых дел; 
      Оказание платных услуг по переводу на китайский язык по запросу.
      Контактная информация
      Адрес: 100035 Китай, Пекин, район Сичэн ул. Пинъаньли Сидацзе, д. 41А 
      Тел.: +86 10 6515 2763
      Факс: +86 10 6515 2773

      ```
   3. 英语

      ```
      Welcome to log in to the Civil and Commercial Judicial Assistance System! 
      Your verification code is %s.

      International Legal Cooperation Center (ILCC)
      ----------------------------------------------------------------------------------------------------------------------------------------------------
            About us：
      The Ministry of Justice of the People's Republic of China is the Central Authority of China designated by the Chinese government to implement the Hague Service Convention, the Hague Evidence Convention and 38 civil and commercial judicial assistance treaties. At present, the International Legal Cooperation Center(ILCC) under the Ministry of Justice performs the functions of the Central Authority in charge of judicial assistance in civil and commercial matters.
       
            Main functions include:
        Receiving, reviewing, transmitting and responding to various civil and commercial judicial assistance requests, and coordinating and communicating with the foreign requesting party and Chinese executive departments during the execution of the requests.
         Participating in the negotiation of civil and commercial judicial assistance treaties and related conventions.
           Attending the relevant convention performance review meetings.
          Carrying out exchanges and cooperating with the central authorities of various countries for civil and commercial judicial matters.
          Providing (paid) Chinese translation services per requested.
            Contact us
      Address: No. 41A, PingAnLi West Ave., Xicheng District, Beijing 100035, China
      Tel: +86 10 6515 2763
      Fax: +86 10 6515 2773

      ```
3. `Send code` 按钮禁用并显示 `60s` 倒计时。
4. 倒计时结束后按钮恢复可点击。
5. 用户点击重新发送后，生成新验证码并重新进入 60 秒倒计时。
6. 新验证码发送成功后，使上一条未使用验证码失效。

**异常处理**：

| 异常场景     | 处理方式                                 |
| ------------ | ---------------------------------------- |
| 邮件发送失败 | 提示发送失败，请稍后重试                 |
| 邮箱不可达   | 提示邮箱发送异常，建议联系管理员         |
| 用户刷新页面 | 后端保留验证码有效期，前端恢复当前倒计时 |

#### 6.2.2 验证码校验并登录

| 字段               | 说明                                                                                 |
| ------------------ | ------------------------------------------------------------------------------------ |
| **功能编号** | MFA-04 / MFA-05 / MFA-06 / HOME-01                                                   |
| **功能描述** | 用户输入邮箱验证码并点击校验并登录；验证码正确、未过期、未使用时登录成功进入系统首页 |
| **前置条件** | 邮箱验证码已成功发送                                                                 |
| **优先级**   | P0                                                                                   |

**页面元素**：

| 元素                    | 类型   | 说明                               | 校验规则       |
| ----------------------- | ------ | ---------------------------------- | -------------- |
| Email verification code | 输入框 | 用户输入邮箱验证码                 | 必填；6 位数字 |
| VERIFY AND LOG IN       | 按钮   | 校验验证码并登录                   | 点击后触发校验 |
| 错误提示                | 文本   | 展示验证码错误、过期、已使用等状态 | 给出错误提示   |

**交互逻辑**：

1. 用户输入邮箱验证码。
2. 用户点击 `VERIFY AND LOG IN`。
3. 系统校验验证码是否存在、是否属于当前邮箱、是否在 5 分钟有效期内、是否未使用、是否匹配。
4. 验证通过后，系统以原子操作标记验证码已使用。
5. 系统建立登录态并进入系统首页。
6. 验证失败时停留当前页并提示错误。

**异常处理**：

| 异常场景         | 处理方式                                      |
| ---------------- | --------------------------------------------- |
| 验证码为空       | 提示请输入邮箱验证码                          |
| 验证码错误       | 提示验证码错误                                |
| 验证码过期       | 提示验证码已过期，请重新发送                  |
| 验证码已使用     | 提示验证码错误                                |
| 重复点击校验按钮 | 后端保证幂等；前端按钮进入 loading 防重复提交 |

### 6.3 系统首页

#### 6.3.1 登录成功进入首页

| 字段               | 说明                                                   |
| ------------------ | ------------------------------------------------------ |
| **功能编号** | HOME-01 / HOME-02 / HOME-03                            |
| **功能描述** | 用户完成邮箱验证码校验后进入系统首页，系统建立安全会话 |
| **前置条件** | 邮箱验证码校验成功                                     |
| **优先级**   | P0                                                     |

**交互逻辑**：

1. 邮箱验证码校验成功。
2. 系统写入安全登录态。
3. 前端进入系统首页。
4. 后续业务接口根据登录态做权限校验。

## 7. 业务规则

| 规则编号 | 规则内容                                                                                                                                                                           | 优先级 |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| RULE-01  | 邮箱验证码长度建议为 6 位数字                                                                                                                                                      | P0     |
| RULE-02  | 邮箱验证码有效期为 5 分钟                                                                                                                                                          | P0     |
| RULE-03  | 发送邮箱验证码后，发送按钮 60 秒内不可再次点击                                                                                                                                     | P0     |
| RULE-04  | 同一个邮箱验证码校验成功后必须立即标记为已使用                                                                                                                                     | P0     |
| RULE-05  | 已使用验证码不可二次使用，即使仍在 5 分钟有效期内                                                                                                                                  | P0     |
| RULE-06  | 用户重新发送验证码后，使该邮箱上一条未使用验证码失效                                                                                                                               | P1     |
| RULE-07  | 页面展示发送目标邮箱时必须脱敏：***@前面的字符串：******长度 1-2 位展示“首字符 +***”，3-6 位展示“首字符 + *** + 末字符”，大于 6 位展示“前 3 位 + *** + 后 2 位”*** | P0     |
| RULE-08  | 生产环境不得在页面展示邮箱验证码明文                                                                                                                                               | P0     |

### 7.1 邮箱脱敏规则示例

| 原邮箱                     | 脱敏后                   |
| -------------------------- | ------------------------ |
| `a@example.com`          | `a***@example.com`     |
| `ab@example.com`         | `a***@example.com`     |
| `abc@example.com`        | `a***c@example.com`    |
| `alice@example.com`      | `a***e@example.com`    |
| `alice.wang@example.com` | `ali***ng@example.com` |
