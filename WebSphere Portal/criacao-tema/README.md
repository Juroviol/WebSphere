# Criação de tema customizado para o WebSphere Portal

Segundo a IBM, a melhor maneira de criar um tema customizado, é copiar o tema padrão do próprio WebSphere Portal, para que na criação do tema customizado, não seja esquecido de incluir as configurações necessárias.

> The best way to start creating your own custom theme is by copying the portal WebSphere® Portal theme. This ensures that your theme has all the required elements for the theme to function.

Para se fazer a cópia do tema padrão do WebSphere Portal precisaremos acessar o diretório onde se encontra o tema dentro de uma instância do WebSphere Portal. A leitura desse diretório se dá através do protocolo WebDAV (Web Distributed Authoring and Versioning) que é um protocolo de transferência de arquivos, semelhantemente ao protocolo FTP, com a vantagem de conter informações sobre a versão e o autor dos arquivos e mudanças.

Para acessar o diretório remoto e transfência dos arquivos do tema padrão do WebSphere Portal é preciso ter um cliente WebDAV.  Sugiro utilizar o WinSCP que é o programa mais conhecido, também muito utilizado para transferência de arquivos via protocolo FTP.

Abrir o WinSCP e configurar uma nova conexão definindo os seguintes parâmetros:

![Imagem nova conexão no WinSCP](imagens/nova-conexao-webdav.png)

- Host
- Porta
- Usuário
- Senha
- Caminho diretório remoto
- Caminho diretório local 

Onde **Caminho diretório remoto** é */wps/mycontenthandler/dav/themelist* que é o diretório onde conterá os arquivos do tema padrão do WebSphere Portal, e 

![Imagem avançado nova conexão no WinSCP](imagens/nova-conexao-webdav-avancado.png)
