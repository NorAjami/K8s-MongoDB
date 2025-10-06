
```markdown
# 🐳 Kubernetes MongoDB Demo

Detta projekt visar hur man sätter upp en **MongoDB-databas**, **Mongo Express UI**, och en **ASP.NET Core ToDo-webapp** i **Kubernetes** (t.ex. Docker Desktop Kubernetes).  
Projektet använder **StatefulSet**, **ConfigMap**, **Job** och **Services (NodePort)** för att visa hur komponenter i ett system kan samverka.

---

## ⚙️ Förutsättningar

För att kunna köra detta demo behöver du:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Kubernetes aktiverat i Docker Desktop (Settings → Kubernetes → Enable Kubernetes)
- [kubectl](https://kubernetes.io/docs/tasks/tools/) installerat
- (valfritt) Ett Docker Hub-konto för att pusha egna images

---

## 🚀 Steg-för-steg

### 1️⃣ Starta Kubernetes

Se till att Kubernetes körs i Docker Desktop.

```bash
kubectl get nodes
````

Du ska se något som liknar:

```
NAME             STATUS   ROLES           AGE   VERSION
docker-desktop   Ready    control-plane   10m   v1.30.x
```

---

### 2️⃣ Deploya MongoDB

```bash
kubectl apply -f kubernetes/mongodb-statefulset.yaml
kubectl apply -f kubernetes/mongodb-service.yaml
```

Verifiera:

```bash
kubectl get pods
kubectl get pvc
```

---

### 3️⃣ Deploya Mongo Express (UI)

```bash
kubectl apply -f kubernetes/mongo-express-deployment.yaml
kubectl apply -f kubernetes/mongo-express-service.yaml
```

Kolla vilken port Mongo Express körs på:

```bash
kubectl get svc mongo-express-service
```

Exempelutskrift:

```
mongo-express-service   NodePort   10.1.1.55   <none>   8081:32000/TCP
```

👉 Öppna webbläsaren: [http://localhost:32000](http://localhost:32000)

---

### 4️⃣ Initiera databasen

```bash
kubectl apply -f kubernetes/mongo-init-job.yaml
kubectl logs --selector job-name=mongo-init-job
```

Du ska se att två “ToDoItems” läggs till i databasen.

---

### 5️⃣ Deploya ToDo-webappen

```bash
kubectl apply -f kubernetes/todo-configmap.yaml
kubectl apply -f kubernetes/todo-deployment.yaml
kubectl apply -f kubernetes/todo-service.yaml
```

Kolla vilken NodePort appen har fått:

```bash
kubectl get svc todo-service
```

Exempel:

```
todo-service   NodePort   10.1.1.99   <none>   80:31000/TCP
```

👉 Öppna webbläsaren: [http://localhost:31000](http://localhost:31000)

---

## 🧹 Rensa allt

När du vill ta bort alla resurser:

```bash
kubectl delete -f kubernetes/
```

---

## 📄 Licens

MIT License – se [LICENSE](LICENSE).

---

## 💡 Tips & felsökning

| Problem                        | Lösning                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------------------- |
| Pod står i `ContainerCreating` | Kör `kubectl describe pod <podnamn>` för att se eventuella fel (t.ex. image-pull eller PVC). |
| Mongo Express öppnas inte      | Kontrollera att rätt NodePort används och att podden körs (`kubectl get pods`).              |
| Init-jobb fastnar              | Kolla loggar: `kubectl logs --selector job-name=mongo-init-job`.                             |
| Vill starta om helt            | `kubectl delete all --all` rensar hela namnområdet (var försiktig).                          |

---

## ✨ Framtida förbättringar

* Lägg till Secrets för MongoDB credentials
* Lägg till Ingress Controller för extern access
* Automatisera med GitHub Actions (CI/CD)


```

