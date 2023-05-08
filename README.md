### Site na Nuvem (AWS)

### 1° Etapa ( Rsolver o problema )

<p> Inicialmente é necessário analisar qual o tipo de problema a empresa está passando<br>
criei uma situação fictícia para poder resolver de forma rápida e prática através da AWS</p><br>
<h2>Problema: </h2><br>
    <code> Uma empresa chamada VFX utilizava um software de desenvolvimento próprio<br>
         que estava apresentando lentidão e quedas constantes e isso ocasionava um<br>
         grande impacto nos resultados do negócio. <code>
<details>
<summary><h2> Problemas Técnicos (Fictício)</h2></summary>
<p> - Estava junto com outras VM´s </p>
<p> - Quedas no sistema </p>
<p> - Apresentava problemas de hardware </p>
<p> - Precisava trocar o servidor já que tinha 5 anos </p>
<p> - Usuários reclamando de lentidão</p>
<p> - Backup ineficiente e perda de dados</p>
<p> - Custos altos de manutenção e contratação </p>
</details>

<details>
<summary><h2> Impactos no negócio (Fictício)</h2></summary>
<p> - Equipe sem poder trabalhar </p>
<p> - Clientes insatisfeitos por motivo da demora</p>
<p> - Os usuários culpam o sistema </p>
<p> - Perda de clientes </p>
<p> - Atrasos na logística</p>
<p> - Perdas financeiras</p>
<p> - Produtividade da equipe </p>
</details>

<details>
<summary><h2> Impacto para infra (Fictício)</h2></summary>
<p> - Muitos recursos para gerenciar </p>
<p> - Falta de equipamentos de qualidade </p>
<p> - Incidentes fora do horário (dor de cabeça) </p>
<p> - Viver apagando incêndio e sem tempo para capacitação </p>
<p> - Stress constante e quase sem vida social</p>
<p> - Só recebem críticas e não se sentem valorizados</p>
<p> - Desmotivado com a profissão </p>
</details>

<details>
<summary><h2> Impacto para Devs (Fictício)</h2></summary>
<p> - Necessidade de gerenciar infra </p>
<p> - Deficuldade no deploy </p>
<p> - Muitos projetos inviabilizados </p>
<p> - Muito suporte </p>
<p> - Dificuldade de manter a alta disponibilidade</p>
<p> - Problemas para criar ambientes de homologação</p>
</details>
    
    
### Detalhes da instância AWS (Amazon Web Services)
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
    
### Passo a passo para a execução do projeto
    
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
