
DN，Distinguished Name ：特有别名 唯一 
CN=Common Name 为用户名或服务器名，最长可以到80个字符，可以为中文；
OU=Organization Unit为组织单元，最多可以有四级，每级最长32个字符，可以为中文；
O=Organization 为组织名，可以3—64个字符长
C=Country为国家名，可选，为2个字符长

例如：CN=test,OU=developer,DC=domainname,DC=com 
在上面的代码中 cn=test 可能代表一个用户名，ou=developer 代表一个 active directory 中的组织单位。这句话的含义可能就是说明 test 这个对象处在domainname.com 域的 developer 组织单元中。



```
dn: ou=xiicloud,dc=example,dc=org
objectclass: top
objectclass: organizationalUnit
ou: xiicloud



# shixiaobo, example.org
dn: cn=xiaobo,dc=example,dc=org
objectClass: inetOrgPerson
cn: xiaobo
sn: shixiaobo
title: the world's most famous mythical manager
mail: sxb@xii.cloud
```

调整shixiaobo到xiicloud下

```
dn: cn=xiaobo,dc=example,dc=org
changetype: modrdn
newrdn: uid=shixiaobo
deleteoldrdn: 0
newsuperior: ou=xiicloud,dc=example,dc=org
```






