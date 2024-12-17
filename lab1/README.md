**1. Развернуть minikube cluster**
```
$ minikube start
```
**2. Манифест файла для развертывания vault**

```
apiVersion: v1
kind: Pod
metadata:
  name: vault
  labels:
    app: vault
spec:
  containers:
  - name: vault
    image: hashicorp/vault:latest
    ports:
    - containerPort: 8200
```

**3. Создание или обновление объекта в Pod**
```
$ kubectl apply -f vault.yaml
```
**4. Создаёт объект Service, который открывает доступ к Pod vault через порт 8200**
```
$ minikube kubectl -- expose pod vault --type=NodePort --port=8200
```
**5. Перенаправление локального порта 8200 на порт 8200 сервиса vault, чтобы получить доступ к сервису**
```
$ minikube kubectl -- port-forward service/vault 8200:8200
```
**6. Написать в браузере url**
```
http://localhost:8200/ui/vault/dashboard
```
**7. Создать новый терминал и найти Root Token**
```
$ minikube kubectl logs vault
```
![photo_2024-12-17_21-37-07](https://github.com/user-attachments/assets/e0d16d26-cd89-4a93-b3b4-c867efcf03c4)
