---
description: 并不是所有自动化测试都能通过IDE直接debug，特别是你刚接手一个新的项目时。远程调试本地的 mvn test，是个不错的临时方案。
---

# 如何远程调试 mvn test

附加 -Dmaven.surefire.debug 参数运行 mvn test

```text
mvn -Dmaven.surefire.debug test
```

稍等一会，你会看到如下输出。程序自动暂停并等待调试器连接。

```text
Listening for transport dt_socket at address: 5005
```

随后，通过你喜欢的 IDE 连接到 mvn test。例如在 IntelliJ IDEA 中，你可以新建一个 Remote  的 Configuration。

![](.gitbook/assets/image%20%2829%29.png)

如果要修改 debugger 端口，可以提供 address 参数给 `maven.surefire.debug` ，如下方所示：

```text
mvn -Dmaven.surefire.debug="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=8000 -Xnoagent -Djava.compiler=NONE" test
```



