### **1. Inserindo front no AWSAmplify**
![IAM config 1](img/create_topic.png)
---
### **2. Publish Message**
![IAM config 1](img/plubish_message_p1.png)
--
![IAM config 1](img/plubish_message_p2.png)
--

### **3. Criar as Funções Lambda**

#### **Função Lambda em Python**

1. Acesse o **AWS Lambda** e clique em **Create Function**.
2. Escolha **Author from Scratch**.
3. Dê um nome à sua função, por exemplo: `ProcessaMensagemSNSPython`.
4. Selecione **Python 3.14** como a linguagem de runtime.
5. Role até as permissões e configure permissões básicas que permitam à função Lambda ser acionada pelo SNS.
6. Clique em **Create Function**.

---
### **4. Adicionando Trigger**
- O trigger vai ser o Tópico do SNS ja criado
 
- Ajuste no código
![IAM config 1](img/3.ajuste_no_codigo.png)
---
- Tirando a prova com cloudwatch se chegou algum log
![IAM config 1](img/4.cloudwatch.png)
---

#### **Publicação Usando AWS CLI**
Se preferir usar a CLI da AWS, publique uma mensagem no SNS com o seguinte comando:

```bash
export AWS_PROFILE=luanr
```
- pego o código arn lá tópico sns
```bash
aws sns publish \
    --topic-arn arn:aws:sns:us-east-1:354918368386:MeuTopicoJornadaDeDados \
    --message '{"nome": "Luan", "aula": "aua14-testando-envio"}' \
    --region us-east-1
```

#### **Publicação Usando Python (com boto3)**

- Até por python também tem q dar o export
```bash
export AWS_PROFILE=luanr

Aqui está um exemplo de código Python para publicar uma mensagem no SNS:

```python
import boto3
import json

# Criar o cliente SNS
sns_client = boto3.client('sns', region_name='us-east-2')

# Criar a mensagem JSON
message = {
    "nome": "Luciano",
    "aula": "aula14"
}

# Publicar a mensagem no tópico SNS
response = sns_client.publish(
    TopicArn='arn:aws:sns:us-east-1:354918368386:MeuTopicoJornadaDeDados',
    Message=json.dumps(message)
)

print(response)
```

### ** Escrevendo o Código Lambda**

#### **Código Python**

- Este código extrai a mensagem enviada pelo SNS, faz o parsing para JSON e imprime os dados recebidos.
- Cola esse script la na aba de code do lambda
- clica em: `Deploy`

```python
import json

def lambda_handler(event, context):
    # Extrair a mensagem do evento SNS
    message = event['Records'][0]['Sns']['Message']
    
    # Parsear a mensagem para JSON
    parsed_message = json.loads(message)
    
    # Extrair informações da mensagem
    nome = parsed_message.get('nome')
    aula = parsed_message.get('aula')
    
    # Imprimir as informações
    print(f"Nome: {nome}, Aula: {aula}")
    
    # Retornar a resposta
    return {
        'statusCode': 200,
        'body': json.dumps({'nome': nome, 'aula': aula})
    }
```
### **Enviando email de notificação**
- Criando o Subscription
- Seleciona o arn
- diz que é: `email` 
- Add email
  
![IAM config 1](img/5.enviando_email.png)

### **É possível personalizar msg de acordo pelo meio q for enviado**
![IAM config 1](img/6.personaizar_msg.png)

### **Modelo personalizado em script.py**
![IAM config 1](img/7.modelo_personalizado.png)