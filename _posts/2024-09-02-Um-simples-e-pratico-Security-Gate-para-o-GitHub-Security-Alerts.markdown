---
layout: post
title:  "Um simples e prático Security Gate para o GitHub Security Alerts"
date:   2024-09-02
---

Índice:

- Sumário  
- Contextualização  
- Objetivo  
- Introdução ao Security Gate  
- O futuro do Security Gate  
- Conclusão  
- Referências

### Sumário

Garantir a segurança do código antes de chegar à produção é crucial. No entanto, isso exige a implementação de soluções que realmente se integrem ao fluxo contínuo de integração e desenvolvimento. Com a crescente complexidade das aplicações e a necessidade de respostas rápidas, a abordagem de *Shift Left* se torna indispensável, movendo a segurança para as fases iniciais do ciclo de desenvolvimento. Neste artigo, apresentamos e exploramos o conceito de *Security Gates*, inspirado em *Quality Gates* e alinhado com a abordagem *Shift Left*, e apresentamos também o [**Security Gate**](https://github.com/instriq/security-gate/), uma solução prática que utiliza GitHub Actions para monitorar o repositório e bloquear a pipeline com base em alertas de segurança. Exploramos como essa ferramenta pode ser configurada para funcionar como um gate de segurança eficaz, visando a segurança contínua do repositório/projeto.

### Contextualização

No cenário contemporâneo de desenvolvimento de software, a segurança se consolidou como um componente fundamental, permeando todas as fases do ciclo de vida de desenvolvimento. Com a crescente adoção de metodologias ágeis e práticas de DevOps, o conceito de *Shift Left* têm ganhado destaque, promovendo a antecipação das atividades de teste e validação de segurança para as etapas iniciais do processo de desenvolvimento. Essa abordagem busca mitigar vulnerabilidades desde os primeiros momentos, o que não apenas reduz os custos de correção, mas também minimiza os riscos associados a problemas de segurança que, se descobertos tardiamente, poderiam comprometer gravemente a integridade do sistema.

O *Shift Left*, ao mover as verificações de segurança para o início do ciclo de desenvolvimento, transforma a segurança em uma parte intrínseca do fluxo de trabalho, em vez de tratá-la como uma fase posterior ou separada. Isso permite que as equipes de desenvolvimento identifiquem e resolvam potenciais falhas antes que elas possam se propagar, resultando em um software mais seguro e confiável. Adicionalmente, essa prática alinha-se com a necessidade de manter a agilidade no desenvolvimento, sem comprometer a proteção contra ameaças.

Dentro desse contexto, os *Quality Gates* emergem como barreiras essenciais na pipeline de desenvolvimento, assegurando que apenas códigos que atendam aos critérios de qualidade avancem para as próximas etapas. Inspirado por esse conceito, o [**Security Gate**](https://github.com/instriq/security-gate/) surge como uma evolução natural, focando especificamente na segurança. Atuando como pontos de verificação dentro da pipeline de CI/CD, os *Security Gates* avaliam o código quanto à conformidade com políticas de segurança estabelecidas, interrompendo seu progresso caso os critérios não sejam atendidos e prevenindo que vulnerabilidades sejam introduzidas no ambiente de produção, garantindo que o software final atenda aos padrões de segurança.

Assim, o [**Security Gate**](https://github.com/instriq/security-gate/) materializa esse conceito em uma solução prática e acessível para o GitHub. Com o objetivo de integrar verificações automáticas de segurança diretamente na pipeline de CI/CD, o [**Security Gate**](https://github.com/instriq/security-gate/) monitora o repositório e bloqueia o avanço da pipeline com base em alertas de segurança, como os gerados pelo DependaBot, Code Scanning e Secret Scanning. Dessa forma, o projeto visa assegurar que apenas códigos que cumpram os critérios de segurança possam avançar na pipeline, alinhando-se com a filosofia do *Shift Left* e reforçando a proteção contínua e proativa do repositório.

### Objetivo

O [**Security Gate**](https://github.com/instriq/security-gate/) visa aprimorar a segurança no ciclo de desenvolvimento de software ao integrar um mecanismo de controle diretamente na pipeline de CI/CD do GitHub. Este projeto é fundamentado na ideia de *Shift Left* e no conceito de *Quality Gates*, adaptando-os para um contexto de segurança. A proposta é garantir que apenas código que atenda aos critérios de segurança avance nas fases de desenvolvimento e produção.

A ferramenta permite a integração com o GitHub Actions e utiliza os alertas de segurança gerados pelo DependaBot, Code Scanning e Secret Scanning como base para a tomada de decisões. O [**Security Gate**](https://github.com/instriq/security-gate/) oferece a capacidade de definir políticas de vulnerabilidade baseadas em impacto — como o número de vulnerabilidades por severidade de ameaça — e atua de forma proativa, bloqueando a pipeline de CI/CD se essas políticas não forem cumpridas.

Para alcançar esse objetivo, o [**Security Gate**](https://github.com/instriq/security-gate/) atende aos seguintes requisitos:

- **Integração com GitHub Actions**: para monitoramento contínuo do repositório, utilizando alertas de segurança para assegurar conformidade com as políticas de segurança definidas;  
- **Suporte a alertas de segurança**: provenientes do DependaBot, Code Scanning e Secret Scanning, permitindo uma análise abrangente das vulnerabilidades e ameaças no código;  
- **Política de vulnerabilidade baseada em Impacto**: como o número de vulnerabilidades por severidade, especificadas através das flags de limite de severidade, para bloquear a pipeline de CI/CD quando essas políticas não são atendidas;  
- **Flexibilidade e controle**: ao permitir a configuração de quais tipos de alertas devem ser verificados, com as flags `--dependency-alerts`, `--secret-alerts` e `--code-alerts`, permitindo que os usuários escolham quais alertas devem ser monitorados conforme suas necessidades.

### Introdução ao Security Gate

![](/assets/img/security-gate.png)

O [**Security Gate**](https://github.com/instriq/security-gate/) foi desenvolvido para integrar-se com as pipelines de desenvolvimento de software, oferecendo uma solução prática para a gestão de alertas de segurança do GitHub. Criado em Perl, o sistema utiliza os recursos de alerta de segurança do GitHub e se integra com GitHub Actions, proporcionando um método para aplicar políticas de segurança e assegurar que o código atenda a critérios específicos antes de ser promovido para produção.

#### Visão Geral

O [**Security Gate**](https://github.com/instriq/security-gate/) busca atender à necessidade de uma abordagem organizada para a segurança dentro das pipelines de CI/CD. O objetivo é oferecer uma maneira de monitorar e aplicar políticas de segurança com base nos alertas fornecidos pelo GitHub. A ferramenta permite configuração detalhada e é projetada para alinhar-se aos requisitos de segurança de diferentes projetos.

#### Principais Recursos

O [**Security Gate**](https://github.com/instriq/security-gate/) inclui os seguintes recursos principais:

- **Scanning de Dependências**  
    
  Este módulo utiliza os alertas do DependaBot fornecidos pelo GitHub para monitorar vulnerabilidades nas dependências do projeto. O sistema consulta a API do GitHub para alertas de dependência abertos, contabiliza as vulnerabilidades classificadas em diferentes níveis de severidade (crítico, alto, médio e baixo) e compara essas contagens com os limites configurados. Caso os limites sejam excedidos, a pipeline de CI/CD pode ser bloqueada, garantindo que vulnerabilidades em bibliotecas de terceiros não comprometam o projeto.  
    
- **Scanning de Secrets**  
    
  O recurso de scanning de secrets integra-se com os alertas de Secret Scanning do GitHub para identificar informações sensíveis que possam ter sido expostas no repositório. A ferramenta recupera e processa alertas de secrets abertos, fornecendo detalhes sobre o número total de alertas e seus locais específicos no código. A configuração de limites para esses alertas ajuda a evitar a implantação de código com secrets expostos, contribuindo para a segurança geral do projeto. Todos os alertas baseado em detecções de secrets são considerados de criticidade alta.  
    
- **Scanning de Código**  
    
  O módulo de scanning de código utiliza os alertas de Code Scanning do GitHub para identificar e gerenciar vulnerabilidades na base de código. O sistema agrega os alertas de scanning de código abertos, classifica-os por severidade e verifica se o número de alertas excede os limites estabelecidos. Essa abordagem permite que as vulnerabilidades no código sejam tratadas antes da implantação do código em uma branch produtiva (por exemplo, na main), reduzindo potenciais riscos de segurança.

Através da integração com o GitHub Actions e da gestão de diversos tipos de alertas de segurança, o [**Security Gate**](https://github.com/instriq/security-gate/) fornece um mecanismo para manter a segurança dos projetos de software durante seu ciclo de vida de desenvolvimento.

#### Integração com o GitHub

O [**Security Gate**](https://github.com/instriq/security-gate/) foi projetado para se integrar ao GitHub, permitindo que verificações de segurança sejam realizadas diretamente nas pipelines de CI/CD. Essa integração possibilita que alertas de segurança sejam monitorados e gerenciados automaticamente durante o desenvolvimento do software.

Para utilizar o [**Security Gate**](https://github.com/instriq/security-gate/) nos workflows do GitHub, é necessário configurar uma GitHub Action que execute a ferramenta durante o processo de CI/CD. Isso inclui a adição de um arquivo de workflow em YAML ao repositório e a criação de um token GitHub com as permissões adequadas.

O funcionamento do [**Security Gate**](https://github.com/instriq/security-gate/) requer a configuração de um token do GitHub com permissões específicas ([https://github.com/settings/tokens?type=beta](https://github.com/settings/tokens?type=beta)). Essas permissões são necessárias para acessar e gerenciar os diferentes tipos de alertas de segurança disponibilizados pelo GitHub.

###### *Permissões Granulares do Token*

O [**Security Gate**](https://github.com/instriq/security-gate/) exige as seguintes permissões para operar:

1. **Alertas do DependaBot:**  
     
   - **Permissão Requerida:** `security_events:read`  
   - **Justificativa:** A permissão `security_events:read` é necessária para que o módulo [SecurityGate::Engine::Dependencies](https://github.com/instriq/security-gate/blob/main/lib/SecurityGate/Engine/Dependencies.pm) possa acessar os alertas do DependaBot, monitorando vulnerabilidades nas dependências do projeto.

2. **Alertas de Verificação de secrets:**  
     
   - **Permissão Requerida:** `secrets:read`  
   - **Justificativa:** O módulo [SecurityGate::Engine::Secrets](https://github.com/instriq/security-gate/blob/main/lib/SecurityGate/Engine/Secrets.pm) necessita da permissão `secrets:read` para recuperar alertas de verificação de secrets, permitindo a identificação de informações sensíveis que possam estar expostas no repositório. 

3. **Alertas de Análise de Código:**  
     
   - **Permissão Requerida:** `security_events:read`  
   - **Justificativa:** A permissão `security_events:read` é necessária para que o módulo [SecurityGate::Engine::Code](https://github.com/instriq/security-gate/blob/main/lib/SecurityGate/Engine/Code.pm) possa acessar os alertas de análise de código e identificar potenciais vulnerabilidades na base de código.

**Resumo das Permissões Necessárias:**

- **`security_events:read`:** Necessária para a leitura de alertas do Dependabot e de análise de código.  
- **`secrets:read`:** Necessária para a leitura de alertas de verificação de secrets.

**Considerações Adicionais:**

- O token deve ter, no mínimo, acesso de leitura ao repositório.  
- Para repositórios privados, o token deve ter acesso ao repositório para que os alertas possam ser recuperados.

Ao configurar o token no GitHub, é importante garantir que essas permissões estejam habilitadas para permitir o funcionamento correto do [**Security Gate**](https://github.com/instriq/security-gate/), seguindo o princípio de menor privilégio.

##### Adição do arquivo YAML ao Repositório

Após a configuração do token, o próximo passo é a inclusão de um arquivo YAML no repositório. Esse arquivo define as etapas necessárias para a execução do [**Security Gate**](https://github.com/instriq/security-gate/) durante o processo de CI/CD. O arquivo `security-gate.yaml` deve ser criado no diretório `.github/workflows/` com o seguinte conteúdo:

```yaml
name: Security Gate - LESIS

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      MAX_CRITICAL: 1
      MAX_HIGH: 2
      MAX_MEDIUM: 3
      MAX_LOW: 4
      GITHUB_TOKEN: ${{ secrets.TOKEN }}
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Pull Docker image from GitHub Container Registry
      run: docker pull ghcr.io/instriq/security-gate/security-gate:latest

    - name: Verify security alerts from dependabot
      run: |
        docker run ghcr.io/instriq/security-gate/security-gate:latest \
        -t $GITHUB_TOKEN \
        -r ${{ github.repository }} \
        --critical $MAX_CRITICAL \
        --high $MAX_HIGH \
        --medium $MAX_MEDIUM \
        --low $MAX_LOW \
        --dependency-alerts \
        --code-alerts \
        --secret-alerts 
```

Nesse exemplo, o workflow executa o [**Security Gate**](https://github.com/instriq/security-gate/) a cada push ou pull request. O workflow realiza o checkout do código, configura as variáveis de ambiente com os limites definidos para cada tipo de alerta e, em seguida, executa a ferramenta [**Security Gate**](https://github.com/instriq/security-gate/).

A integração do [**Security Gate**](https://github.com/instriq/security-gate/) ao workflow no GitHub Actions permite a aplicação de políticas de segurança, assegurando que apenas o código que atenda aos critérios estabelecidos possa avançar na pipeline de CI/CD.

### O Futuro do Security Gate

Atualmente, o [**Security Gate**](https://github.com/instriq/security-gate/) suporta a verificação de alertas de segurança relacionados a dependências, secrets e código, assegurando que projetos atendam aos critérios de segurança estabelecidos antes da implantação. Contudo, existem algumas áreas potenciais para aprimoramento que podem ser exploradas em versões futuras para expandir suas funcionalidades e melhorar a experiência do usuário.

Uma das áreas a ser considerada é a ampliação do suporte a diferentes plataformas de gestão de repositórios como por exemplo Bitbucket e Gitlab.   
Finalmente, há a possibilidade de expandir a integração do [**Security Gate**](https://github.com/instriq/security-gate/) para incluir alertas provenientes de outras ferramentas de segurança. Isso permitiria uma análise mais abrangente, incorporando dados de segurança de diversas fontes e oferecendo uma visão mais completa do perfil de segurança do projeto.

Com essas futuras melhorias, o [**Security Gate**](https://github.com/instriq/security-gate/) poderá ampliar sua cobertura de segurança e melhorar a integração com os processos de desenvolvimento de software, contribuindo para a manutenção das práticas de segurança ao longo do ciclo de vida do software.

### Conclusão

A implementação do [**Security Gate**](https://github.com/instriq/security-gate/) tem o objetivo de melhorar a segurança das pipelines de CI/CD, integrando alertas de segurança diretamente no processo de desenvolvimento através do GitHub Actions. A ferramenta foi projetada para monitorar e gerenciar o risco de alertas de segurança relacionados a dependências, secrets e código, bloqueando a pipeline quando os critérios de segurança definidos não são atendidos.

Até o momento, o [**Security Gate**](https://github.com/instriq/security-gate/) demonstrou a capacidade de integrar verificações de segurança de maneira eficaz, alinhando-se com a filosofia do *Shift Left* ao antecipar a detecção de vulnerabilidades. A ferramenta oferece uma solução prática e acessível para garantir que apenas código que cumpre com os critérios de segurança avance na pipeline de CI/CD.

Para futuras versões, há planos para expandir as funcionalidades do [**Security Gate**](https://github.com/instriq/security-gate/), incluindo a adição de suporte a novas plataformas de gerenciamento de repositórios como Gitlab e Bitbucket. 

* Esta publicação foi escrita por Giovanni Sagioro: estudante de Ciência da Computação, buscador da sabedoria Perl e pesquisador de segurança. Focado em segurança de aplicativos, descoberta de vulnerabilidades e desenvolvimento de exploits. Como Larry Wall diz — ‘Coisas fáceis devem ser fáceis, e coisas difíceis devem ser possíveis’ — então estou tentando tornar as coisas difíceis possíveis.

---

### Referências

- [OWASP Top 10 CI/CD Security Risks](https://owasp.org/www-project-top-10-ci-cd-security-risks/)  
- [Snyk Blog: Strengthen Security in CI/CD Pipeline](https://snyk.io/pt-BR/blog/strengthen-security-ci-cd-pipeline/)  
- [Snyk Blog: Building a Security-Conscious CI/CD Pipeline](https://snyk.io/pt-BR/blog/building-security-conscious-ci-cd-pipeline/)  
- [Fortinet Cyber Glossary: Shift-Left Security](https://www.fortinet.com/br/resources/cyberglossary/shift-left-security)  
- [IBM: Shift-Left Testing](https://www.ibm.com/topics/shift-left-testing)  
- [The ‘Shift Left’ Principle](https://doi.org/10.12968/S0047-9624(22)60234-7)  
- C. Weir, S. Migues and L. Williams, "Exploring the Shift in Security Responsibility," _IEEE Security & Privacy_, vol. 20, no. 6, pp. 8-17, Nov.-Dec. 2022. doi: 10.1109/MSEC.2022.3150238
- [SonarSource: Quality Gates](https://www.sonarsource.com/learn/quality-gate/)  
- [LinearB: Quality Gates](https://linearb.io/blog/quality-gates)  
- [Trailhead Technology: Quality Gates in Software Development](https://trailheadtechnology.com/quality-gates-in-software-development/)  
- [GitHub REST API Endpoints for Dependabot](https://docs.github.com/en/rest/dependabot?apiVersion=2022-11-28)  
- [GitHub REST API Endpoints for Secret Scanning](https://docs.github.com/en/rest/secret-scanning/secret-scanning?apiVersion=2022-11-28)  
- [GitHub REST API Endpoints for Code Scanning](https://docs.github.com/en/rest/code-scanning?apiVersion=2022-11-28)
