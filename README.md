Perfekt! 😍 Då gör vi din **README.md** både **proffsig och snygg** – med färgade badges, emoji-ikoner och en modern GitHub-look (som företag och rekryterare älskar att se).
Jag behåller samma tydliga struktur som innan, men nu ser den ut som ett “riktigt projekt” i din portfolio.

---

### 🌟 **Uppdaterad README.md (med badges och design)**

```markdown
<h1 align="center">🐳 Kubernetes MongoDB Demo</h1>

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License: MIT">
  <img src="https://img.shields.io/badge/Kubernetes-v1.30+-blue.svg" alt="Kubernetes">
  <img src="https://img.shields.io/badge/Docker-Desktop-orange.svg" alt="Docker Desktop">
  <img src="https://img.shields.io/badge/MongoDB-Atlas%20Ready-brightgreen.svg" alt="MongoDB">
  <img src="https://img.shields.io/badge/ASP.NET-Core%20Razor%20Pages-68217A.svg" alt="ASP.NET Core">
</p>

<p align="center">
  <b>Fullstack Kubernetes demo med MongoDB, Mongo Express och en ASP.NET ToDo-app</b><br>
  <i>Byggd för att förstå hur flera containrar kan kommunicera i ett Kubernetes-kluster.</i>
</p>

---

## 📚 Innehållsförteckning
- [Om projektet](#-om-projektet)
- [Struktur](#-projektstruktur)
- [Förutsättningar](#️-förutsättningar)
- [Installation och körning](#-steg-för-steg)
- [Rensa resurser](#-rensa-allt)
- [Felsökning](#-tips--felsökning)
- [Framtida förbättringar](#-framtida-förbättringar)
- [Licens](#-licens)

---

## 💡 Om projektet

Detta projekt visar hur man:
- Sätter upp **MongoDB** med **persistent storage (PVC)**  
- Deployar **Mongo Express** som UI för databasen  
- Skapar ett **init-jobb** som fyller MongoDB med testdata  
- Deployar en **ASP.NET Core Razor Pages ToDo-app** som läser från databasen  

Kort sagt: du lär dig grunderna i **Kubernetes-kommunikation, ConfigMaps, Jobs och NodePort-tjänster**. 🚀

---

## 📂 Projektstruktur

```

k8s-mongo-demo/
├─ kubernetes/
│  ├─ mongodb-statefulset.yaml
│  ├─ mongodb-service.yaml
│  ├─ mongo-express-deployment.yaml
│  ├─ mongo-express-service.yaml
│  ├─ mongo-init-job.yaml
│  ├─ todo-configmap.yaml
│  ├─ todo-deployment.yaml
│  └─ todo-service.yaml
│
├─ init-image/
│  ├─ Dockerfile
│  └─ init-mongo.sh
│
├─ .gitignore
└─ LICENSE

````

---

## ⚙️ Förutsättningar

Du behöver ha följande installerat:

| Program | Länk |
|----------|------|
| 🐋 Docker Desktop | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop) |
| ☸️ Kubernetes (aktiverat i Docker Desktop) | Via Docker Desktop Settings → Kubernetes |
| 🔧 kubectl | [Installera här](https://kubernetes.io/docs/tasks/tools/) |
| 🐙 Git + GitHub | För att versionera projektet |
| 🧱 Docker Hub (valfritt) | Om du vill pusha egna images |

---

## 🚀 Steg-för-steg

### 1️⃣ Starta Kubernetes
```bash
kubectl get nodes
````

Om allt funkar ska du se något liknande:

```
NAME             STATUS   ROLES           AGE   VERSION
docker-desktop   Ready    control-plane   5m    v1.30.x
```

---

### 2️⃣ Deploya MongoDB

```bash
kubectl apply -f kubernetes/mongodb-statefulset.yaml
kubectl apply -f kubernetes/mongodb-service.yaml
```

Kontrollera:

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

Hitta porten:

```bash
kubectl get svc mongo-express-service
```

Öppna i webbläsaren (exempel):
👉 [http://localhost:32000](http://localhost:32000)

---

### 4️⃣ Initiera databasen (Job)

```bash
kubectl apply -f kubernetes/mongo-init-job.yaml
kubectl logs --selector job-name=mongo-init-job
```

✅ Du ska se loggar som visar att “ToDoItems” har skapats.

---

### 5️⃣ Deploya ToDo-webappen

```bash
kubectl apply -f kubernetes/todo-configmap.yaml
kubectl apply -f kubernetes/todo-deployment.yaml
kubectl apply -f kubernetes/todo-service.yaml
```

Kolla port:

```bash
kubectl get svc todo-service
```

Öppna i webbläsaren:
👉 [http://localhost:31000](http://localhost:31000)

---

## 🧹 Rensa allt

När du är klar:

```bash
kubectl delete -f kubernetes/
```

---

## 🔍 Tips & Felsökning

| Problem                 | Orsak                      | Lösning                                    |
| ----------------------- | -------------------------- | ------------------------------------------ |
| `ContainerCreating`     | Kubernetes väntar på image | `kubectl describe pod <namn>` för mer info |
| `CrashLoopBackOff`      | Init-scriptet misslyckades | Kontrollera `kubectl logs` för jobben      |
| Mongo Express laddas ej | NodePort ej öppen          | `kubectl get svc` och öppna rätt port      |
| Vill börja om helt      | -                          | `kubectl delete all --all` (försiktigt!)   |

---

## 🧭 Framtida förbättringar

* [ ] Lägga till Secrets för användarnamn/lösenord
* [ ] Använda Ingress istället för NodePort
* [ ] CI/CD med GitHub Actions
* [ ] Helm Chart-version av detta projekt

---

## 📄 Licens

MIT License – se [LICENSE](LICENSE)

---

