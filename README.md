# WUTNote
吾理Note——简单易用的校园笔记分享平台

# 数据库设计文档

## 客户端

### 1. user

user表为用户表，存储用户信息
username/password和openid暂时先都保留，取决于学校是否提供学生学号信息

| 字段名      | 数据类型     | 说明                   | 备注        |
| ----------- | ------------ | ---------------------- | ----------- |
| id          | bigint       | 主键                   | 自增        |
| username    | varchar(32)  | 用户名（学号）         | 唯一        |
| pwd         | varchar(64)  | 密码                   |             |
| openid      | varchar(45)  | 微信用户唯一标识       | 唯一        |
| nickname    | varchar(10)  | 昵称，不超过10字符     |             |
| avator      | varchar(255) | 头像路径               |             |
| signature   | varchar(20)  | 个性签名，不超过20字符 |             |
| state       | int          | 账号状态               | 1正常 0封号 |
| create_time | datetime     | 注册时间               |             |
| update_time | datetime     | 修改时间               |             |


### 2. note

note表为笔记表，存储笔记相关信息

| 字段名      | 数据类型          | 说明            | 备注        |
| ----------- | ----------------- | --------------- | ----------- |
| id          | bigint            | 主键            | 自增        |
| user_id     | bigint            | 用户(作者)id    | 逻辑外键    |
| title       | varchar(20)       | 标题            |             |
| abstract    | varchar(20)       | 笔记摘要        |             |
| content     | text/varchar(255) | 笔记内容/云路径 |             |
| browse_num  | bigint            | 浏览数          |             |
| favor_num | bigint            | 点赞数          |             |
| collect_num | bigint            | 收藏数          |             |
| visi | int               | 可见权限        | 1公开 0私有 |
| create_time | datetime          | 发表时间        |             |
| update_time | datetime     | 修改时间               ||
### 3.tag

tag为标签表

| 字段名  | 数据类型    | 说明     | 备注 |
| ------- | ----------- | -------- | ---- |
| id      | bigint      | 主键     | 自增 |
| tagname | varchar(10) | 标签名称 |      |




### 4. note_tag

note_tag为笔记标签表

| 字段名  | 数据类型 | 说明   | 备注     |
| ------- | -------- | ------ | -------- |
| id      | bigint   | 主键   | 自增     |
| note_id | bigint   | 笔记id | 逻辑外键 |
| tag_id  | bigint   | 标签id | 逻辑外键 |



### 5. comment

comment为评论表

| 字段名      | 数据类型     | 说明           | 备注     |
| ----------- | ------------ | -------------- | -------- |
| id          | bigint       | 主键           | 自增     |
| note_id     | bigint       | 笔记id         | 逻辑外键 |
| user_id     | bigint       | 用户(评论者)id | 逻辑外键 |
| content     | varchar(255) | 评论内容       |          |
| create_time | datetime     | 评论时间       |          |
| favor       | bigint       | 点赞数         |          |
| sub_num     | bigint       | 子评论数       |          |

### 6. subcomment

subcomment为子评论表

| 字段名      | 数据类型     | 说明           | 备注     |
| ----------- | ------------ | -------------- | -------- |
| id          | bigint       | 主键           | 自增     |
| comment_id  | bigint       | 主评论id       | 逻辑外键 |
| user_id     | bigint       | 用户(评论者)id | 逻辑外键 |
| content     | varchar(255) | 评论内容       |          |
| create_time | datetime     | 评论时间       |          |
| favor_num   | bigint       | 点赞数         |          |

### 7. collect

collect【个人】用户收藏笔记表

| 字段名      | 数据类型 | 说明       | 备注     |
| ----------- | -------- | ---------- | -------- |
| id          | bigint   | 主键       | 自增     |
| user_id     | bigint   | 用户id     | 逻辑外键 |
| note_id     | bigint   | 收藏笔记id | 逻辑外键 |
| create_time | datetime | 收藏时间   |          |

### 8. concern

concern【个人】用户关注表

| 字段名   | 数据类型 | 说明         | 备注     |
| -------- | -------- | ------------ | -------- |
| id       | bigint   | 主键         | 自增     |
| user_id  | bigint   | 用户id       | 逻辑外键 |
| user2_id | bigint   | 关注的用户id | 逻辑外键 |


### 9. column

column 【个人】专栏表

| 字段名   | 数据类型     | 说明     | 备注     |
| -------- | ------------ | -------- | -------- |
| id       | bigint       | 主键     | 自增     |
| user_id  | bigint       | 用户id   | 逻辑外键 |
| col_name | varchar(10)  | 专栏名称 |          |
| abstract | varchar(20)  | 专栏简介 |          |
| pic      | varchar(255) | 专栏图片 |          |
| note_num | bigint       | 文章数量 |          |

### 10.note_column

note_column笔记-专栏表：一个笔记可能归属多个专栏，一个专栏有多个笔记

| 字段名    | 数据类型 | 说明   | 备注     |
| --------- | -------- | ------ | -------- |
| id        | bigint   | 主键   | 自增     |
| note_id   | bigint   | 笔记id | 逻辑外键 |
| column_id | bigint   | 专栏id | 逻辑外键 |




## 管理端

### admin

admin为管理员表
管理员能更改用户账号状态、删除笔记、评论

| 字段名   | 数据类型    | 说明   | 备注 |
| -------- | ----------- | ------ | ---- |
| id       | bigint      | 主键   | 自增 |
| username | varchar(20) | 用户名 | 唯一 |
| pwd      | varchar(20) | 密码   |      |
| nickname | varchar(20) | 昵称   |      |

