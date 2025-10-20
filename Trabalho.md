<h2 align="center">TRABALHO REDES - BLOQUEIO DE SITES POR PROXY SQUID E EXIBIÇÃO DE SERVIDOR WEB LINUX</h2>
<p align="center">

---

### Sumário
1. [Integrantes do Grupo](#integrantes-do-grupo)
2. [Passo a Passo](#passo-a-passo)
3. [Salvar](#Salvar)

---

### Integrantes do Grupo
- **Grupo 4:** José Otávio e Arthur Spironelo
> **Nota:** Todas as instruções a seguir devem ser executadas no terminal do Linux.
> **Ferramentas:** SSH, Linux, Windows, Apache 2, Sub-interface e Proxy(SQUID e IP TABLES)

---

## Passo a passo
1. **Planejar as redes**
   - Definir a topologia de rede, incluindo dispositivos e conexões
   - **LAN:** 192.168.10.24/29
   - **WAN:** 200.10.10.12/30


2. **Instalar o Apache 2 no Linux**
   - Para instalação, siga as orientações abaixo:
     ```bash
     sudo apt update
     sudo apt install apache2
     ```

   - Inicie o serviço
     ```bash
     sudo systemctl start apache2
     ```
     
   - Criando a página Inicial do servidor
     ```bash
     sudo nano /var/www/html/index.html
     ```

   - A página Inicial do servidor
     ```html
      <!DOCTYPE html>
      <html lang="pt-BR">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Servidor Apache2 - Grupo 4</title>
          <style>
              * {
                  margin: 0;
                  padding: 0;
                  box-sizing: border-box;
              }
      
              body {
                  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
                  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                  min-height: 100vh;
                  display: flex;
                  align-items: center;
                  justify-content: center;
                  padding: 20px;
              }
      
              .container {
                  background: rgba(255, 255, 255, 0.95);
                  border-radius: 20px;
                  padding: 50px;
                  max-width: 800px;
                  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
                  text-align: center;
                  animation: fadeIn 0.8s ease-in;
              }
      
              @keyframes fadeIn {
                  from {
                      opacity: 0;
                      transform: translateY(-20px);
                  }
                  to {
                      opacity: 1;
                      transform: translateY(0);
                  }
              }
      
              .logo {
                  width: 120px;
                  height: 120px;
                  margin: 0 auto 30px;
                  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                  border-radius: 20px;
                  display: flex;
                  align-items: center;
                  justify-content: center;
                  font-size: 48px;
                  font-weight: bold;
                  color: white;
                  box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
              }
      
              h1 {
                  color: #333;
                  font-size: 2.5em;
                  margin-bottom: 10px;
              }
      
              .subtitle {
                  color: #667eea;
                  font-size: 1.5em;
                  font-weight: 600;
                  margin-bottom: 30px;
              }
      
              .status {
                  background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);
                  color: white;
                  padding: 15px 30px;
                  border-radius: 50px;
                  display: inline-block;
                  font-weight: bold;
                  font-size: 1.1em;
                  margin-bottom: 30px;
                  box-shadow: 0 5px 15px rgba(17, 153, 142, 0.3);
              }
      
              .info {
                  background: #f8f9fa;
                  border-left: 4px solid #667eea;
                  padding: 20px;
                  margin: 30px 0;
                  text-align: left;
                  border-radius: 5px;
              }
      
              .info h3 {
                  color: #667eea;
                  margin-bottom: 15px;
                  font-size: 1.2em;
              }
      
              .info p {
                  color: #555;
                  line-height: 1.6;
                  margin-bottom: 10px;
              }
      
              .footer {
                  margin-top: 40px;
                  padding-top: 20px;
                  border-top: 2px solid #eee;
                  color: #777;
                  font-size: 0.9em;
              }
      
              .apache-logo {
                  font-weight: bold;
                  color: #d12127;
              }
          </style>
      </head>
      <body>
          <div class="container">
              <div class="logo">G4</div>
              
              <h1>Servidor Apache2</h1>
              <div class="subtitle">Grupo 4</div>
              
              <div class="status">✓ Servidor Funcionando</div>
              
              <div class="info">
                  <h3>📋 Informações do Servidor</h3>
                  <p><strong>Servidor Web:</strong> Apache2</p>
                  <p><strong>Sistema Operacional:</strong> Linux</p>
                  <p><strong>Grupo:</strong> Grupo 4</p>
                  <p><strong>Nomes:</strong> Arthur Spironello Otavio Baggio</p>
              </div>
              
              
              <div class="footer">
                  <p>Powered by <span class="apache-logo">Apache2</span> on Linux</p>
                  <p>&copy; 2025 Grupo 4 - Todos os direitos reservados</p>
              </div>
          </div>
      </body>
      </html>
     ```

   - Para salvar e sair da "criação html"
     ```bash
     Ctrl + X
     ou
     Y + Enter
     ```

   - Criando a página de bloqueio
     ```bash
     sudo nano /var/www/html/bloqueado.html
     ```

   - A página de bloqueio
     ```html
      <!DOCTYPE html>
            <html lang="pt-BR">
            <head>
              <meta charset="UTF-8">
              <meta name="viewport" content="width=device-width, initial-scale=1.0">
              <title>Acesso Bloqueado</title>
              <style>
                * {
                  margin: 0;
                  padding: 0;
                  box-sizing: border-box;
                }
      
             body {
               background: linear-gradient(135deg, #c31432 0%, #240b36 100%);
               color: white;
               font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
               min-height: 100vh;
               display: flex;
               align-items: center;
               justify-content: center;
               padding: 20px;
             }
         
             .container {
               max-width: 600px;
               width: 100%;
               text-align: center;
             }
         
             .image-wrapper {
               margin-bottom: 30px;
               position: relative;
               animation: fadeInDown 0.8s ease;
             }
         
             .image-wrapper img {
               max-width: 100%;
               height: auto;
               border-radius: 15px;
               box-shadow: 0 15px 50px rgba(0, 0, 0, 0.5);
               border: 3px solid rgba(255, 255, 255, 0.1);
             }
         
             .content-box {
               background: rgba(0, 0, 0, 0.7);
               backdrop-filter: blur(10px);
               padding: 40px 30px;
               border-radius: 20px;
               box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
               border: 1px solid rgba(255, 255, 255, 0.1);
               animation: fadeInUp 0.8s ease;
             }
         
             .warning-icon {
               font-size: 80px;
               margin-bottom: 20px;
               animation: pulse 2s infinite;
             }
         
             h1 {
               font-size: 42px;
               margin-bottom: 20px;
               font-weight: 700;
               letter-spacing: -1px;
             }
         
             .message {
               font-size: 18px;
               line-height: 1.6;
               margin-bottom: 15px;
               opacity: 0.95;
             }
         
             .category {
               display: inline-block;
               background: rgba(255, 255, 255, 0.15);
               padding: 8px 20px;
               border-radius: 25px;
               margin: 20px 0;
               font-weight: 600;
               font-size: 16px;
             }
      
             .footer {
               margin-top: 25px;
               padding-top: 25px;
               border-top: 1px solid rgba(255, 255, 255, 0.2);
               font-size: 15px;
               opacity: 0.8;
             }
         
             @keyframes fadeInDown {
               from {
                 opacity: 0;
                 transform: translateY(-30px);
               }
               to {
                 opacity: 1;
                 transform: translateY(0);
               }
             }
         
             @keyframes fadeInUp {
               from {
                 opacity: 0;
                 transform: translateY(30px);
               }
               to {
                 opacity: 1;
                 transform: translateY(0);
                 }
             }
      
             @keyframes pulse {
               0%, 100% {
                 transform: scale(1);
                 opacity: 1;
               }
               50% {
                 transform: scale(1.1);
                 opacity: 0.8;
               }
             }
         
             @media (max-width: 600px) {
               h1 {
                 font-size: 32px;
               }
         
               .warning-icon {
                 font-size: 60px;
               }
         
               .content-box {
                 padding: 30px 20px;
               }
         
               .message {
                 font-size: 16px;
               }
               }
           </style>
         </head>
         <body>
           <div class="container">
             <div class="image-wrapper">
               <img src="abordagem.jpg" alt="Acesso Bloqueado">
          </div>
         
             <div class="content-box">
               <div class="warning-icon">⚠️</div>
               <h1>Acesso Bloqueado</h1>
         
               <p class="message">
                 O acesso a este site foi bloqueado pela política de segurança da rede.
               </p>
         
               <div class="category">
                 🚫 Apostas ou Conteúdo Adulto
               </div>
         
               <div class="footer">
                 <p>Para mais informações, entre em contato com o administrador da rede.>
               </div>
             </div>
           </div>
         </body>
         </html>
     ```

   - Para salvar e sair da "criação html"
     ```bash
     Ctrl + X
     ou
     Y + Enter
     ```

   - Configurando permissões
     ```bash
     sudo chown -R www-data:www-data /var/www/html/
     sudo chmod -R 755 /var/www/html/
     ```

   - (Opcional) Habilitar no firewall
     ```bash
     sudo ufw allow 'Apache'
     ```

   - Abrir Site criado: http://10.104.16.13/grupo4
     
3. **Instalar o SSH no Linux**
   - Para instalação, siga as orientações abaixo
     ```bash
     sudo apt-get update
     sudo apt-get upgrade
     sudo apt-get install openssh-client
     ```

   - Criar usuário
     ```bash
     sudo adduser username
     ```

   - Adicionar usuário na lista do SUDO
     ```bash
     sudo usermod -aG sudo username
     ```

   - Entra como super usuário
     ```bash
     sudo su
     ```

   - Logar usuário / Mudar usuário
     ```bash
     sudo su username
     ```

4. **Criar Sub-interfaces no Linux**
   - Primeiro precisa instalar o net-tools
     ```bash
     sudo apt install net-tools
     ```

   - Mostra roteador
     ```bash
     sudo ifconfig
     ```

   - Adiciona a sub-interface (O IP será diferente conforme o grupo)
     Adicionando o IP para a rede LAN com o Windows
     ```bash
     sudo ifconfig enp0s31f6:0 192.168.10.25 netmask 255.255.255.248
     ```
     Adicionando o IP para a rede WAN com o Servidor 
      ```bash
     sudo ifconfig enp0s31f6:0 200.10.10.14 netmask 255.255.255.252
     ```

5. **Bloquear sites com Proxy**
   - Baixar o SQUID
     ```bash
     sudo apt-get install squid
     ```

   - Verificar a Instalação
     ```bash
     sudo service squid status
     ```

   - Configurar o SQUID
     ```bash
     cd /etc/squid
     ```
     
   - Fazer Backup do SQUID
     ```bash
     sudo cp squid.conf squid.conf.backup
     ```
     
   - Apagar o SQUID e Criar Novo
     ```bash
     sudo rm squid.conf
     sudo nano squid.conf
     ```

   - Entrar no Config
     ```bash
     cd /etc/squid
     ```

   - Criar arquivo
     ```bash
     sudo touch /etc/squid/sitesbloqueados.txt
     ```
     
   - Entrar no Arquivo
     ```bash
     sudo nano /etc/squid/sitesbloqueados.txt
     ``` 

   - Configurações do SQUID
     ```bash
      http_port 3128
      
      # ACLs
      acl sites_bloqueados url_regex -i "/etc/squid/sitesbloqueados.txt"
      
      # Página de erro personalizada
      deny_info http://10.104.16.13/bloqueado sites_bloqueados
      
      # Regras de acesso
      http_access deny sites_bloqueados
      http_access allow all
     ```


   - Reiniciar SQUID / Qualquer modificação deve ser reiniciado o SQUID
     ```bash
     sudo systemctl stop squid
     sudo systemctl start squid
     ```
     
6. **Acessando o site pelo Windows 11**
   - Abra as configurações.
   - Entre em **Rede e Internet**.
      - Clique em **Proxy**.
      - Desmarque a opção **Detectar configurações automaticamente**.
   - Em configurações de proxy manual, clique na opção **Editar**.
      - Clique em **Ativado**.
   - Em **Endereço de proxy**, coloque o IP da sua máquina Linux e, na **Porta**, coloque 3128.
---

7. **Configurando Servidor Linux**
   Em um computador linux separado faça a intalação do SSH e a adição do IPS atraves das sub-interfaces (seguindo o passo a passo das etapas 3 e 4 respectivamente)

   Obs: Os comandos para os ips deste servidor para esta WAN será
     ```bash
     sudo ifconfig enp0s31f6:0 200.10.10.13 netmask 255.255.255.252
     ```
9. 
   Em um Computador separado e Linux faça a instalação do Ip Tables
   
   - Instalação
     ```bash
     sudo apt install iptables

     sudo apt install iptables-persistent
      
     sudo systemctl enable netfilter-persistent
     ```

     - Cria rota no IP TABLES
     ```bash
     IP route

     route add - net 0.0.0.0 netmask 0.0.0.0 GW IP
     ```
     Colocar as rotas dos endereços referentes ao computador de cada grupo
     
### Salvar
Endereços que começam com 172 são endereços inválidos que não navegam pela internet.
Linux: quando criar sub-interface não vai permitir. IPV4 alterar 0 para 1.

- **Endereço IPv4:** 
- **Máscara de Sub-rede:** 
- **Gateway Padrão:** 
- **IP Original** aaa

---

<h2 align="center">Endereços dos Sites</h2>

<div align="center">

Nome            |         Link                                               
- **Grupo 4**  [Pagina Incial do Servidor](http://10.104.16.13/) 
- **Bloquear**  [Pagina de Bloqueio](http://10.104.16.13/bloqueado)      
 

</div>
