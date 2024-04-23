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
---

#### Task 5.2: Monitoring and Blue-Green vs Canary Deployment

<blockquote>
<details>
<summary><b>Details</b></summary>

Завдання 1 

1. Розгорніть Kubernetes кластер на Google Cloud за допомогою gcloud cli

2. Після отримання доступу до кластеру, створіть deployment v1.0.0 що повертає версію “Version: 1.0.0”

3. Налаштуйте сервіс типу LoadBalancer та отримайте IP-адресу

4. Налаштуйте Monitor Type Keyword у Uptime Robot вказавши IP-адресу балансера та Keyword “Version: 1.0.0”

5. Моніторингова система перевірить в реальному часі доступність першої версії

6. Налаштуйте публічну status page додавши до неї перший Monitoring

Завдання 2

7. Наступним кроком внесіть зміни у програму, збілдайте та запуште нову версію контейнеру у контейнер реєстр

8. Створить новий деплоймент з версію образу v2.0.0

9. Переведіть трафік з першої на другу версію методами: Canary (25%) та Blue-Green (100%) Deployment

10. По завершенню тестування, залишіть активною v2.0.0 на 100%

11. Налаштуйте Monitor Type Keyword у Uptime Robot вказавши IP-адресу балансера та Keyword “Version: 2.0.0”

12. Моніторингова система перевірить в реальному часі доступність другої версії

13. Налаштуйте публічну status page додавши до неї другий Monitoring

</details>
</blockquote>
---

#### Task 5.3: Examples of prompt engineering solutions for kubernetes deployment

<blockquote>
<details>
<summary><b>Details</b></summary>

Створить портфоліо власних промтів англійською мовою для маніфестів із списку (Приклад: [https://github.com/den-vasyliev/go-demo-app/tree/master/yaml](https://github.com/den-vasyliev/go-demo-app/tree/master/yaml)):

- app.yaml
- app-livenessProbe.yaml
- app-readinessProbe.yaml
- app-volumeMounts.yaml
- app-cronjob.yaml
- app-job.yaml
- app-multicontainer.yaml
- app-resources.yaml
- app-secret-env.yaml

</details>
</blockquote>