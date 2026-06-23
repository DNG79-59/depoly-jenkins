# depoly-jenkins
记录是如何安装jenkins和配置镜像
## Docker安装，docker-compose管理，其docker-compose.yml文件如下
```yml
version: '3.8'

services:
  jenkins:
    # 改用官方最新LTS JDK17，抛弃华为缓存旧镜像
    image: jenkins/jenkins:lts-jdk17
    container_name: jenkins
    restart: always
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - /data/var/jenkins_home:/var/jenkins_home
    environment:
      - TZ=Asia/Shanghai
      # 清华插件源，升级后下载插件更快
      - JENKINS_UC=https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates
      - JENKINS_UC_DOWNLOAD=https://mirrors.tuna.tsinghua.edu.cn/jenkins
```

## 插件处理如下
1.进入：http://localhost:8080/pluginManager/advanced

2.拉到页面最下面，把https改成http。这里其实就已经可以重启jenkins，使用了。但是为了更方便使用，我们可以设置镜像源，下载插件更快速！

3.修改上面的URL地址为国内镜像源：选择下面任何一个即可！
```
https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```
```
http://mirror.esuni.jp/jenkins/updates/update-center.json
```
```
http://mirror.xmission.com/jenkins/updates/update-center.json
```
图片[[3]]<img width="601" height="191" alt="image" src="https://github.com/user-attachments/assets/e914280d-fcb1-4cb8-8a9a-3ab93a1857b7" />

4.工作目录/updates/default.json设置

updates.jenkins.io/download或updates.jenkins-ci.org全部替换成选择的镜像源mirrors.tuna.tsinghua.edu.cn/jenkins  

`www.google.com`全部替换成`www.baidu.com`或其它能打开的网页  

<img width="670" height="347" alt="image" src="https://github.com/user-attachments/assets/73c5245e-d206-464f-867e-71684bcaeaee" />

5.保存后，重启Jenkins。ok!

注意：使用https时，需要跳过ssl证书认证，不然可能会失败！如下操作  
就是手动安装一个ID: skip-certificate-check插件，  下载地址(https://github.com/DNG79-59/depoly-jenkins/blob/main/skip-certificate-check.hpi)
<img width="509" height="243" alt="image" src="https://github.com/user-attachments/assets/46555a6e-5545-452a-bd7e-d5bd6ce7e947" />  
然后通过jenkins手动安装插件，重启。然后就可以更新了。
