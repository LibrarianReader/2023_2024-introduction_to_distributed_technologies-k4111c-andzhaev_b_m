# Лабораторная работа №2 "Развертывание веб сервиса в Minikube, доступ к веб интерфейсу сервиса. Мониторинг сервиса."

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2023/2024

Group: K4111

Author: Andzhaev Batnasan Mukobenovich

Lab: Lab2

Date of create: 17.12.2024

Date of finished: 19.12.2024

**1. Развернуть minikube cluster**
```
$ minikube start
```
**2. Манифест файла для развертывания deployment-react**

```
apiVersion: apps/v1
kind: Deployment                                            
metadata:
  name: deployment-react                         
spec:
  replicas: 2
  selector:
    matchLabels:
      app: react
  template:
    metadata:
      labels:
        app: react
    spec:                                      
      containers:
        - image: ifilyaninitmo/itdt-contained-frontend:master
          name: react                           
          ports:
          - name: react-port
            containerPort: 3000
          env:
            - name: REACT_APP_USERNAME
              value: 'andzhaev'
            - name: REACT_APP_COMPANY_NAME
              value: 'itmo'
```

**replicas:** 
Количество экземпляров приложения, которые должны быть запущены одновременно

**selector:** 
Определяет, какие поды будут управляться этим деплойментом

**3. Манифест файла для Service**
```
apiVersion: v1
kind: Service
metadata:
  name: front-service
spec:
  selector:
    app: react
  type: NodePort
  ports:
    - port: 3010
      name: react-port
      targetPort: react-port
      protocol: TCP
```
**4. Привязывание манифеста**
```
$ kubectl apply -f service.yaml
```
**5. Перенаправление запрос с Service на pods**
```
$ minikube kubectl -- port-forward service/frontend-service 3010:3010
```

**6. Написать в браузере url**
```
http://localhost:3010
```
**7. Создать новый терминал и найти логи**
```
C:\Users\chitatel>minikube kubectl -- get pods
NAME                                READY   STATUS    RESTARTS      AGE
deployment-react-5858d5b9db-jntdt   1/1     Running   0             44m
deployment-react-5858d5b9db-l6z25   1/1     Running   0             44m
vault                               1/1     Running   1 (26m ago)   88m

C:\Users\chitatel>minikube kubectl -- logs deployment-react-5858d5b9db-l6z25
Builing frontend
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
build finished
Server started on port 3000

C:\Users\chitatel>minikube kubectl -- logs deployment-react-5858d5b9db-jntdt
Builing frontend
build finished
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
Server started on port 3000
```
![image_2024-12-17_22-44-05](https://github.com/user-attachments/assets/b6b88d8d-9363-4561-ab2c-b4c59436b7e6)
![image_2024-12-17_22-47-56](https://github.com/user-attachments/assets/232db1ad-8898-4c1b-aef4-1efc2708d723)
![lab2Draw](https://github.com/user-attachments/assets/85890c14-de4c-40d6-8fd3-6adcf5c7bfa8)


