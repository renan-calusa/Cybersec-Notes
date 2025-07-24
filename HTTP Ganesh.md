## O que é HTTP?

O **HTTP (Hypertext Transfer Protocol)** é um protocolo de comunicação usado para transferir informações (páginas, imagens, vídeos e outros recursos) entre clientes e servidores na internet. Ele funciona no modelo **cliente-servidor**, onde o cliente (como navegadores, aplicativos móveis ou APIs) envia uma **requisição (Request)** e o servidor responde com uma **resposta (Response)**. O HTTP é **stateless (sem estado)**, o que significa que cada requisição é completamente independente das anteriores - não guarda informações sobre estados anteriores - por isso a existência de **cookies**, **local storage** e **sessions**, com o intuito de oferecer uma melhor experiência para o usuário.

O **HTTP** opera na **camada de aplicação (Application Layer)** do modelo **[OSI (Open Systems Interconnection)](https://pt.wikipedia.org/wiki/Modelo_OSI)** e por padrão, utiliza a **porta 80**, embora essa configuração possa ser alterada dependendo da configuração do servidor. O acesso aos recursos na web é realizado por meio de **[URLs (Uniform Resource Locators)](https://pt.wikipedia.org/wiki/URL)**, que indicam o endereço do recurso solicitado, incluindo o protocolo, domínio e caminho do recurso.

### Estrutura de uma URL (Uniform Resource Locator):

A **URL** é o endereço utilizado para localizar recursos na web. Ela é composta por várias partes essenciais, como no exemplo abaixo:

![[Pasted image 20250214172502.png]]

* **Protocolo/Scheme**: define como os dados serão transferidos (HTTP ou HTTPS).
* **Domínio/Host**: identifica o servidor pelo **nome do host (hostname)** quanto pelo **endereço de IP**.
* **Porta/Port**: aponta à porta utilizada pelo servidor web; separada do **Host** por um **:** (porta padrão para **HTTP** é **80**, e para **HTTPS** é **443**).
* **Caminho/Path**: aponta para um recurso específico, seja um arquivo ou uma pasta. Caso nenhum seja passado, o padrão aponta ao **index padrão** (i.e. index.html).
* **Parâmetros de Consulta/Query String**: enviam informações adicionais ao servidor, geralmente para **filtrar, pesquisar ou personalizar** o conteúdo exibido. Normalmente iniciada por um **?**, seguida de pares **chave-valor** no formato: **chave=valor** (pares separados por **&**).
* **Fragmentos/Fragments**: iniciados por **#**, direcionam a uma seção específica da página.

## HTTPS (Hyper Text Transfer Protocol Secure)

O **HTTPS (HyperText Transfer Protocol Secure)** é a versão segura do protocolo HTTP, projetado para proteger a comunicação entre cliente e servidor. A principal diferença entre HTTP e HTTPS é a **criptografia**: os dados transmitidos são cifrados, tornando-os ilegíveis para terceiros que tentem interceptá-los. A segurança é garantida por meio de protocolos de criptografia, especialmente o **[SSL (Secure Sockets Layer)](https://www.geeksforgeeks.org/secure-socket-layer-ssl/)** e seu sucessor, o **[TLS (Transport Layer Security)](https://pt.wikipedia.org/wiki/Transport_Layer_Security)**. Esses protocolos criam um **canal criptografado** entre cliente e servidor, impedindo que os dados sejam transmitidos em **texto claro (plain text)** - mesmo que os pacotes sejam interceptados, seu conteúdo estará ilegível.

- **SSL (Secure Sockets Layer)**: Foi o primeiro protocolo a oferecer segurança para o HTTP. No entanto, versões mais antigas do SSL apresentavam vulnerabilidades, levando à sua substituição pelo TLS.
- **TLS (Transport Layer Security)**: É o sucessor do SSL e atualmente é o padrão para proteger comunicações na web. Embora o termo **SSL** ainda seja popular, a maioria das conexões seguras utiliza o **TLS**.

O HTTPS utiliza um sistema de **chaves assimétricas** e **certificados digitais** emitidos por **Autoridades Certificadoras (CAs)**. Durante a conexão:

1. O cliente solicita uma conexão segura ao servidor.
2. O servidor responde com seu **certificado digital**, que contém sua chave pública.
3. O cliente valida a autenticidade do certificado.
4. Uma chave de sessão é trocada para [criptografia simétrica](https://www.ibm.com/br-pt/think/topics/symmetric-encryption), garantindo que os dados sejam seguros e rápidos durante a transmissão.

`(Wireshark example)`
## Como o HTTP Request e Response funcionam?

Como já citado, o **HTTP** funciona através de um modelo simples de **requisição (request)** e **resposta (response)** no padrão **cliente-servidor**. O **Request** é uma mensagem enviada pelo cliente ao servidor para solicitar um recurso. Ela é composta por:

- Linha de Requisição
- Cabeçalhos/Headers
- Corpo/Body (opcional)

Já a **resposta**

`(Show an example in Burp Suite)`

### What composes a HTTP Request and Response?
5. HTTP Method
6. HTTP path
7. HTTP protocol
8. HTTP Request and Response Headers
9. HTTP Body - page itself (optional)

### HTTP Methods:
10. GET: Retrieves data from the server
11. POST: Submit data to the server - Adding something - new post
12. PUT: Update data on the server - Edit things
13. DELETE: Deletes data from the server
14. OPTIONS
15. HEAD

### HTTP Headers:
What each thing means

### HTTP Status Codes:
100, 200, 300, 400, 500

Most seen ones:
![[Pasted image 20250214163316.png]]
### HTTP Flow

Difference between each version protocol

`(Interact with Netcat, Burp suite, Curl & Developer Mode - How to use it)`




## Security Related:

### Enumerating - whatweb? TCM Sec

### Vulnerabilities Related? - Changing requests (IDOR, SQL Injection, etc.)