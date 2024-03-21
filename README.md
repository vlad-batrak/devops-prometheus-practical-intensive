# devops-prometheus-practical-intensive
### Task exercises for Prometheus &amp; GlobalLogic course: "DevOps and Kubernetes. Practical intensive" by Denys Vasyliev

#### Task 1: Implement a web service in your own development environment (play a practical task based on the lecturer's demonstration in the "Build, Ship, Run" coding session)

<details>
<summary><b>Details</b></summary>

#### Практичне завдання

**Завдання:** реалізувати  вебсервіс у власному середовищі розробки (відтворити практичне завдання за демонстрацією лектора у кодінг-сесії «Build, Ship, Run»)

**Мета:** спробувати себе у ролі розробника та інженера експлуатації. Визначити свої сильні сторони, інструменти та теми, що викликають інтерес. Зрозуміти рівень складності подачі матеріалу та практичних завдань. 
Інструменти та технології, які використані в цій роботі, ми будемо надалі вивчати в курсі. Тому не переживайте, якщо ви чогось зараз не знаєте чи не вмієте. Ваше завдання - відчути себе в ролі інженера DevOps і ознайомитися з його типовими завданнями.

#### Очікуваний практичний результат
#### Базовий (необхідний) рівень:
- створені основні акаунти в системах та сервісах, наведених у матеріалах кодінг-сесії;
- налаштоване працююче середовище розробки VSCode;
- створено початковий код виконання практичного завдання;
- код компілюється та запускається локально.

#### Розширений рівень:
- створено образ контейнера за допомогою docker;
- контейнер успішно запускається локально;
- образ контейнеру залитий на docker hub.

#### Топовий рівень:
- встановлено версію kubernetes k3s;
- контейнер запущено у kubernetes;
- сервіс працює.

</details>

<details>
<summary><b>Useful commands</b></summary>
  
##### Deploy first version of an application

```bash
export IMAGE=<image_name or DockerHub repository>
export TAG=<tag or image version>
export NEW_TAG=<new image version>
export APP_NAME=<application name>
```

```bash
# Build docker image
docker build . -t "$IMAGE:$TAG"

# Push image on DockerHub
docker push "$IMAGE:$TAG"
```

```bash
# Deploy application
kubectl create deploy $APP_NAME --image "$IMAGE:$TAG"

# List all pods in ps output format with more information (such as node name)
kubectl get pods -o wide

# Get real-time monitoring of pods changing
kubectl get pods -w

# Forwarding claster port to workstation. (Make it in an another terminal)
kubectl port-forward deploy/$APP_NAME 8080
```

##### Check the result on [https://127.0.0.1:8080](http://127.0.0.1:8080/)

##### Redeploy new version of the application

```bash
# Get information about running deployment
kubectl get deploy $APP_NAME -o wide
```

```bash
export CONTAINER=<copy container name from previous command!>

# Redeploy new version
kubectl set image deploy $APP_NAME $CONTAINER="$IMAGE:$NEW_TAG"
```
##### Check the result on [https://127.0.0.1:8080](http://127.0.0.1:8080/)

</details>
