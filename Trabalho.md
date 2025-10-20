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

   - Criando a página
     ```bash
     sudo nano /var/www/html/bloqueado.html
     ```

   - A página
     ```html
     <!DOCTYPE html>
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
4. **IP TABLES no Linux**
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
     
5. **Criar Sub-interfaces no Linux**
   - Primeiro precisa instalar o net-tools
     ```bash
     sudo apt install net-tools
     ```

   - Mostra roteador
     ```bash
     sudo ifconfig
     ```

   - Adiciona a sub-interface (O IP será diferente conforme o grupo)
     ```bash
     sudo ifconfig enp0s31f6:0 192.168.10.24 netmask 255.255.255.248
     ```

6. **Bloquear sites com Proxy**
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

Nome                     Link                                               
- **Grupo 4**  [Pagina Incial do Servidor](http://10.104.16.13/) 
- **Bloquear**  [Pagina de Bloqueio](http://10.104.16.13/bloqueado)      
 

</div>
