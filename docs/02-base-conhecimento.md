# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Como o Vithor utiliza? |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Visualizar situações anteriores, para melhorar o atendimento |
| `perfil_investidor.json` | JSON | Conhecer a pessoa do cliente e quais suas metas principais e ganhos |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

- Modifiquei o 'historico_atendimento.csv' para adequa-lo a situação do meu agente e Usuario
- Acabei por excluir o 'produtos_financeiros.json' pois ele não era util ao meu agente, pelo menos nesse momento
- Modifiquei e adicionei informações a 'perfil_investidor.json' para que ele tivesse os dados principais do usuario e suas principais metas
- Adicionei dados ao 'transações.csv' para que ele fique melhor para meu exemplo

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os dados podem ser coloacados diretamente no prompt usando copiar e colar, ou usando o codigo exemplo abaixo:

```python
import pandas as pd
import json

# CSVs
historico = pd.read_csv('data/historico_atendimento.csv')
transacoes = pd.read_csv('data/transacoes.csv')

# JSONs
with open('data/perfil_investidor.json', 'r', encoding='utf-8') as f:
perfil = json.load(f)

```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Eles podem ser colocados no system prompt mas a melhor forma é o acesso dinamico aos dados para consulta do agente

```text
DADOS DO USUARIO(data/perfil_investidor.json):

"nome": "Mariana Senna Cardoso",
"idade": 22,
"profissao": "Analista de RH",
"renda_mensal": 3000.00,
"objetivo_principal": "Aproveitar a juventude sem extrapolar",
"patrimonio_total": 5000.00,
"reserva_emergencia_atual": 1.00,

TRANSAÇÕES DO USUARIO(data/transacoes.csv):

data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,3000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,550.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,60.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,120.00,saida
2025-10-25,Combustível,transporte,250.00,saida
2025-10-26,Conta de Agua,moradia,102.00,saida
2025-10-26,Balada,lazer,200.00,saida


HISTORICO DE ATENDIMENTO(data/historico_atendimento.csv):

data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Usuario perguntou sobre rentabilidade e prazos,sim
2025-09-22,Chat,Cartões,Usuario quis encerrar o cartão de credito,sim
2025-10-01,Chat,Problemas no app,Problema ao visualizar extrato foi corrigido,sim
2025-10-12,Telefone,conta corrente,Usuário pediu auxilio sobre como criar uma reserva,sim

METAS DO USUARIO(data/perfil_investidor.json):

"metas": 
    
"meta": "Completar reserva de emergência",
"valor_necessario": 1680.00,
"prazo": "2026-08"

"meta": "Viajar para a India",
"valor_necessario": 30000.00,
"prazo": "2028-12"

"meta": "Alugar uma casa de praia com amigos",
"valor_necessario": 2000.00,
"prazo": "2026-12"

```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

O exemplo abaixo se baseia no consumo apenas das informações mais importantes para o agente e para a economia e otimização de tokens, mas vale lembrar que é importante o agente entender todo o contexto possivel para que possa auxiliar da melhor forma

```
Dados do Cliente:
- Nome: Mariana Senna Cardoso
- Perfil: Conservador
- Renda Mensal: R$ 3.000

Últimas transações:
- 26/10: Conta de agua - R$ 102
- 26/10: Balada - R$ 200

Resumo de gastos:
- Moradia: R$ 1482
- Alimentação: R$ 610
- Lazer: R$ 255,90
- Transporte: R$ 295
- Saude: R$ 209
- Total de Gastos: R$ 2.851,90

Metas Atuais(da mais proxima a menos proxima):
- Completar reserva de emergência (R$ 1680)
- Viajar para a India (R$ 30000)
...
```
