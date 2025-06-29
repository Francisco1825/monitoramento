
# 🍿 Rotten Potatoes - Observabilidade com Prometheus e Grafana

Este projeto é uma aplicação full-stack monitorada com **Prometheus** e **Grafana** via Docker Compose. A aplicação principal é o *Rotten Potatoes*, que se conecta a um banco MongoDB e está pronta para observabilidade e coleta de métricas.

---

## 📦 Arquitetura

```plaintext
+-------------+       +-----------+       +-------------+
|             |       |           |       |             |
|   Grafana   <-------+ Prometheus+<------+ Application |
|             |       |           |       |   (Gunicorn)|
+-------------+       +-----------+       +------+------+ 
                                    |            
                              +-----v------+
                              |   MongoDB  |
                              +------------+
```

---

## ⚙️ Tecnologias utilizadas

| Ferramenta     | Função                                               |
|----------------|------------------------------------------------------|
| Docker Compose | Orquestra os containers                              |
| MongoDB        | Banco de dados NoSQL                                 |
| Gunicorn       | Servidor WSGI para aplicação Python                  |
| Prometheus     | Coleta e armazena métricas da aplicação              |
| Grafana        | Exibe dashboards e gráficos com base nas métricas    |

---

## 🚀 Como executar

### 1. Clone o repositório

```bash
git clone https://github.com/Francisco1825/monitoramento.git 
```

### 2. Estrutura esperada

Certifique-se de ter os seguintes arquivos na raiz:

```
.
├── docker-compose.yml
├── prometheus.yml
```

### 3. Suba os containers

```bash
docker compose up -d
```

### 4. Acesse os serviços

| Serviço          | URL                      |
|------------------|--------------------------|
| Rotten Potatoes  | http://localhost:8080    |
| MongoDB          | Internamente como `mongodb:27017` |
| Prometheus       | http://localhost:9090    |
| Grafana          | http://localhost:3000    |

---

## 📊 Dashboards e Métricas

### Prometheus

Prometheus está configurado para coletar métricas dos seguintes serviços:

- **Rotten Potatoes (porta 5000)**: `job_name: rottenpotatoes`
- **Prometheus em si**: `job_name: prometheus`
- **Docker Host Metrics**: `job_name: docker`  
  > ⚠️ É necessário que a porta 9323 esteja disponível com as métricas do Docker Engine habilitadas (`--metrics-addr` no daemon Docker).

### Grafana

Para acessar:

- Acesse: [http://localhost:3000](http://localhost:3000)
- Login padrão:
  - Usuário: `Admin`
  - Senha: `admin`

Você pode importar dashboards usando Prometheus como fonte de dados.

---

## 🔧 Variáveis de ambiente

### Aplicação Web

```env
MONGODB_DB=admin
MONGODB_HOST=mongodb
MONGODB_PORT=27017
MONGODB_USERNAME=mongouser
MONGODB_PASSWORD=mongopwd
```

### MongoDB

```env
MONGO_INITDB_ROOT_USERNAME=mongouser
MONGO_INITDB_ROOT_PASSWORD=mongopwd
```

---

## 📂 Volumes e Persistência

- Os dados do MongoDB são persistidos através do volume `db_mongo`, montado em `/data/db`.

---

## 🧪 Teste rápido

Acesse:

```bash
curl http://localhost:8080
```

E depois confira no Prometheus se o *endpoint* está sendo coletado:

```bash
http://localhost:9090/targets
```

---

## ❗ Observações importantes

- Para que a coleta de métricas do Docker (`host.docker.internal:9323`) funcione corretamente, é necessário habilitar o **Docker Metrics Endpoint** com:

```json
{
  "metrics-addr" : "0.0.0.0:9323",
  "experimental" : true
}
```

- Em sistemas Linux, `host.docker.internal` pode não funcionar nativamente. Considere adicionar mapeamentos no `extra_hosts` ou expor a porta diretamente.

---

## 🛠️ Próximos passos (Sugestões)

- Adicionar o cAdvisor para métricas detalhadas de containers
- Configurar alertas no Prometheus e Grafana
- Criar dashboards personalizados no Grafana para MongoDB e aplicação
- Adicionar autenticação segura e volumes nomeados para Grafana

---

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---

## 🤝 Contribuição

Contribuições são bem-vindas! Sinta-se livre para abrir uma *issue* ou *pull request*.

---

## 🙋‍♂️ Autor

Desenvolvido por **[Francisco Douglas]
GITHUB: (https://github.com/Francisco1825)
LinkedIn:(https://www.linkedin.com/in/francisco-douglas-85b709216/)  
Entre em contato para dúvidas ou sugestões!
