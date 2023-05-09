# Site na Nuvem (AWS)

## 1° Etapa ( Resolver o problema )

<p> Inicialmente é necessário analisar qual o tipo de problema a empresa está passando<br>
criei uma situação fictícia para poder resolver de forma rápida e prática através da AWS</p><br>
<h2>Problema: </h2><br>
    <code> Uma empresa chamada VFX utilizava um software de desenvolvimento próprio<br>
         que estava apresentando lentidão e quedas constantes e isso ocasionava um<br>
         grande impacto nos resultados do negócio. <code>
    
<details>
<summary><h2> Problemas Técnicos</h2></summary>
- Estava junto com outras VM´s
- Quedas no sistema
- Apresentava problemas de hardware
- Precisava trocar o servidor já que tinha 5 anos
- Usuários reclamando de lentidão
- Backup ineficiente e perda de dados
- Custos altos de manutenção e contratação
</details>

<details>
<summary><h2> Impactos no negócio</h2></summary>
- Equipe sem poder trabalhar
- Clientes insatisfeitos por motivo da demora
- Os usuários culpam o sistema
- Perda de clientes
- Atrasos na logística
- Perdas financeiras
- Produtividade da equipe
</details>

<details>
<summary><h2> Impacto para infra</h2></summary>
- Muitos recursos para gerenciar
- Falta de equipamentos de qualidade
- Incidentes fora do horário (dor de cabeça)
- Viver apagando incêndio e sem tempo para capacitação
- Stress constante e quase sem vida social
- Só recebem críticas e não se sentem valorizados
- Desmotivado com a profissão
</details>

<details>
<summary><h2> Impacto para Devs</h2></summary>
- Necessidade de gerenciar infra
- Deficuldade no deploy
- Muitos projetos inviabilizados
- Muito suporte
- Dificuldade de manter a alta disponibilidade
- Problemas para criar ambientes de homologação
</details>
    
    
## Detalhes da instância AWS (Amazon Web Services)
Inicialmente foi necessário criar uma instância EC2 do tipo Ubuntu server Versão 20.04LTS<br>
abaixo terá a tabela completa, lembre de adicionar um ip públixo a instância<br>
    
Tipo da Instância | Tipo de Chave | Config. de Rede | Armazenamento
---|---|---|---
T2 Micro | .pem | 0.0.0.0/0 | 8GiB (Gp2)
Family | .ppk | Seu_IP |  
    
### Portas da instância (entradas e saídas)

TCP | UDP | NFS | HTTP | SSH | HTTPS
---|---|---|---|---|---
8080 | 2049 | 2049 | 80 | 22 | 443
111 | 111 | 
443 | 
80 |
    
    
<a href="https://cdn-icons-png.flaticon.com/512/5610/5610989.png" target="_blank"><img height="20" width="20" src="https://cdn-icons-png.flaticon.com/512/5610/5610989.png" target="_blank"></a>  Observação: A configuração de rede está com 0.0.0.0/0 para facilitar o acesso mas<br>
não é uma boa prática utilizar essa cong. de rede pois pode deixar a instância vulnerável<br>
estou utilizando porquê é um ambiente apenas para testes, nada de confidêncial está em risco<br>
a instância foi criada apenas para fins práticos.
</div><br>
    
# Passo a passo para a execução do projeto
    
Após a configuração da instãncia EC2 na opção de <strong>Dados do Usuário</strong><br>
Será adicionado o seguinte código bash para adiantar algumas coisas<br>
    
    
    #!/bin/bash
    sudo apt-get update -y
    sudo apt-get install apache2 php7.4 libapache2-mod-php7.4 php7.4-common php7.4-curl php7.4-intl php7.4-mbstring       php7.4-json php7.4-xmlrpc php7.4-soap php7.4-mysql php7.4-gd php7.4-xml php7.4-cli php7.4-zip wget mysql-client       unzip git binutils ruby -y
    sudo systemctl start apache2
    sudo systemctl enable apache2
    sudo systemctl restart apache2
    wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
    chmod +x ./install
    sudo ./install auto
    sudo service codedeploy-agent start
    sudo chmod 777 /etc/init.d/codedeploy-agent
    sudo wget https://s3.sa-east-1.amazonaws.com/pages.cloudtreinamentos.com/aws/bootcamp/Bootcamp9.zip 
    sudo unzip -o Bootcamp9.zip -d /var/www/html/
    sudo rm /var/www/html/index.html
    sudo chmod -R 777 /var/www/html
    
Após isso pode executar a instância, assim que as verificações estarem completas entre no<br>
navegador e coloque o ip público da instãncia em execução para ver o site que foi adicionao<br>
na máquina no ar.
    
    
# 2° Etapa ( Disponiblidade )

<h2> a partir daqui será configurado o banco de dados, de maneira geral é bem simples<br>
     após a 1° etapa está pronta vamos configurar e deixar no ar o banco de dados criado<br>
    através do RDS na AWS.</h2>
    
    
## Detalhes do Banco de Dados RDS
    
.|.|.|.
---|---|---|---
Tipo de Banco de Dados | Nome do BD | Tipo de criação | Modelo de BD
Relacional | banco | Padrão | MySQL Community
    
    
<h3> Ao selecionar a opção <strong>Criar Banco de dados</strong> você irá selecionar<br>
    as seguintes opções: </h3>

- Cração Padrão
- MySQL ( Edição MySQL Community )
- Versão 8.0.32
- Modelos ( Nível Gratuito )
- identificados ( ou deixa o padrão ou coloque o nome de preferir )
- Nome do usuário (admin ou de sua escolha)
- Senha principal ( Recomendo utilizar uma senha numérica apenas pois a aplicação está com bug )
- Tipo de Armazenamneto ( SSD )
- Escolher qual a subnet a ser usada ( OBS: é necessário ter subnets em 2 AZ´s )
- Acesso público ( Não )
- Grupo de VPC ( Pode ser Default )
- Autenticação de banco de dados ( Autenticação de senha )
- Criar Banco de Dados

OBS: Após criar o Banco de Dados espera de 3 a 5 minutos, demora um pouco.
