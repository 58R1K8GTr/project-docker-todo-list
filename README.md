# 🐳 Docker Todo List — Infraestrutura & Orquestração Full-Stack

O **Docker Todo List** é um projeto focado em DevOps, conteinerização e orquestração de infraestrutura de microsserviços. O objetivo principal foi pegar uma aplicação full-stack de tarefas (composta por Front-end, Back-end e uma suíte de testes ponta a ponta) e isolar cada camada em ecossistemas portáteis, eficientes e comunicáveis utilizando **Docker** e **Docker Compose**.

---

## 🚀 Habilidades Desenvolvidas & Consolidadas

Este projeto validou a capacidade de configurar ambientes de produção escaláveis e consistentes, mitigando o clássico problema do *"na minha máquina funciona"*:

* **Gerenciamento de Containers via CLI:** Domínio de comandos fundamentais para criação (`create`), execução (`run`), auditoria (`inspect`, `cat`), listagem filtrada (`ps --filter`) e limpeza (`rm`, `stop`) de instâncias isoladas.
* **Construção de Imagens Inteligentes (`Dockerfile`):**
    * Criação de receitas de build multiplataforma utilizando imagens base otimizadas (`node:16-alpine`).
    * Manipulação avançada de arquivos comprimidos utilizando o comando `ADD` para descompactação automática de dependências em tempo de build.
    * Definição estratégica de ciclos de execução combinando as diretivas `ENTRYPOINT` (para comandos imutáveis) e `CMD` (para parâmetros sobrescritíveis).
* **Orquestração de Microsserviços (`Docker Compose`):**
    * Estruturação de um arquivo multi-container (`docker-compose.yml`) unificando serviços distintos sob uma mesma rede virtual isolada.
    * Gerenciamento de fluxo de inicialização através da modelagem de árvores de dependência lógica (`depends_on`).
    * Injeção dinâmica de variáveis de ambiente para comunicação entre redes internas de containers mapeando hosts nominais.
* **Mapeamento de Portas e Volumes:** Exposição controlada de serviços internos (`3000` para o Front-end e `3001` para o Back-end) com amarração às portas do sistema hospedeiro.

---

## 🗺️ Arquitetura dos Microsserviços Orquestrados

A infraestrutura foi desenhada para conectar três frentes distintas em perfeita sintonia:

1.  **`todoback` (Back-end):** API baseada em Node.js rodando na porta `3001`.
2.  **`todofront` (Front-end):** Interface em React rodando na porta `3000` que consome dinamicamente os endpoints injetados pela variável de ambiente `REACT_APP_API_HOST`.
3.  **`todotests` (E2E Tests):** Container utilitário baseado em `Puppeteer` que aguarda a inicialização completa dos serviços anteriores para simular cliques e rotinas reais de usuário, validando a integridade de ponta a ponta da aplicação.

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

* **Engine Core:** Docker CLI
* **Orquestrador:** Docker Compose (Versão v3+)
* **Ambientes Base:** Node.js 16 (Variante Alpine Linux) / Puppeteer Headless Image
* **Testes do Ambiente:** Jest (Integrado ao avaliador da aplicação)

---

## 🐳 Como Executar a Orquestração Localmente

Para rodar todo o ecossistema full-stack unificado na sua máquina local, você precisa apenas do Docker instalado.

> ⚠️ **Aviso:** Certifique-se de que as portas `3000` e `3001` estejam totalmente livres no seu sistema antes de prosseguir.

1.  **Clone o repositório:**
    ```bash
    git clone git@github.com:seu-usuario/sd-040-project-docker-todo-list.git
    cd sd-040-project-docker-todo-list
    ```

2.  **Suba o ecossistema orquestrado via Compose:**
    Navegue até a pasta onde o arquivo estruturado do compose está localizado e execute:
    ```bash
    docker-compose up -d
    ```
    *Isso vai baixar/estruturar as imagens locais em segundo plano e ligar os microsserviços integrados.*

3.  **Acesse a aplicação:**
    * Abra o navegador e acesse a interface visual em: `http://localhost:3000`
    * A API estará escutando requisições em: `http://localhost:3001`

4.  **Rodar os testes locais (Opcional):**
    Caso queira validar os arquivos de comandos do CLI criados na pasta `docker-commands/`, instale os runners locais e execute a automação:
    ```bash
    npm install
    npm test
    ```

---

## 📝 Estrutura dos Artefatos de Infraestrutura

O repositório foi construído de forma modularizada dividindo os comandos em arquivos estritos de Linha de Comando (`.dc`) e arquivos estruturais de imagem:

* `docker/docker-commands/command01.dc` até `command08.dc`: Automações puras de controle de estado de containers via CLI.
* `docker/todo-app/back-end/Dockerfile`: Arquitetura da imagem Node para a API.
* `docker/todo-app/front-end/Dockerfile`: Arquitetura da imagem Node para o cliente React.
* `docker/todo-app/tests/Dockerfile`: Arquitetura do container de testes integrados.
* `docker/docker-compose.yml`: Arquivo mestre de orquestração unificada de rede e serviços.
