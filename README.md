# Portal de Notícias

Esta aplicação de Portal de Notícias foi originalmente desenvolvida pela equipe da Alura. No entanto, fiz adaptações para melhor adequá-la à minha forma de organização e estudos. A aplicação permite cadastrar e exibir notícias de forma simples. Foi desenvolvida em PHP e, em diversas ocasiões, precisei acessar containers para verificar variáveis de ambiente e fazer ajustes necessários.

## Objetivo

O principal objetivo deste projeto é aplicar meus conhecimentos em Docker e Kubernetes, garantindo que a aplicação seja escalável à medida que o uso de recursos, como CPU, atinja determinados limites. Embora a aplicação original tenha sido implementada pela equipe da Alura, adaptei o projeto para refletir meu processo de aprendizagem e otimizei alguns aspectos. Removi configurações desnecessárias e adicionei comentários detalhados em vários arquivos de manifesto para melhorar a organização do código.

## Configuração do Ambiente

Utilizei o Linux Ubuntu 22.04 e o Minikube para criar um cluster local, com fins didáticos. Cada diretório do projeto contém um arquivo `README.md` separado, com uma documentação passo a passo detalhando como configurar e executar cada parte da aplicação. Além disso, explico as decisões técnicas e o motivo de cada configuração incluída no projeto.

## Estrutura do Projeto

O projeto está organizado em três pastas principais, cada uma com um propósito específico:

- **projeto**: Esta pasta contém a configuração "crua" da aplicação. Dentro dela, você encontrará as pastas separadas para o frontend, backend, e o banco de dados MySQL. Cada uma foi configurada de forma a permitir o desenvolvimento modular e independente de cada componente da aplicação.

- **projeto-deploy**: Nesta pasta, concentrei todas as configurações de deploy da aplicação. Ao contrário da pasta `projeto`, aqui todo o processo de implantação está consolidado em um único arquivo, facilitando o deploy em um ambiente de produção ou desenvolvimento.

- **volumes**: Esta pasta foi criada para fins de estudo prático, onde explorei o uso de **StatefulSets**. Aqui, configurei o uso de **Persistent Volumes (PV)**, **Persistent Volume Claims (PVC)**, e **StorageClass (SC)** o que permite a persistência de dados, essencial para gerenciar o estado da aplicação no ambiente de Kubernetes.

## Tecnologias Utilizadas

- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Kubernetes Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
- [Minikube](https://minikube.sigs.k8s.io/docs/)
- [Grafana](https://grafana.com/) & [Prometheus](https://prometheus.io/) (em breve)

