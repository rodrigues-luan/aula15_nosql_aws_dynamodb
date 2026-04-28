### **1. Inserindo front no AWSAmplify - Conectando ao meu github**
![IAM config 1](img/passo1-conectando-github.png)
---
### **2. Autorizaçao**
![IAM config 1](img/passo2-autorizacao_github.png)
--
### **3. Selecionando Repo**
![IAM config 1](img/passo3-selecionerepositorio.png)
--
### **4. Inserindo Nome do App**
![IAM config 1](img/passo4-inserindonome.png)
--
### **3. Criar as Funções Lambda**
1. No console da AWS, acesse **AWS Lambda** e crie uma nova função chamada `SorteioFunction`.
2. Selecione **Python 3.9** como a linguagem de execução.
3. Substitua o código padrão pelo seguinte:

```python
import json
import random

def lambda_handler(event, context):
    nome_sorteio = event['nomeSorteio']
    num_min = int(event['numMin'])
    num_max = int(event['numMax'])
    numero_sorteado = random.randint(num_min, num_max)
    
    return {
        'statusCode': 200,
        'body': json.dumps({
            'nomeSorteio': nome_sorteio,
            'numeroSorteado': numero_sorteado
        })
    }
```

Aqui está um body em JSON para testar essa função Lambda:

```json
{
    "nomeSorteio": "Sorteio Aula 15",
    "numMin": 1,
    "numMax": 15
}
```

Este body passará o nome do sorteio como "Sorteio Aula 15" e sorteará um número aleatório entre 1 e 15.

4. **Salve** e **implante** a função.

Essa função receberá os números mínimo e máximo e retornará um número aleatório entre eles.

---