# Jenkins_SharedLibrary
## first learning the jenkins autodeploy

# 流程说明
## 第一步
#```Dev push ->  Git/Svn  *trigger*  Build = Package```

## 第二步
#```If Package:  push  registry ->>> [nexus]```
#```if docker image Push registry -->>> [harbor]```

## 第三步 
#```if deploy !k8s: use script || run package To At server```
#```if deploy k8s:  use yaml || run package To At Pod```

#第四步
##健康检查
## 服务监控