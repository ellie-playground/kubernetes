[Hello Minikube](https://kubernetes.io/ko/docs/tutorials/hello-minikube/)

1. minikube 설치
    
    ```json
    brew install minikube
    ```
    
2. minikube 클러스터 만들기
    
    ```json
    minikube start
    ```
    
    - 아래 오류 발생 시, [docker 설치](https://docs.docker.com/desktop/setup/install/mac-install/)
        
        ```json
        flitto@flittoui-MacBookPro ~ % minikube start
        😄  Darwin 26.3 (arm64) 의 minikube v1.38.1
        👎  Unable to pick a default driver. Here is what was considered, in preference order:
        💡  또는 다음 드라이버 중 하나를 설치할 수 있습니다:
            ▪ docker: Not installed: exec: "docker": executable file not found in $PATH
            ▪ vfkit: Not installed: exec: "vfkit": executable file not found in $PATH
            ▪ qemu2: Not installed: exec: "qemu-system-aarch64": executable file not found in $PATH
            ▪ virtualbox: Not installed: unable to find VBoxManage in $PATH
            ▪ parallels: Not installed: exec: "prlctl": executable file not found in $PATH
            ▪ hyperkit: Not installed: exec: "hyperkit": executable file not found in $PATH
            ▪ podman: Not installed: exec: "podman": executable file not found in $PATH
            ▪ krunkit: Not installed: exec: "krunkit": executable file not found in $PATH
        
        ❌  Exiting due to DRV_NOT_DETECTED: No possible driver was detected. Try specifying --driver, or see https://minikube.sigs.k8s.io/docs/start
        ```
        
3. 대시보드 접속하기
    
    ```json
    minikube dashboard
    ```
    
    http://127.0.0.1:52737/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/workloads?namespace=default
    

## 디플로이먼트 만들기

쿠버네티스 파드는 관리와 네트워킹 목적으로 함께 묶여 있는 하나 이상의 컨테이너 그룹이다. 이 튜토리얼의 파드에는 단 하나의 컨테이너만 있다. 쿠버네티스 디플로이먼트는 파드의 상태를 확인하고, 파드의 컨테이너가 종료되었다면 재시작한다. 파드의 생성 및 스케일링을 관리하는 방법으로 디플로이먼트를 권장한다.

1. `kubectl create` 명령어를 실행해 파드를 관리할 디플로이먼트를 생성한다.
    
    ```json
    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.53
    ```
    
2. 디플로이먼트 조회하기
    
    ```json
    flitto@flittoui-MacBookPro ~ % kubectl get deployments
    NAME         READY   UP-TO-DATE   AVAILABLE   AGE
    hello-node   1/1     1            1           86s
    ```
    
3. 파드 조회하기
    
    ```json
    flitto@flittoui-MacBookPro ~ % kubectl get pods
    NAME                          READY   STATUS    RESTARTS   AGE
    hello-node-699bc7d45f-xfkl9   1/1     Running   0          2m1s
    ```
    
4. 클러스터 이벤트 보기
    
    ```json
    flitto@flittoui-MacBookPro ~ % kubectl get events
    LAST SEEN   TYPE      REASON                    OBJECT                             MESSAGE
    2m35s       Normal    Scheduled                 pod/hello-node-699bc7d45f-xfkl9    Successfully assigned default/hello-node-699bc7d45f-xfkl9 to minikube
    2m35s       Normal    Pulling                   pod/hello-node-699bc7d45f-xfkl9    Pulling image "registry.k8s.io/e2e-test-images/agnhost:2.53"
    2m23s       Normal    Pulled                    pod/hello-node-699bc7d45f-xfkl9    Successfully pulled image "registry.k8s.io/e2e-test-images/agnhost:2.53" in 12.627s (12.627s including waiting). Image size: 145780890 bytes.
    2m23s       Normal    Created                   pod/hello-node-699bc7d45f-xfkl9    Container created
    2m23s       Normal    Started                   pod/hello-node-699bc7d45f-xfkl9    Container started
    2m36s       Normal    SuccessfulCreate          replicaset/hello-node-699bc7d45f   Created pod: hello-node-699bc7d45f-xfkl9
    2m36s       Normal    ScalingReplicaSet         deployment/hello-node              Scaled up replica set hello-node-699bc7d45f from 0 to 1
    10m         Normal    NodeHasSufficientMemory   node/minikube                      Node minikube status is now: NodeHasSufficientMemory
    10m         Normal    NodeHasNoDiskPressure     node/minikube                      Node minikube status is now: NodeHasNoDiskPressure
    10m         Normal    NodeHasSufficientPID      node/minikube                      Node minikube status is now: NodeHasSufficientPID
    10m         Normal    NodeAllocatableEnforced   node/minikube                      Updated Node Allocatable limit across pods
    10m         Normal    Starting                  node/minikube                      Starting kubelet.
    10m         Normal    NodeAllocatableEnforced   node/minikube                      Updated Node Allocatable limit across pods
    10m         Normal    NodeHasSufficientMemory   node/minikube                      Node minikube status is now: NodeHasSufficientMemory
    10m         Normal    NodeHasNoDiskPressure     node/minikube                      Node minikube status is now: NodeHasNoDiskPressure
    10m         Normal    NodeHasSufficientPID      node/minikube                      Node minikube status is now: NodeHasSufficientPID
    10m         Normal    NodeReady                 node/minikube                      Node minikube status is now: NodeReady
    10m         Normal    RegisteredNode            node/minikube                      Node minikube event: Registered Node minikube in Controller
    9m59s       Normal    Starting                  node/minikube                      
    9m6s        Warning   ContainerGCFailed         node/minikube                      rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing: dial unix /var/run/cri-dockerd.sock: connect: connection refused"
    9m1s        Normal    RegisteredNode            node/minikube                      Node minikube event: Registered Node minikube in Controller
    8m55s       Normal    Starting                  node/minikube                      
    
    ```
    
5. `kubectl` 환경설정 보기
    
    ```json
    flitto@flittoui-MacBookPro ~ % kubectl config view
    apiVersion: v1
    clusters:
    - cluster:
        certificate-authority: /Users/flitto/.minikube/ca.crt
        extensions:
        - extension:
            last-update: Thu, 12 Mar 2026 18:25:49 KST
            provider: minikube.sigs.k8s.io
            version: v1.38.1
          name: cluster_info
        server: https://127.0.0.1:52674
      name: minikube
    contexts:
    - context:
        cluster: minikube
        extensions:
        - extension:
            last-update: Thu, 12 Mar 2026 18:25:49 KST
            provider: minikube.sigs.k8s.io
            version: v1.38.1
          name: context_info
        namespace: default
        user: minikube
      name: minikube
    current-context: minikube
    kind: Config
    users:
    - name: minikube
      user:
        client-certificate: /Users/flitto/.minikube/profiles/minikube/client.crt
        client-key: /Users/flitto/.minikube/profiles/minikube/client.key
    ```
    
6. 파드 내 컨테이너 애플리케이션 로그 확인
    
    ```json
    kubectl logs $(kubectl get pods 명령어 출력에서 얻은 파드 이름)
    
    flitto@flittoui-MacBookPro ~ % kubectl logs hello-node-699bc7d45f-xfkl9
    Paused (???)
    ```
    

## 서비스 만들기

1. `kubectl expose` 명령어로 퍼블릭 인터넷에 파드 노출하기
    
    ```json
    flitto@flittoui-MacBookPro ~ % kubectl expose deployment hello-node --type=LoadBalancer --port=8080
    service/hello-node exposed
    ```
    
    - `--type=LoadBalancer` 플래그는 클러스터 밖의 서비스로 노출하기 원한다는 뜻이다.
2. 생성한 서비스 살펴보기
    
    ```json
    flitto@flittoui-MacBookPro ~ % kubectl get services
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    hello-node   LoadBalancer   10.99.172.247   <pending>     8080:31738/TCP   73s
    kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          18m
    ```
    
3. 아래 명령어를 실행해 브라우저 창을 열어 앱을 보여주고, 그 응답을 표시한다.
    
    ```json
    flitto@flittoui-MacBookPro ~ % minikube service hello-node
    ┌───────────┬────────────┬─────────────┬───────────────────────────┐
    │ NAMESPACE │    NAME    │ TARGET PORT │            URL            │
    ├───────────┼────────────┼─────────────┼───────────────────────────┤
    │ default   │ hello-node │ 8080        │ http://192.168.49.2:31738 │
    └───────────┴────────────┴─────────────┴───────────────────────────┘
    🔗  hello-node 서비스의 터널을 시작하는 중
    ┌───────────┬────────────┬─────────────┬────────────────────────┐
    │ NAMESPACE │    NAME    │ TARGET PORT │          URL           │
    ├───────────┼────────────┼─────────────┼────────────────────────┤
    │ default   │ hello-node │             │ http://127.0.0.1:53057 │
    └───────────┴────────────┴─────────────┴────────────────────────┘
    🎉  Opening service default/hello-node in default browser...
    ❗  darwin 에서 Docker 드라이버를 사용하고 있기 때문에, 터미널을 열어야 실행할 수 있습니다.
    
    ```
