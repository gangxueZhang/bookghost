# Kubernetes命令介绍

[Kubernetes中文文档](https://www.kubernetes.org.cn/doc-45)

[Kubernetes中文文档](https://www.kubernetes.org.cn/doc-59)

[Kubernetes常用命令大全](https://debian.cn/articles/633?www)

## 查看命令

### kubectl 

**功能说明：**使用kubectl来管理Kubernetes集群

```shell
# 语法
$ kubectl
```

```shell
# 选项
		--add-dir-header=false: 如果为 true，则将文件目录添加到标题中
    --alsologtostderr[=false]: 同时输出日志到标准错误控制台和文件。
    --as='': 模拟操作的用户名
    --as-group=[]: 用于模拟组的操作，此标志可以重复指定多个组。
    --cache-dir='/root/.kube/http-cache': 默认HTTP缓存目录
    --api-version="": 和服务端交互使用的API版本。
    --certificate-authority="": 用以进行认证授权的.cert文件路径。
    --client-certificate="": TLS使用的客户端证书路径。
    --client-key="": TLS使用的客户端密钥路径。
    --cluster="": 指定使用的kubeconfig配置文件中的集群名。
    --context="": 指定使用的kubeconfig配置文件中的环境名。
    --insecure-skip-tls-verify[=false]: 如果为true，将不会检查服务器凭证的有效性，这会导致你的HTTPS链接变得不安全。
    --kubeconfig="": 命令行请求使用的配置文件路径。
    --log-backtrace-at=:0: 当日志长度超过定义的行数时，忽略堆栈信息。
    --log-dir="": 如果不为空，将日志文件写入此目录。
    --log-file='': 如果非空，请使用此日志文件
    --log-file-max-size=1800: 定义日志文件可以增长到的最大大小。单位是兆字节。如果值为0,最大文件大小没有限制。
    --log-flush-frequency=5s: 刷新日志的最大时间间隔。
    --logtostderr[=true]: 输出日志到标准错误控制台，不输出到文件。
    --match-server-version[=false]: 要求服务端和客户端版本匹配。
-n, --namespace="": 如果不为空，命令将使用此namespace。
    --password="": API Server进行简单认证使用的密码。
    --profile='none': 要捕获的配置文件的名称。(none|cpu|heap|goroutine|threadcreate|block|mutex)
    --profile-output='profile.pprof': 要将配置文件写入到的文件名
    --request-timeout='0': 在放弃单个服务器请求之前等待的时间长度。 非零值
-s, --server="": Kubernetes API Server的地址和端口号。
		--skip-headers=false: 如果为 true，则避免在日志消息中使用标题前缀
    --skip-log-headers=false: 如果为 true，则在打开日志文件时避免使用标头
    --stderrthreshold=2: 高于此级别的日志将被输出到错误控制台。
    --token="": 认证到API Server使用的令牌。
    --user="": 指定使用的kubeconfig配置文件中的用户名。
    --username="": API Server进行简单认证使用的用户名。
    --v=0: 指定输出日志的级别。
    --vmodule=: 指定输出日志的模块，格式如下：pattern=N，使用逗号分隔。
```

### kubectl get获取资源

**功能说明：**获取列出一个或多个资源的信息

```shell
# 语法：
$ kubectl get (TYPE [NAME | -l label] | TYPE/NAME ...) [(-o|--output=)json|yaml|...]  [flags]
# 列出所有运行的Pod信息。
$ kubectl get pods
# 列出Pod以及运行Pod节点信息。
$ kubectl get pods -o wide
# 列出指定NAME的 replication controller信息。
$ kubectl get replicationcontroller web
# 以JSON格式输出一个pod信息。
$ kubectl get pod podName -n nameSpace -o json 
# 以“pod.yaml”配置文件中指定资源对象和名称输出JSON格式的Pod信息。
$ kubectl get -f pod.yaml -o json
# 返回指定pod的相位值。
$ kubectl get -o template pod/web-pod-13je7 --template={{.status.phase}}
# 列出所有replication controllers和service信息。
$ kubectl get rc,services
# 按其资源和名称列出相应信息。
$ kubectl get rc/web service/frontend pods/web-pod-13je7
# 列出所有不同的资源对象。
$ kubectl get all
```

```shell
# 选项
-A, --all-namespaces=false: 列出所有命名空间中请求的对象
		--allow-missing-template-keys=true: 如果为true，则在模板中缺少字段或映射键时忽略模板中的任何错误。仅适用于golang和jsonpath输出格式。
		--chunk-size=500: 分块返回大列表，而不是一次全部返回。传递0以禁用。此标志是测试版，将来可能会更改。
		--field-selector='': 要过滤的选择器（字段查询），支持 '='、'==' 和 '!='。（例如 --field-selector key1=value1,key2=value2）。 
-f, --filename=[]: 文件名、目录或 URL，用于标识要从服务器获取的资源的文件。
		--ignore-not-found=false: 如果请求的对象不存在，该命令将返回退出代码 0。
-k, --kustomize='': 处理自定义目录。 此标志不能与 -f 或 -R 一起使用。
-L, --label-columns=[]: 接受逗号分隔的标签列表，名字区分大小写。 多个标志选项使用：-L label1 -L label2
		--no-headers=false: 使用默认或自定义列输出格式时，不要打印标题（默认打印标题）。
-o, --output='': 输出格式。 之一：json|yaml|wide|name|custom-columns=...|custom-columns-file=...|go-template=...|go-template-file=...|jsonpath=...| jsonpath-文件=...
		--output-watch-events=false: 使用 --watch 或 --watch-only 时输出监视事件对象。 现有对象作为初始 ADDED 事件输出。
		--raw='': 从服务器请求的原始 URI。 使用 kubeconfig 文件指定的传输。
-R, --recursive=false: 递归处理 -f, --filename 中使用的目录。 当您想要管理在同一目录中组织的相关清单时很有用。
-l, --selector='': 要过滤的选择器（标签查询），支持 '='、'==' 和 '!='。（例如 -l key1=value1,key2=value2）
		--server-print=true: 如果为true，则让服务器返回适当的表输出。支持扩展API和CRD。
		--show-kind=false: 如果存在，请列出所请求对象的资源类型。
		--show-labels=false: 打印时，将所有标签显示为最后一列（默认隐藏标签列）
		--sort-by='': 如果非空，请使用此字段规范对列表类型进行排序。字段规范表示为JSONPath表达式（例如“{.metadata.name}”）
		--template='': -o=go-template, -o=go-template-file时使用的模板字符串或模板文件路径。
-w, --watch=false: 列出/获取请求的对象后，注意更改。 如果未提供对象名称，则排除未初始化的对象。
		--watch-only=false: 注意对请求对象的更改，而不是首先列出/获取。
```

### kubectl describe查看详细信息

**功能说明：**输出指定的一个/多个资源的详细信息。

```shell
# 语法
$ kubectl describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME) [options]
# 描述一个node
$ kubectl describe nodes kubernetes-minion-emt8.c.myproject.internal
# 描述一个pod
$ kubectl describe pods/nginx
# 描述pod.json中的资源类型和名称指定的pod
$ kubectl describe -f pod.json
# 描述所有的pod
$ kubectl describe pods
# 描述所有包含label name=myLabel的pod
$ kubectl describe po -l name=myLabel
# 描述所有被replication controller “frontend”管理的pod（rc创建的pod都以rc的名字作为前缀）
$ kubectl describe pods frontend
```

```shell
# 选项
-A, --all-namespaces=false: 列出所有命名空间中请求的对象
-f, --filename=[]: 文件名、目录或 URL，用于标识要从服务器获取的资源的文件。
-k, --kustomize='': 处理自定义目录。 此标志不能与 -f 或 -R 一起使用。
-R, --recursive=false: 递归处理 -f, --filename 中使用的目录。 当您想要管理在同一目录中组织的相关清单时很有用。
-l, --selector='': 要过滤的选择器（标签查询），支持 '='、'==' 和 '!='。（例如 -l key1=value1,key2=value2）
		--show-events=true: 如果为 true，则显示与所描述对象相关的事件。
```

### kubectl logs查看日志

**功能说明：** 输出pod中一个容器的日志。

```shell
# 语法
$ kubectl logs [-f] [-p] POD [-c CONTAINER]
# 返回仅包含一个容器的pod nginx的日志快照
$ kubectl logs nginx
# 返回pod ruby中已经停止的容器web-1的日志快照
$ kubectl logs -p -c ruby web-1
# 持续输出pod ruby中的容器web-1的日志
$ kubectl logs -f -c ruby web-1
# 仅输出pod nginx中最近的20条日志
$ kubectl logs --tail=20 nginx
# 输出pod nginx中最近一小时内产生的所有日志
$ kubectl logs --since=1h nginx
```

```shell
# 选项
-c, --container="": 容器名。
-f, --follow[=false]: 指定是否持续输出日志。
		--interactive[=true]: 如果为true，当需要时提示用户进行输入。默认为true。
		--limit-bytes=0: 输出日志的最大字节数。默认无限制。
-p, --previous[=false]: 如果为true，输出pod中曾经运行过，但目前已终止的容器的日志。
		--since=0: 仅返回相对时间范围，如5s、2m或3h，之内的日志。默认返回所有日志。只能同时使用since和since-time中的一种。
		--since-time="": 仅返回指定时间（RFC3339格式）之后的日志。默认返回所有日志。只能同时使用since和since-time中的一种。
		--tail=-1: 要显示的最新的日志条数。默认为-1，显示所有的日志。
		--timestamps[=false]: 在日志中包含时间戳。
```

### kubectl exec执行命令

**功能说明：** 在容器内部执行命令。

```shell
# 语法
$ kubectl exec (POD | TYPE/NAME) [-c CONTAINER] [flags] -- COMMAND [args...] [options]
# 默认在pod podName的第一个容器中运行“date”并获取输出
$ kubectl exec podName date
# 在pod podName的容器ruby-container中运行“date”并获取输出
$ kubectl exec podName -c ruby-container date
# 终端模式运行命令，将进入到容器中并执行bash命令，将其输出到控制台
$ kubectl exec -it podName -c ruby-container -- /bin/bash
# 终端模式运行命令，将进入到指定命名空间的容器中并执行bash命令，将其输出到控制台
$ kubectl -n namspace exec -it podName -c ruby-container -- /bin/bash
```

```shell
#选项
-c, --container="": 容器名。如果未指定，使用pod中的一个容器。
-p, --pod="": Pod名。
-i, --stdin[=false]: 将控制台输入发送到容器。
-t, --tty[=false]: 将标准输入控制台作为容器的控制台输入。
```



## 创建命令

### kubectl create创建资源

**功能说明：** 通过文件名或控制台输入，创建资源。

```shell
# 语法
$ kubectl create -f FILENAME [options]
# 使用pod.json文件创建一个pod
$ kubectl create -f ./pod.json
# 通过控制台输入的JSON创建一个pod
$ cat pod.json | kubectl create -f -
```

```shell
# 可选命令
clusterrole         创建集群角色.
clusterrolebinding  为特定的 ClusterRole 创建一个ClusterRoleBinding
configmap           从本地文件、目录或文字值创建配置映射
cronjob             创建具有指定名称的 cronjob。
deployment          创建具有指定名称的部署。
job                 创建具有指定名称的作业。
namespace           创建具有指定名称的命名空间
poddisruptionbudget 创建具有指定名称的 Pod 中断预算。
priorityclass       创建具有指定名称的优先级类。
quota               创建具有指定名称的配额。
role                创建具有单一规则的角色。
rolebinding         为特定角色或集群角色创建角色绑定
secret              使用指定的子命令创建一个秘密
service             使用指定的子命令创建服务。
serviceaccount      创建具有指定名称的服务帐户
#选项
		--allow-missing-template-keys=true: 如果为true，则在模板中缺少字段或映射键时忽略模板中的任何错误。仅适用于golang和jsonpath输出格式。
		--dry-run=false: 如果为 true，则只打印将要发送的对象，而不发送它。
		--edit=false: 在创建之前编辑 API 资源
-f, --filename=[]: 用于创建资源的文件名、目录或 URL
-k, --kustomize='': 处理自定义目录。 此标志不能与 -f 或 -R 一起使用。
-o, --output='': 输出格式。 其中之一：json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-file。
		--raw='': POST 到服务器的原始 URI。 使用 kubeconfig 文件指定的传输。
		--record=false: 在资源注解中记录当前kubectl命令。 如果设置为 false，则不记录命令。如果设置为true,则记录命令。
-R, --recursive=false: 递归处理 -f, --filename 中使用的目录。 当您想要管理在同一目录中组织的相关清单时很有用。
		--save-config=false: 如果为 true，则当前对象的配置将保存在其注释中。否则注释将保持不变。当您以后想在此对象上执行kubectl apply时，此标志很有用。
-l, --selector='': 要过滤的选择器（标签查询），支持 '='、'==' 和 '!='。（例如 -l key1=value1,key2=value2）
		--template='': -o=go-template, -o=go-template-file 时使用的模板字符串或模板文件路径。
		--validate=true: 如果为 true，则在发送之前使用架构来验证输入
		--windows-line-endings=false: 仅在 --edit=true 时相关。
```

## 更新命令

### kubectl replace替换资源

**功能说明：** 通过文件名或控制台输入替换资源。

```shell
# 语法
$ kubectl replace -f FILENAME [options]
# 使用pod.json中的数据替换 pod。
$ kubectl replace -f ./pod.json
# 根据传递到stdin的JSON替换pod。
$ cat pod.json | kubectl replace -f -
# 将单容器pod的镜像版本(标签)更新为v4
$ kubectl get pod mypod -o yaml | sed 's/\(image: myimage\):.*$/\1:v4/' | kubectl replace -f -
# 强制替换、删除然后重新创建资源
$ kubectl replace --force -f ./pod.json
```

```shell
#选项
    --allow-missing-template-keys=true: 如果为true，则在模板中缺少字段或映射键时忽略模板中的任何错误。仅适用于golang和jsonpath输出格式。
    --cascade=true: 如果为true，则级联删除此资源管理的资源(例如，由ReplicationController创建的Pod)。默认为真。
-f, --filename=[]: 用于替换资源。
    --force=false: 仅在宽限期=0时使用。如果为true，立即从API中删除资源并绕过正常删除。请注意，立即删除某些资源可能会导致不一致或数据丢失，需要确认。
    --grace-period=-1: 给予资源正常终止的时间段(以秒为单位)。如果为负则忽略。设置为1以立即关闭。只能在 --force为 true时设置为 0(强制删除)。
-k, --kustomize='': 处理自定义目录。 此标志不能与 -f 或 -R 一起使用。
-o, --output='': 输出格式。json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-file其中之一。
    --raw='': PUT 到服务器的原始 URI。 使用 kubeconfig 文件指定的传输。
-R, --recursive=false: 递归处理 -f, --filename中使用的目录。当您想要管理在同一目录中组织的相关清单时很有用。
    --save-config=false: 如果为 true，则当前对象的配置将保存在其注释中。否则，注释将保持不变。当您以后想在此对象上执行kubectl apply时，此标志很有用。
    --template='': -o=go-template, -o=go-template-file时使用的模板字符串或模板文件路径。模板格式为golang模板.
    --timeout=0s: 放弃删除之前等待的时间长度，零表示根据对象的大小确定超时
    --validate=true: 如果为 true，则在发送之前使用架构来验证输入。
    --wait=false: 如果为 true，则等待资源消失后再返回。 这等待终结者。
```

### kubectl apply资源配置

**功能说明：** 通过文件名或控制台输入，对资源进行配置。

```shell
# 语法
$ kubectl apply (-f FILENAME | -k DIRECTORY) [options]
# 将pod.json中的配置应用到pod。
$ kubectl apply -f ./pod.json
# 从包含kustomization.yaml的目录中应用资源 - 例如：目录/kustomization.yaml。
$ kubectl apply -k dir/
# 将传入stdin的JSON应用到pod。
$ cat pod.json | kubectl apply -f -
# 注意：--prune仍处于内部测试阶段
# 应用 manifest.yml中匹配标签app=nginx的配置，并删除文件中没有匹配标签app=nginx的所有其他资源。
$ kubectl apply --prune -f manifest.yaml -l app=nginx
# 应用manifest.yaml中的配置并删除文件中没有的所有其他配置映射。
$ kubectl apply --prune -f manifest.yaml --all --prune-whitelist=core/v1/ConfigMap
```

```shell
#选项
    --all=false: 选择指定资源类型的命名空间中的所有资源。
    --allow-missing-template-keys=true: 如果为 true，则在模板中缺少字段或映射键时忽略模板中的任何错误。仅适用于golang和jsonpath输出格式。
    --cascade=true: 如果为 true，则级联删除此资源管理的资源，默认为真。
    --dry-run=false: 如果为 true，则只打印将要发送的对象，而不发送它。 警告：--dry-run无法准确输出合并本地清单和服务器端数据的结果。使用 --server-dry-run来获取合并结果。
    --field-manager='kubectl'：用于跟踪字段所有权的经理的名称。
-f, --filename=[]: 包含要应用的配置
    --force=false: 仅在宽限期=0时使用。 如果为 true，则立即从API中删除资源并绕过正常删除。请注意，立即删除某些资源可能会导致不一致或数据丢失，需要确认。
    --force-conflicts=false: 如果为 true，则服务器端应用将针对冲突强制更改。
    --grace-period=-1: 给予资源正常终止的时间段(以秒为单位)。如果为负则忽略。设置为 1以立即关闭。只能在 --force为 true时设置为 0(强制删除)。
-k, --kustomize='': 处理自定义目录。 此标志不能与 -f或 -R一起使用。
    --openapi-patch=true: 如果为 true，则在openapi出现并且资源可以在openapi规范中找到时使用openapi计算差异。 否则，回退到使用baked-in类型。
-o, --output='': 输出格式。json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-file其中之一
    --overwrite=true: 使用修改后的配置中的值自动解决修改后的配置和实时配置之间的冲突
    --prune=false: 自动删除资源对象，包括未初始化的资源对象，这些对象没有出现在配置中并且由apply或 create --save-config创建。 应与 -l或 --all一起使用。
    --prune-whitelist=[]: 使用<group/version/kind>为 --prune覆盖默认白名单
    --record=false: 在资源注解中记录当前kubectl命令。如果设置为 false，则不记录命令。 如果设置为 true，则记录命令。如果未设置，则默认仅在已存在注释值时更新现有注释值。
-R, --recursive=false: 递归处理 -f, --filename 中使用的目录。 当您想要管理在同一目录中组织的相关清单时很有用。
-l, --selector='': 要过滤的选择器(标签查询)，支持 '='、'==' 和 '!='。（例如 -l key1=value1,key2=value2）
    --server-dry-run=false: 如果为 true，请求将发送到带有试运行标志的服务器，这意味着修改不会被持久化。 这是一个内部测试功能和标志。
    --server-side=false: 如果为 true，则应用在服务器而不是客户端中运行。
    --template='': -o=go-template, -o=go-template-file 时使用的模板字符串或模板文件路径。 模板格式为 golang 模板
    --timeout=0s: 放弃删除之前等待的时间长度，零表示根据对象的大小确定超时
    --validate=true: 如果为 true，则在发送之前使用架构来验证输入
    --wait=false: 如果为 true，则等待资源消失后再返回。 这等待终结者。
```



## 删除命令

### kubectl delete删除资源

**功能说明：** 通过文件名、控制台输入、资源名或者label selector删除资源。

```shell
# 语法
$ kubectl delete ([-f FILENAME] | [-k DIRECTORY] | TYPE [(NAME | -l label | --all)]) [options]
# 使用 pod.json 中指定的类型和名称删除 pod。
$ kubectl delete -f ./pod.json
# 从包含kustomization.yaml的目录中删除资源 - 例如:目录/kustomization.yaml。
$ kubectl delete -k dir
# 根据传递到stdin的JSON中的类型和名称删除pod。
$ cat pod.json | kubectl delete -f -
# 删除具有相同名称“baz”和“foo”的pod和服务
$ kubectl delete pod,service baz foo
$ # 删除标签名称为myLabel的pod和服务。
kubectl delete pods,services -l name=myLabel
$ # 以最小延迟删除Pod
kubectl delete pod podName --now
$ # 强制删除死节点上的pod
kubectl delete pod podName --grace-period=0 --force
$ # 删除所有Pod
kubectl delete pods --all
```

```shell
#选项
    --all=false: 删除指定资源类型的命名空间中的所有资源，包括未初始化的资源。
-A, --all-namespaces=false: 如果存在，列出所有命名空间中请求的对象。
    --cascade=true: 如果为true，则级联删除此资源管理的资源(例如，由ReplicationController创建的Pod)。默认为真。
    --field-selector='': 要过滤的选择器，支持 '='、'==' 和 '!='。(例如 --field-selector key1=value1,key2=value2)。服务器仅支持每种类型的有限数量的字段查询。
-f, --filename=[]: 包含要删除的资源。
    --force=false: 仅在宽限期=0时使用。如果为true，则立即从API中删除资源并绕过正常删除。请注意，立即删除某些资源可能会导致不一致或数据丢失，需要确认。
    --grace-period=-1: 给予资源正常终止的时间段(以秒为单位)。如果为负则忽略。设置为 1以立即关闭。只能在 --force为 true时设置为 0(强制删除)
    --ignore-not-found=false: 将“未找到资源”视为成功删除。 指定 --all 时默认为“true”。
-k, --kustomize='': 处理自定义目录。 此标志不能与 -f或 -R一起使用。
    --now=false: 如果为true，则通知资源立即关闭(与 --grace-period=1相同)。
-o, --output='': 输出模式。 使用“-o name”来缩短输出（资源/名称）。
    --raw='': 要删除到服务器的原始 URI。 使用 kubeconfig 文件指定的传输。
-R, --recursive=false: 递归处理 -f, --filename 中使用的目录。当您想要管理在同一目录中组织的相关清单时很有用。
-l, --selector='': 要过滤的选择器（标签查询），不包括未初始化的。
    --timeout=0s: 放弃删除之前等待的时间长度，零表示根据对象的大小确定超时
    --wait=true: 如果为 true，则等待资源消失后再返回。这等待终结者。
```

## 拷贝命令

### kubectl cp资源

**功能说明：** 将文件和目录复制到容器或从容器复制到本地。

```shell
# 语法
$ kubectl cp <file-spec-src> <file-spec-dest> [options]
# 重要的提示：要求容器映像中存在tar二进制文件。如果tar不存在，kubectl cp将失败。
# 对于高级用例，例如符号链接、通配符扩展或文件模式保留，请考虑使用kubectl exec。
# 将/tmp/foo本地文件复制到命名空间namespace中远程podName中的/tmp/bar
tar cf - /tmp/foo | kubectl exec -i -n namespace podName -- tar xf - -C /tmp/bar
# 将/tmp/foo从远程podName复制到本地的/tmp/bar
kubectl exec -n namespace podName -- tar cf - /tmp/foo | tar xf - -C /tmp/bar
# 将/tmp/foo_dir本地目录复制到默认命名空间中远程podName中的/tmp/bar_dir
kubectl cp /tmp/foo_dir podName:/tmp/bar_dir
# 将/tmp/foo本地文件复制到特定容器中远程podName中的/tmp/bar
kubectl cp /tmp/foo podName:/tmp/bar -c container
# 将/tmp/foo本地文件复制到命名空间namespace中远程podName中的/tmp/bar
kubectl cp /tmp/foo namespace/podName:/tmp/bar
# 将/tmp/foo从远程podName复制到本地的/tmp/bar
kubectl cp namespace/podName:/tmp/foo /tmp/bar

```

```shell
#选项
-c, --container='': 容器名称。 如果省略，将选择 pod 中的第一个容器
		--no-preserve=false: 复制的文件/目录的所有权和权限不会保留在容器中
```



# 附录A

## K8S中的TYPE选项

| 资源名                     | 简称   | 说明                        |
| -------------------------- | ------ | --------------------------- |
| all                        |        | 所有资源                    |
| certificatesigningrequests | csr    | 请求证书签名                |
| clusterroles               |        | 集群角色                    |
| clusters                   |        | 集群，仅对联合apiserver有效 |
| componentstatuses          | cs     | 组件状态                    |
| configmaps                 | cm     | 配置映射                    |
| controllerrevisions        |        | 控制器修订                  |
| cronjobs                   |        | 定时任务                    |
| daemonsets                 | ds     | 守护进程                    |
| deployments                | deploy | 部署                        |
| endpoints                  | ep     | 端点                        |
| events                     | ev     | 事件                        |
| horizontalpodautoscalers   | hpa    | pod水平自动缩放器           |
| ingresses                  | ing    | 网络入口                    |
| jobs                       |        | 任务                        |
| limitranges                | limits | 限制范围                    |
| namespaces                 | ns     | 命名空间                    |
| networkpolicies            | netpol | 网络策略                    |
| nodes                      | no     | 节点                        |
| persistentvolumeclaims     | pvc    | 声明的持久卷                |
| persistentvolumes          | pv     | 持久卷                      |
| poddisruptionbudgets       | pdb    | pod中断预算                 |
| podpreset                  |        | pod预设                     |
| pods                       | po     | pod列表                     |
| psdpodsecuritypolicies     |        | pod安全策略                 |
| podtemplates               |        | pod模板                     |
| replicasets                | rs     | 副本集                      |
| replicationcontrollers     | rc     | 副本控制器                  |
| resourcequotas             | quota  | 资源配额                    |
| rolebindings               |        | 角色绑定                    |
| roles                      |        | 角色列表                    |
| secrets                    |        | 秘钥                        |
| serviceaccounts            | sa     | 服务帐户                    |
| services                   | svc    | 服务列表                    |
| statefulsets               |        | 有状态集                    |
| storageclasses             |        | 存储类                      |
| thirdpartyresources        |        | 第三方资源                  |

