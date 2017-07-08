# git账户配置以及多账户配置

## 使用情境
个人和公司,针对一平台有多个git帐户,在使用时难免要区分登陆

## ssh-keygen以下配置

```
ssh-keygen -t rsa -f ~/.ssh/id_rsa.aliyun_tom -C 'tom@qq.com'
ssh-keygen -t rsa -f ~/.ssh/id_rsa.aliyun_garry -C 'garry@qq.com'
```

## ssh config 配置
vi ~/.ssh/config

```
Host aliyun_tom
    HostName code.aliyun.com
    IdentityFile ~/.ssh/id_rsa.tom
    User tom
    PreferredAuthentications publickey

Host aliyun_garry
    HostName code.aliyun.com
    IdentityFile ~/.ssh/id_rsa.garry
    PreferredAuthentications publickey
    User garry
```

## 全局设置一次
如果你只是通过这篇文章中所述配置了Host，那么你多个账号下面的提交用户会是一个人，所以需要通过命令

```
git config --global --unset user.name
git config --global --unset user.email
```

删除用户账户设置，在每一个repo下面使用命令单独设置用户账户信息

```
git config --local user.name '你的名字'
git config --local user.email '你的邮箱@mail.com'
```


## 应用
```
git clone aliyun_garry:test/oa.git
```
