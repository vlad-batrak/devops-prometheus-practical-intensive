## devops-prometheus-practical-intensive
# Task exercises to Prometheus &amp; GlobalLogic course: "DevOps and Kubernetes. Practical intensive" by Denys Vasyliev

#### Task 1.1: Implement a web service in your own development environment (play a practical task based on the lecturer's demonstration in the "Build, Ship, Run" coding session)

<blockquote>
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
</blockquote>

<blockquote>
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
</blockquote>

---

#### Task 5.1: Extending kubectl (Extending kubectl). Help colleagues complete tasks

<blockquote>
<details>
<summary><b>Details</b></summary>
Завдання:

- Доопрацюйте та виправте помилки скрипта kubeplugin
- Зробіть рефаторинг коду скрипта
- Протестуйте скрипт як плагін kubectl
- Закомітьте скрипт у директорію scripts репозиторію
- Текстовий вивід статистики з реального кластеру namespace kube-system у форматі: "Resource, Namespace, Name, CPU, Memory"
- Додайте інструкцію з використання

Код плагіну:
``` bash
#!/bin/bash

# Define command-line arguments

RESOURCE_TYPE=$0

# Retrieve resource usage statistics from Kubernetes
kubectl $2 $RESOURCE_TYPE -n $1 | tail -n +2 | while read line
do
  # Extract CPU and memory usage from the output
  NAME=$(echo $line | awk '{print $1}')
  CPU=$(echo $line | awk '{print $2}')
  MEMORY=$(echo $line | awk '{print $3}')

  # Output the statistics to the console
  # "Resource, Namespace, Name, CPU, Memory"
done
```

</details>
</blockquote>