# openstreetmap注意事项

## Environment

需要Ubuntu 22.04，参考[DockerFile](Dockerfile)或者[INSTALL steps](INSTALL.md)配置环境

- ruby
- gem
- bundle
- nodejs
- npm

## Initial Setup

```
cp config/example.storage.yml config/storage.yml
cp config/docker.database.yml config/database.yml
touch config/settings.local.yml
```

## Migrations （需要时）

`bin/rails db:migrate`

## 当页面（地图页和编辑页）出问题时

- 国际化问题：`rake i18n:js:export`??
- css样式问题：`bin/rails assets:precompile`

## 注册和忘记密码

注册成功或者找回密码时候，表单提交后，控制台会打印邮件内容。

```html
 <h1>注册验证链接：</h1>
<p style="color: black; margin: 0.75em 0; font-family: 'Helvetica Neue', Arial, sans-serif">
  <a
    href="http://127.0.0.1:3000/user/zhaoyuehai5282/confirm?confirm_string=kDyMhbLOKvm3jzUQeYD19Z6ePz9PYH">
    http://127.0.0.1:3000/user/zhaoyuehai5282/confirm?confirm_string=kDyMhbLOKvm3jzUQeYD19Z6ePz9PYH
  </a>
</p>
<h1>找回密码验证链接：</h1>
<p style="color: black; margin: 0.75em 0; font-family: 'Helvetica Neue', Arial, sans-serif">
  <a href="http://127.0.0.1:3000/user/reset-password?token=wAyq4Pgi7CiyXA407cc5JkJlCRugof">
    http://127.0.0.1:3000/user/reset-password?token=wAyq4Pgi7CiyXA407cc5JkJlCRugof
  </a>
</p>
```

## 地图编辑

需要预先添加OAuth Consumer Keys，参考[configuration steps](CONFIGURE.md)

### 1.注册新应用程序：（点击用户名弹框 > 我的设置 > OAuth2 应用程序）
  - 名称：OpenStreetMap Web Site（随意定义）
  - 重定向URI：（多个URL时需要用空格或者回车分开；路径后面带“:8080/land.html”的URL是为单独部署的iD编辑器web项目而配置的）
    ```
    http://192.168.93.163
    http://192.168.93.163:8080/land.html
    http://192.168.93.168
    http://192.168.93.168:8080/land.html
    ```
  - 勾选相关权限，点击注册
  - 注册成功后会返回客户端ID和客户密钥

### 2.复制客户端ID到config/settings.local.yml
    ```
    # Default editor
    default_editor: "id"
    # OAuth 2 Client ID for iD
    id_application: "evkUSXTAwTOVdXZhLXXdr5TmnwuOcea2SbiUvxqvGi8"
    ```

## 运行命令

- `bundle exec rails s -p 3000 -b '0.0.0.0'`（默认Environment: development）

- `bundle exec rails s -e production -p 3000 -b '0.0.0.0'`（指定Environment: production）

  注意：production环境运行时需要credentials否则报异常：Missing `secret_key_base` for 'production' environment, set this
  string with `bin/rails credentials:edit`

## docker部署运行

```
docker-compose build
docker-compose up -d

docker-compose run --rm web bundle exec rails db:migrate # 需要时
docker-compose run --rm web bundle exec rails assets:precompile # 需要时
```
