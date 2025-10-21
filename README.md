# Configuração de SSL Local com Nginx e mkcert

Este projeto mostra como configurar um certificado SSL local para rodar um site com HTTPS usando o Nginx e o mkcert.

# Passos realizados:

Atualizei os pacotes do sistema:
    sudo apt update

Instalei as dependências necessárias:
    sudo apt install libnss3-tools
    sudo apt install mkcert

Verifiquei se o mkcert foi instalado corretamente:
    mkcert --version

Gerei os certificados para o domínio local:
    mkcert gcsi.local localhost 127.0.0.1 ::1

Instalei o certificado raiz local do mkcert:
    mkcert -install

Criei uma pasta para armazenar os certificados no Nginx:
    cd /etc/nginx
    mkdir ssl

Movi os arquivos gerados para dentro da pasta SSL do Nginx:
    cd /home/silvadev/.ssh
    sudo mv gcsi.local+3.pem /etc/nginx/ssl/
    sudo mv gcsi.local+3-key.pem /etc/nginx/ssl/

Editei o arquivo de configuração padrão do Nginx:
    /etc/nginx/sites-available/default
    nano default

E deixei assim:

    server {
    listen 443 ssl;

    root /var/www/atlas-vital;
    index index.html index.htm;
    server_name localhost;

    ssl_certificate /etc/nginx/ssl/gcsi.local+3.pem;
    ssl_certificate_key /etc/nginx/ssl/gcsi.local+3-key.pem;

    location / {
        try_files $uri $uri/ =404;
    }
}

Após salvar as alterações e reiniciar o Nginx, o site local passou a funcionar com HTTPS,

