# Especificação Técnica: Sistema de Carrossel Dinâmico - Stand Farense

**Projeto:** Tecnologias Web - TP2
**Desenvolvido por:** João Azul (a91152)
**Versão:** 1.0.0

---

## 1. Sumário Executivo
Este projeto consiste no desenvolvimento de uma aplicação web *Full-Stack* para apresentação de um carroussel de imagens dinâmica. A solução implementa um carrossel interativo que carrega conteúdos do servidor em tempo real e possui um sistema de *backend* para registo de auditoria (logs de visualização) e métricas de comportamento do utilizador (contagem de cliques), sem persistência em base de dados relacional.

## 2. Arquitetura do Sistema

A solução segue uma arquitetura Cliente-Servidor, onde a comunicação de dados é gerida de forma assíncrona para garantir a fluidez da navegação.

### 2.1. Stack Tecnológico
* **Backend:** Node.js com *framework* Express (gestão de rotas e servidor HTTP).
* **Frontend:** HTML5, CSS3 (Design Responsivo) e JavaScript (ES6).
* **Comunicação Assíncrona:** **AJAX (Asynchronous JavaScript and XML)** utilizando o objeto `XMLHttpRequest`. Esta tecnologia é fundamental para enviar e receber dados do servidor sem recarregar a página.
* **Armazenamento:** Sistema de ficheiros local (`JSON` e `TXT`) para persistência de logs.

---


---

## 4. Implementação Frontend e AJAX

A lógica do lado do cliente (script.js) é responsável pela interatividade. 
A tecnologia AJAX é utilizada em três momentos cruciais do ciclo de vida da aplicação:

### 4.1. Inicialização (Carregamento Dinâmico)

    Ação: Ao abrir a página.

    Método AJAX: GET /api/imagens

    Descrição: O script envia um pedido assíncrono ao servidor para obter a lista de imagens existentes na pasta. 
    Isto permite adicionar novas fotos à pasta sem alterar uma única linha de código no frontend.

### 4.2. Registo de Interação (Logging)

    Ação: Sempre que uma imagem é apresentada no carrossel.

    Método AJAX: POST /api/log

    Descrição: É enviado um pacote de dados (JSON) contendo a ação ("ver"), o nome da imagem e a data. 
    O envio é feito em "segundo plano", não interrompendo a navegação do utilizador.

### 4.3. Métricas de Navegação (Analytics)

Ação: Clique nos botões "Anterior" ou "Seguinte".

Método AJAX: POST /api/click

Descrição: O sistema regista a intenção do utilizador (direção do clique). O servidor processa estes dados para calcular estatísticas de uso (ex: preferência por avançar ou recuar).

## 5. Especificação da API (Backend)

O servidor (server.js) expõe endpoints REST para servir o frontend.

### 5.1. Rota: GET /api/imagens

Função: Leitura do diretório de imagens.

Processo: Utiliza o módulo fs (File System) para ler a pasta public/images, filtra ficheiros com extensões válidas (.png, .jpg, etc.) e retorna um array JSON para o cliente.

### 5.2. Rota: POST /api/log

Função: Auditoria de acessos.

Processo: Recebe o objeto JSON do frontend, adiciona o endereço IP do cliente e anexa a entrada ao ficheiro logs.txt.

### 5.3. Rota: POST /api/click

Função: Contabilização estatística.

Processo:

        Lê o ficheiro cliques.txt.

        Calcula o total acumulado de cliques à esquerda e à direita.

        Adiciona a nova interação.

        Reescreve o ficheiro com o novo sumário estatístico.

## 6. Interface e Design

    Estilo: Utilização de Glassmorphism (efeito de vidro fosco com backdrop-filter) para uma estética moderna.

    Responsividade: O CSS adapta o tamanho do carrossel e dos botões para dispositivos móveis (ecrãs inferiores a 520px).

## 7. Instruções de Instalação

    Instalar Dependências: Na raiz do projeto, executar:
    Bash

npm install express

Executar Servidor:
Bash

node server.js

Acesso: Abrir o navegador em http://localhost:3000. As imagens devem estar colocadas na pasta public/images para serem carregadas automaticamente.
