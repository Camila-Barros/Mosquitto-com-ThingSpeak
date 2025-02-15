# Mosquitto-com-ThingSpeak
Projeto testando a conexão do protocolo Mosquitto com a plataforma ThingSpeak

## Ferramentas

- [Eclipse Mosquitto](https://mosquitto.org/download/)
- [ThingSpeak](https://thingspeak.mathworks.com/)


## Preparar o ThingSpeak:

<b>Passo 1:</b> Criar uma Conta no Thingspeak
1. Acesse [https://thingspeak.com](https://thingspeak.mathworks.com/)

<b>Passo 2:</b> Criar um Canal:
1. No painel do Thingspeak, clique em New Channel.
2. Preencha os campos:
    - <i>Name:</i> Nome do canal (ex: "IoT Device").
    - <i>Description:</i> Descrição do canal (opcional).
    - <i>Field 1:</i> Tópico 1, nomeie como "Temperatura".
    - <i>Field 2:</i> Tópico 2, nomeie como "Corrente".
    - Marque a opção Make Public se quiser que o canal seja público.
3. Clique em Save Channel.
4. Anote o seu "<i>Channel ID</i>" que aparecerá no início da janela.

<b>Passo 3:</b> Configurar o MQTT:
1. Vá para a aba <i>Devices</i> > <i>MQTT</i>.
2. Clique em <i>"Add a new device"</i>.
3. Em Device Information, preencha os campos:
    - <i>Name:</i> Nome do seu broker MQTT (ex: "Teste MQTT").
    - <i>Description:</i> Descrição do broker (opcional).
4. Em Authorize channels to access:
    - Selecione o seu canal.
    - Clique em Add Channel.
5. Clique em Add Device.
6. Anote as credenciais:
    - <i>Client ID:</i> Seu ID de usuário do Thingspeak.
    - <i>Username:</i> Seu nome de usuário do Thingspeak.
    - <i>Password:</i> Senha gerada pelo Thingspeak.


## Preparar o Mosquitto:

<b>Passo 1:</b> Criar um novo usuário:

1. Abra o Prompt de Comando e entre na pasta do msoquitto:

  ```bash
    cd "C:\Program Files\mosquitto"
    #colocar o caminho do local onde está a pasta
  ```

2. Criar o novo usuário e senha:
    - USUARIO: colocar o <b>Username</b> informado, do device MQTT criado no ThingSpeak
    - SENHA: colocar a <b>Password</b> informada, do device MQTT criado no ThingSpeak

  ```bash
    mosquitto_passwd -b passwd.txt USUARIO "SENHA"
    #conferir se o nome do arquivo de senhas é passwd
  ```

3. Verificar se o novo usuário foi criado, listando todos os usuários cadastrados:

  ```bash
    type passwd.txt
  ```

<b>Passo 2:</b> Reiniciar o Mosquitto para aplicar as mudanças:

  ```bash
    net stop mosquitto
    net start mosquitto
  ```


## Enviando dados do Mosquitto para o ThingSpeak:

Abra uma janela do Prompt de Comando e subescreva uma mensagem:

  ```bash
    mosquitto_pub -h ENDERECO -p PORTA -t "TOPICO" -i "ID" -u USUARIO -P "SENHA" -t "TOPICO" -m "MENSAGEM"
  ```

   - ENDEREÇO: mqtt3.thingspeak.com
   - PORTA: 1883
   - ID: é o <b>Client ID</b> informado, do device MQTT criado no ThingSpeak
   - USUARIO: é o <b>Username</b> informado, do device MQTT criado no ThingSpeak
   - SENHA: é a <b>Password</b> informada, do device MQTT criado no ThingSpeak
   - TOPICO: será "channels/CHANNEL_ID/publish", substituindo CHANNEL_ID pelo seu ID do canal.
   - MENSAGEM: será "field1=xxx&field2=xxx", sendo
      - TOPICO 1: field1=xxx , substituindo xxx pelo valor do seu tópico 1
      - TOPICO 2: field2=xxx , substituindo xxx pelo valor do seu tópico 2



Por exemplo:

  ```bash
    mosquitto_pub -h mqtt3.thingspeak.com -p 1883 -i "FQguAjUGFx05OjQxMzYQPSk" -u FQguAjUGFx05OjQxMzYQPSk -P "dxSKVC1cCoE4LsjE9t5Y4d/y" -t "channels/2838108/publish" -m "field1=50&field2=40" 
  ```

   - ENDEREÇO: mqtt3.thingspeak.com
   - PORTA: 1883
   - ID: FQguAjUGFx05OjQxMzYQPSk
   - USUARIO: FQguAjUGFx05OjQxMzYQPSk
   - SENHA: dxSKVC1cCoE4LsjE9t5Y4d/y
   - TOPICO: channels/2838108/publish
   - MENSAGEM: field1=50&field2=40
      - TOPICO 1: temperatura de 50
      - TOPICO 2: corrente de 40

Os dados enviados aparecerão na página do seu canal do ThingSpeak:

![image](https://github.com/Camila-Barros/Mosquitto-com-ThingSpeak/blob/main/ImgTopicos.png)





## Autora

Eng. Camila Cabral de Barros

Mestranda em Inovação Tecnológica pela UNIFESP

[Lattes](http://lattes.cnpq.br/2066462797590469)
