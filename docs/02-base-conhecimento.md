# Base de Conhecimento

## Dados Utilizados

Dois arquivos são utilizados para alimentar a base de dados do Agente.

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `transacoes.csv` | CSV | Esse arquivo é utilizado para analisar o padrão de gastos do cliente |
| `objetivos.csv` | CSV | Esse arquivo é utilizado para entender os planos e objetivos do cliente |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Foi criado o `arquivo objetivo.csv` com os objetivos e planos financeiros do cliente, afim de que o agente entenda como melhor identificar o que trazer de informações, além disso, o arquivo `transacoes.csv` foi modificado, recebendo novos registros (gerados através do Gemini, baseado nos existentes, com adição de novos aleatórios envolvendo supermercado, restaurantes, transporte, combustível e compras pontuais em lojas online), para os meses anteriores até 2020, "acompanhando" a inflação.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os dados podem ser informados pelo usuário diretamente no Prompt (na conversa com o Agente), mas também, há os dados da pasta data que são carregados anteriomente via código, através do seguinte código em Python:

```python
import pandas as pd

transactions = pd.read_csv('data/transacoes.csv')
objectives = pd.read_csv('data/objetivos.csv')
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Os dados são coletados dos arquivos da pasta `data`, porém, caso necessário, ele pode ser informado no Prompt pelo usuário, copiando e colando os exemplos abaixo:

```text
TRANSACOES DO CLIENTE (data/transacoes.csv):
data,descricao,categoria,valor,tipo

2020-01-01,Salário,receita,3500.00,entrada
2020-01-02,Aluguel,moradia,950.00,saida
2020-01-05,Netflix,lazer,27.90,saida
2020-01-07,Combustível,transporte,85.00,saida
2020-01-10,Restaurante,alimentacao,92.00,saida
2020-01-12,Supermercado,alimentacao,335.00,saida
2020-01-15,Conta de Luz,moradia,129.00,saida
2020-01-18,Uber,transporte,28.00,saida
2020-01-20,Academia,saude,49.00,saida
2020-01-22,Combustível,transporte,79.00,saida

2020-02-01,Salário,receita,3500.00,entrada
2020-02-02,Aluguel,moradia,950.00,saida
2020-02-04,Supermercado,alimentacao,315.00,saida
2020-02-05,Netflix,lazer,27.90,saida
2020-02-11,Restaurante,alimentacao,105.00,saida
2020-02-14,Uber,transporte,18.00,saida
2020-02-15,Conta de Luz,moradia,114.00,saida
2020-02-19,Combustível,transporte,172.00,saida
2020-02-20,Academia,saude,49.00,saida
2020-02-24,Renner,vestuario,119.90,saida

2020-03-01,Salário,receita,3500.00,entrada
2020-03-02,Aluguel,moradia,950.00,saida
2020-03-04,Supermercado,alimentacao,190.00,saida
2020-03-05,Netflix,lazer,27.90,saida
2020-03-07,Farmácia,saude,64.00,saida
2020-03-11,Restaurante,alimentacao,95.00,saida
2020-03-14,Combustível,transporte,165.00,saida
2020-03-15,Conta de Luz,moradia,125.00,saida
2020-03-18,Supermercado,alimentacao,160.00,saida
2020-03-20,Academia,saude,49.00,saida

2020-04-01,Salário,receita,3500.00,entrada
2020-04-02,Aluguel,moradia,950.00,saida
2020-04-05,Netflix,lazer,27.90,saida
2020-04-07,Supermercado,alimentacao,348.00,saida
2020-04-11,Restaurante,alimentacao,75.00,saida
2020-04-13,Uber,transporte,21.00,saida
2020-04-15,Conta de Luz,moradia,122.00,saida
2020-04-20,Academia,saude,49.00,saida
2020-04-23,Combustível,transporte,168.00,saida

2020-05-01,Salário,receita,3500.00,entrada
2020-05-02,Aluguel,moradia,950.00,saida
2020-05-03,Supermercado,alimentacao,160.00,saida
2020-05-05,Netflix,lazer,27.90,saida
2020-05-07,Farmácia,saude,25.00,saida
2020-05-09,Combustível,transporte,160.00,saida
2020-05-11,Restaurante,alimentacao,112.00,saida
2020-05-15,Conta de Luz,moradia,118.00,saida
2020-05-17,Supermercado,alimentacao,172.00,saida
2020-05-20,Academia,saude,49.00,saida
2020-05-24,Amazon,eletronicos,89.90,saida

2020-06-01,Salário,receita,3500.00,entrada
2020-06-02,Aluguel,moradia,950.00,saida
2020-06-04,Combustível,transporte,172.00,saida
2020-06-05,Netflix,lazer,27.90,saida
2020-06-06,Supermercado,alimentacao,340.00,saida
2020-06-10,Restaurante,alimentacao,95.00,saida
2020-06-13,Uber,transporte,14.00,saida
2020-06-15,Conta de Luz,moradia,115.00,saida
2020-06-20,Academia,saude,49.00,saida

2020-07-01,Salário,receita,3500.00,entrada
2020-07-02,Aluguel,moradia,950.00,saida
2020-07-04,Supermercado,alimentacao,355.00,saida
2020-07-05,Netflix,lazer,27.90,saida
2020-07-07,Farmácia,saude,68.00,saida
2020-07-10,Restaurante,alimentacao,90.00,saida
2020-07-13,Combustível,transporte,170.00,saida
2020-07-15,Conta de Luz,moradia,131.00,saida
2020-07-20,Academia,saude,49.00,saida

2020-08-01,Salário,receita,3500.00,entrada
2020-08-02,Aluguel,moradia,950.00,saida
2020-08-03,Supermercado,alimentacao,150.00,saida
2020-08-05,Netflix,lazer,27.90,saida
2020-08-08,Restaurante,alimentacao,105.00,saida
2020-08-11,Combustível,transporte,90.00,saida
2020-08-15,Conta de Luz,moradia,128.00,saida
2020-08-18,Supermercado,alimentacao,175.00,saida
2020-08-20,Academia,saude,49.00,saida
2020-08-24,Combustível,transporte,75.00,saida

2020-09-01,Salário,receita,3500.00,entrada
2020-09-02,Aluguel,moradia,950.00,saida
2020-09-04,Supermercado,alimentacao,325.00,saida
2020-09-05,Netflix,lazer,27.90,saida
2020-09-07,Farmácia,saude,32.00,saida
2020-09-11,Restaurante,alimentacao,85.00,saida
2020-09-15,Conta de Luz,moradia,120.00,saida
2020-09-18,Mercado Livre,casa,54.90,saida
2020-09-20,Academia,saude,49.00,saida
2020-09-26,Combustível,transporte,168.00,saida

2020-10-01,Salário,receita,3500.00,entrada
2020-10-02,Aluguel,moradia,950.00,saida
2020-10-04,Supermercado,alimentacao,340.00,saida
2020-10-05,Netflix,lazer,27.90,saida
2020-10-08,Combustível,transporte,160.00,saida
2020-10-09,Restaurante,alimentacao,98.00,saida
2020-10-12,Uber,transporte,17.00,saida
2020-10-15,Conta de Luz,moradia,124.00,saida
2020-10-20,Academia,saude,49.00,saida

2020-11-01,Salário,receita,3500.00,entrada
2020-11-02,Aluguel,moradia,950.00,saida
2020-11-04,Supermercado,alimentacao,185.00,saida
2020-11-05,Netflix,lazer,27.90,saida
2020-11-07,Combustível,transporte,85.00,saida
2020-11-09,Restaurante,alimentacao,88.00,saida
2020-11-15,Conta de Luz,moradia,119.50,saida
2020-11-18,Supermercado,alimentacao,170.00,saida
2020-11-20,Academia,saude,49.00,saida
2020-11-24,Combustível,transporte,88.00,saida

2020-12-01,Salário,receita,3500.00,entrada
2020-12-02,Aluguel,moradia,950.00,saida
2020-12-03,Supermercado,alimentacao,370.00,saida
2020-12-05,Netflix,lazer,27.90,saida
2020-12-08,Restaurante,alimentacao,125.00,saida
2020-12-11,Uber,transporte,25.00,saida
2020-12-14,Combustível,transporte,165.00,saida
2020-12-15,Conta de Luz,moradia,142.00,saida
2020-12-20,Academia,saude,49.00,saida
2020-12-22,Farmácia,saude,45.00,saida

2021-01-01,Salário,receita,3800.00,entrada
2021-01-02,Aluguel,moradia,1000.00,saida
2021-01-03,Supermercado,alimentacao,370.00,saida
2021-01-05,Netflix,lazer,32.90,saida
2021-01-08,Restaurante,alimentacao,98.00,saida
2021-01-10,Combustível,transporte,182.00,saida
2021-01-12,Uber,transporte,31.00,saida
2021-01-15,Conta de Luz,moradia,138.00,saida
2021-01-20,Academia,saude,59.00,saida

2021-02-01,Salário,receita,3800.00,entrada
2021-02-02,Aluguel,moradia,1000.00,saida
2021-02-05,Netflix,lazer,32.90,saida
2021-02-07,Supermercado,alimentacao,345.00,saida
2021-02-11,Restaurante,alimentacao,118.00,saida
2021-02-14,Uber,transporte,19.50,saida
2021-02-15,Conta de Luz,moradia,121.00,saida
2021-02-20,Academia,saude,59.00,saida
2021-02-22,Combustível,transporte,190.00,saida

2021-03-01,Salário,receita,3800.00,entrada
2021-03-02,Aluguel,moradia,1000.00,saida
2021-03-04,Supermercado,alimentacao,205.00,saida
2021-03-05,Netflix,lazer,32.90,saida
2021-03-07,Farmácia,saude,72.00,saida
2021-03-09,Combustível,transporte,180.00,saida
2021-03-11,Restaurante,alimentacao,102.00,saida
2021-03-15,Conta de Luz,moradia,133.50,saida
2021-03-18,Supermercado,alimentacao,175.00,saida
2021-03-20,Academia,saude,59.00,saida

2021-04-01,Salário,receita,3800.00,entrada
2021-04-02,Aluguel,moradia,1000.00,saida
2021-04-05,Netflix,lazer,32.90,saida
2021-04-07,Supermercado,alimentacao,372.00,saida
2021-04-11,Restaurante,alimentacao,80.00,saida
2021-04-13,Uber,transporte,24.00,saida
2021-04-15,Conta de Luz,moradia,130.00,saida
2021-04-18,Shopee,lazer,39.90,saida
2021-04-20,Academia,saude,59.00,saida
2021-04-25,Combustível,transporte,185.00,saida

2021-05-01,Salário,receita,3800.00,entrada
2021-05-02,Aluguel,moradia,1000.00,saida
2021-05-03,Supermercado,alimentacao,180.00,saida
2021-05-05,Netflix,lazer,32.90,saida
2021-05-07,Farmácia,saude,28.50,saida
2021-05-11,Restaurante,alimentacao,125.00,saida
2021-05-14,Combustível,transporte,175.00,saida
2021-05-15,Conta de Luz,moradia,126.00,saida
2021-05-17,Supermercado,alimentacao,188.00,saida
2021-05-20,Academia,saude,59.00,saida

2021-06-01,Salário,receita,3800.00,entrada
2021-06-02,Aluguel,moradia,1000.00,saida
2021-06-05,Netflix,lazer,32.90,saida
2021-06-06,Supermercado,alimentacao,365.00,saida
2021-06-09,Combustível,transporte,190.00,saida
2021-06-10,Restaurante,alimentacao,108.00,saida
2021-06-13,Uber,transporte,15.50,saida
2021-06-15,Conta de Luz,moradia,124.00,saida
2021-06-20,Academia,saude,59.00,saida

2021-07-01,Salário,receita,3800.00,entrada
2021-07-02,Aluguel,moradia,1000.00,saida
2021-07-04,Supermercado,alimentacao,380.00,saida
2021-07-05,Netflix,lazer,32.90,saida
2021-07-07,Farmácia,saude,79.00,saida
2021-07-10,Restaurante,alimentacao,98.00,saida
2021-07-12,Renner,vestuario,89.90,saida
2021-07-15,Conta de Luz,moradia,142.00,saida
2021-07-18,Combustível,transporte,185.00,saida
2021-07-20,Academia,saude,59.00,saida

2021-08-01,Salário,receita,3800.00,entrada
2021-08-02,Aluguel,moradia,1000.00,saida
2021-08-03,Supermercado,alimentacao,165.00,saida
2021-08-05,Netflix,lazer,32.90,saida
2021-08-07,Combustível,transporte,100.00,saida
2021-08-08,Restaurante,alimentacao,115.00,saida
2021-08-15,Conta de Luz,moradia,139.00,saida
2021-08-18,Supermercado,alimentacao,190.00,saida
2021-08-20,Academia,saude,59.00,saida
2021-08-24,Combustível,transporte,85.00,saida

2021-09-01,Salário,receita,3800.00,entrada
2021-09-02,Aluguel,moradia,1000.00,saida
2021-09-04,Supermercado,alimentacao,345.00,saida
2021-09-05,Netflix,lazer,32.90,saida
2021-09-07,Farmácia,saude,38.00,saida
2021-09-11,Restaurante,alimentacao,92.00,saida
2021-09-14,Combustível,transporte,182.00,saida
2021-09-15,Conta de Luz,moradia,128.50,saida
2021-09-20,Academia,saude,59.00,saida

2021-10-01,Salário,receita,3800.00,entrada
2021-10-02,Aluguel,moradia,1000.00,saida
2021-10-04,Supermercado,alimentacao,360.00,saida
2021-10-05,Netflix,lazer,32.90,saida
2021-10-08,Combustível,transporte,175.00,saida
2021-10-09,Restaurante,alimentacao,110.00,saida
2021-10-12,Uber,transporte,19.00,saida
2021-10-15,Conta de Luz,moradia,135.00,saida
2021-10-20,Academia,saude,59.00,saida

2021-11-01,Salário,receita,3800.00,entrada
2021-11-02,Aluguel,moradia,1000.00,saida
2021-11-05,Netflix,lazer,32.90,saida
2021-11-06,Supermercado,alimentacao,200.00,saida
2021-11-10,Restaurante,alimentacao,95.00,saida
2021-11-12,Combustível,transporte,90.00,saida
2021-11-15,Conta de Luz,moradia,129.00,saida
2021-11-17,Supermercado,alimentacao,185.00,saida
2021-11-19,Amazon,livros,74.50,saida
2021-11-20,Academia,saude,59.00,saida
2021-11-24,Combustível,transporte,95.00,saida

2021-12-01,Salário,receita,3800.00,entrada
2021-12-02,Aluguel,moradia,1000.00,saida
2021-12-04,Supermercado,alimentacao,395.00,saida
2021-12-05,Netflix,lazer,32.90,saida
2021-12-08,Restaurante,alimentacao,140.00,saida
2021-12-10,Combustível,transporte,180.00,saida
2021-12-11,Uber,transporte,28.50,saida
2021-12-15,Conta de Luz,moradia,152.00,saida
2021-12-20,Academia,saude,59.00,saida
2021-12-22,Farmácia,saude,51.00,saida

2022-01-01,Salário,receita,4100.00,entrada
2022-01-02,Aluguel,moradia,1050.00,saida
2022-01-03,Supermercado,alimentacao,400.00,saida
2022-01-05,Netflix,lazer,39.90,saida
2022-01-07,Combustível,transporte,198.00,saida
2022-01-09,Restaurante,alimentacao,105.00,saida
2022-01-13,Uber,transporte,35.00,saida
2022-01-15,Conta de Luz,moradia,146.00,saida
2022-01-20,Academia,saude,69.00,saida

2022-02-01,Salário,receita,4100.00,entrada
2022-02-02,Aluguel,moradia,1050.00,saida
2022-02-05,Netflix,lazer,39.90,saida
2022-02-07,Supermercado,alimentacao,370.00,saida
2022-02-11,Restaurante,alimentacao,125.00,saida
2022-02-14,Uber,transporte,22.00,saida
2022-02-15,Conta de Luz,moradia,128.00,saida
2022-02-18,Combustível,transporte,205.00,saida
2022-02-20,Academia,saude,69.00,saida

2022-03-01,Salário,receita,4100.00,entrada
2022-03-02,Aluguel,moradia,1050.00,saida
2022-03-03,Supermercado,alimentacao,220.00,saida
2022-03-05,Netflix,lazer,39.90,saida
2022-03-08,Farmácia,saude,79.00,saida
2022-03-10,Combustível,transporte,195.00,saida
2022-03-11,Restaurante,alimentacao,110.00,saida
2022-03-15,Conta de Luz,moradia,142.00,saida
2022-03-17,Supermercado,alimentacao,190.00,saida
2022-03-20,Academia,saude,69.00,saida

2022-04-01,Salário,receita,4100.00,entrada
2022-04-02,Aluguel,moradia,1050.00,saida
2022-04-05,Netflix,lazer,39.90,saida
2022-04-08,Supermercado,alimentacao,398.00,saida
2022-04-12,Restaurante,alimentacao,85.00,saida
2022-04-14,Uber,transporte,26.00,saida
2022-04-15,Conta de Luz,moradia,139.00,saida
2022-04-20,Academia,saude,69.00,saida
2022-04-22,Combustível,transporte,200.00,saida

2022-05-01,Salário,receita,4100.00,entrada
2022-05-02,Aluguel,moradia,1050.00,saida
2022-05-04,Supermercado,alimentacao,195.00,saida
2022-05-05,Netflix,lazer,39.90,saida
2022-05-07,Farmácia,saude,31.00,saida
2022-05-11,Restaurante,alimentacao,138.00,saida
2022-05-13,Mercado Livre,casa,62.00,saida
2022-05-15,Conta de Luz,moradia,135.00,saida
2022-05-17,Supermercado,alimentacao,200.00,saida
2022-05-20,Academia,saude,69.00,saida
2022-05-22,Combustível,transporte,190.00,saida

2022-06-01,Salário,receita,4100.00,entrada
2022-06-02,Aluguel,moradia,1050.00,saida
2022-06-05,Netflix,lazer,39.90,saida
2022-06-07,Supermercado,alimentacao,390.00,saida
2022-06-11,Restaurante,alimentacao,115.00,saida
2022-06-14,Uber,transporte,17.00,saida
2022-06-15,Conta de Luz,moradia,131.50,saida
2022-06-18,Combustível,transporte,205.00,saida
2022-06-20,Academia,saude,69.00,saida

2022-07-01,Salário,receita,4100.00,entrada
2022-07-02,Aluguel,moradia,1050.00,saida
2022-07-03,Supermercado,alimentacao,405.00,saida
2022-07-05,Netflix,lazer,39.90,saida
2022-07-07,Farmácia,saude,85.50,saida
2022-07-10,Restaurante,alimentacao,105.00,saida
2022-07-13,Combustível,transporte,200.00,saida
2022-07-15,Conta de Luz,moradia,151.00,saida
2022-07-20,Academia,saude,69.00,saida

2022-08-01,Salário,receita,4100.00,entrada
2022-08-02,Aluguel,moradia,1050.00,saida
2022-08-04,Supermercado,alimentacao,180.00,saida
2022-08-05,Netflix,lazer,39.90,saida
2022-08-06,Combustível,transporte,110.00,saida
2022-08-08,Restaurante,alimentacao,122.00,saida
2022-08-15,Conta de Luz,moradia,148.00,saida
2022-08-19,Supermercado,alimentacao,205.00,saida
2022-08-20,Academia,saude,69.00,saida
2022-08-24,Combustível,transporte,90.00,saida

2022-09-01,Salário,receita,4100.00,entrada
2022-09-02,Aluguel,moradia,1050.00,saida
2022-09-03,Supermercado,alimentacao,365.00,saida
2022-09-05,Netflix,lazer,39.90,saida
2022-09-07,Farmácia,saude,42.00,saida
2022-09-11,Restaurante,alimentacao,98.00,saida
2022-09-15,Conta de Luz,moradia,137.20,saida
2022-09-17,Shopee,lazer,24.90,saida
2022-09-20,Academia,saude,69.00,saida
2022-09-26,Combustível,transporte,198.00,saida

2022-10-01,Salário,receita,4100.00,entrada
2022-10-02,Aluguel,moradia,1050.00,saida
2022-10-05,Netflix,lazer,39.90,saida
2022-10-07,Supermercado,alimentacao,385.00,saida
2022-10-09,Combustível,transporte,190.00,saida
2022-10-11,Restaurante,alimentacao,118.00,saida
2022-10-14,Uber,transporte,21.00,saida
2022-10-15,Conta de Luz,moradia,144.00,saida
2022-10-20,Academia,saude,69.00,saida

2022-11-01,Salário,receita,4100.00,entrada
2022-11-02,Aluguel,moradia,1050.00,saida
2022-11-04,Supermercado,alimentacao,215.00,saida
2022-11-05,Netflix,lazer,39.90,saida
2022-11-07,Combustível,transporte,98.00,saida
2022-11-09,Restaurante,alimentacao,105.00,saida
2022-11-15,Conta de Luz,moradia,138.50,saida
2022-11-18,Supermercado,alimentacao,198.00,saida
2022-11-20,Academia,saude,69.00,saida
2022-11-24,Combustível,transporte,105.00,saida

2022-12-01,Salário,receita,4100.00,entrada
2022-12-02,Aluguel,moradia,1050.00,saida
2022-12-03,Supermercado,alimentacao,425.00,saida
2022-12-05,Netflix,lazer,39.90,saida
2022-12-08,Restaurante,alimentacao,155.00,saida
2022-12-11,Uber,transporte,32.00,saida
2022-12-14,Combustível,transporte,195.00,saida
2022-12-15,Conta de Luz,moradia,164.00,saida
2022-12-20,Academia,saude,69.00,saida
2022-12-22,Farmácia,saude,58.00,saida

2023-01-01,Salário,receita,4400.00,entrada
2023-01-02,Aluguel,moradia,1100.00,saida
2023-01-04,Supermercado,alimentacao,430.00,saida
2023-01-05,Netflix,lazer,44.90,saida
2023-01-08,Restaurante,alimentacao,112.00,saida
2023-01-10,Combustível,transporte,210.00,saida
2023-01-12,Uber,transporte,38.00,saida
2023-01-15,Conta de Luz,moradia,154.00,saida
2023-01-20,Academia,saude,79.00,saida

2023-02-01,Salário,receita,4400.00,entrada
2023-02-02,Aluguel,moradia,1100.00,saida
2023-02-05,Netflix,lazer,44.90,saida
2023-02-06,Supermercado,alimentacao,395.00,saida
2023-02-10,Restaurante,alimentacao,132.00,saida
2023-02-13,Uber,transporte,23.50,saida
2023-02-15,Conta de Luz,moradia,133.00,saida
2023-02-18,Combustível,transporte,220.00,saida
2023-02-20,Academia,saude,79.00,saida

2023-03-01,Salário,receita,4400.00,entrada
2023-03-02,Aluguel,moradia,1100.00,saida
2023-03-04,Supermercado,alimentacao,240.00,saida
2023-03-05,Netflix,lazer,44.90,saida
2023-03-08,Renner,vestuario,159.90,saida
2023-03-09,Farmácia,saude,85.00,saida
2023-03-11,Combustível,transporte,205.00,saida
2023-03-12,Restaurante,alimentacao,118.00,saida
2023-03-15,Conta de Luz,moradia,149.80,saida
2023-03-18,Supermercado,alimentacao,205.00,saida
2023-03-20,Academia,saude,79.00,saida

2023-04-01,Salário,receita,4400.00,entrada
2023-04-02,Aluguel,moradia,1100.00,saida
2023-04-05,Netflix,lazer,44.90,saida
2023-04-07,Supermercado,alimentacao,425.00,saida
2023-04-11,Restaurante,alimentacao,90.00,saida
2023-04-14,Uber,transporte,29.00,saida
2023-04-15,Conta de Luz,moradia,145.00,saida
2023-04-20,Academia,saude,79.00,saida
2023-04-26,Combustível,transporte,212.00,saida

2023-05-01,Salário,receita,4400.00,entrada
2023-05-02,Aluguel,moradia,1100.00,saida
2023-05-03,Supermercado,alimentacao,210.00,saida
2023-05-05,Netflix,lazer,44.90,saida
2023-05-08,Farmácia,saude,35.00,saida
2023-05-10,Combustível,transporte,200.00,saida
2023-05-12,Restaurante,alimentacao,145.00,saida
2023-05-15,Conta de Luz,moradia,142.30,saida
2023-05-19,Supermercado,alimentacao,215.00,saida
2023-05-20,Academia,saude,79.00,saida

2023-06-01,Salário,receita,4400.00,entrada
2023-06-02,Aluguel,moradia,1100.00,saida
2023-06-05,Netflix,lazer,44.90,saida
2023-06-06,Supermercado,alimentacao,415.00,saida
2023-06-08,Amazon,casa,129.90,saida
2023-06-10,Restaurante,alimentacao,125.00,saida
2023-06-13,Uber,transporte,18.50,saida
2023-06-15,Conta de Luz,moradia,138.00,saida
2023-06-18,Combustível,transporte,220.00,saida
2023-06-20,Academia,saude,79.00,saida

2023-07-01,Salário,receita,4400.00,entrada
2023-07-02,Aluguel,moradia,1100.00,saida
2023-07-04,Supermercado,alimentacao,430.00,saida
2023-07-05,Netflix,lazer,44.90,saida
2023-07-08,Farmácia,saude,92.00,saida
2023-07-11,Restaurante,alimentacao,112.00,saida
2023-07-14,Combustível,transporte,215.00,saida
2023-07-15,Conta de Luz,moradia,159.00,saida
2023-07-20,Academia,saude,79.00,saida

2023-08-01,Salário,receita,4400.00,entrada
2023-08-02,Aluguel,moradia,1100.00,saida
2023-08-03,Supermercado,alimentacao,195.00,saida
2023-08-05,Netflix,lazer,44.90,saida
2023-08-07,Combustível,transporte,120.00,saida
2023-08-09,Restaurante,alimentacao,130.00,saida
2023-08-15,Conta de Luz,moradia,155.00,saida
2023-08-18,Supermercado,alimentacao,225.00,saida
2023-08-20,Academia,saude,79.00,saida
2023-08-25,Combustível,transporte,95.00,saida

2023-09-01,Salário,receita,4400.00,entrada
2023-09-02,Aluguel,moradia,1100.00,saida
2023-09-04,Supermercado,alimentacao,390.00,saida
2023-09-05,Netflix,lazer,44.90,saida
2023-09-08,Farmácia,saude,47.50,saida
2023-09-12,Restaurante,alimentacao,105.00,saida
2023-09-15,Conta de Luz,moradia,144.00,saida
2023-09-20,Academia,saude,79.00,saida
2023-09-23,Combustível,transporte,210.00,saida

2023-10-01,Salário,receita,4400.00,entrada
2023-10-02,Aluguel,moradia,1100.00,saida
2023-10-04,Mercado Livre,casa,84.00,saida
2023-10-05,Netflix,lazer,44.90,saida
2023-10-06,Supermercado,alimentacao,410.00,saida
2023-10-10,Restaurante,alimentacao,125.00,saida
2023-10-13,Uber,transporte,22.00,saida
2023-10-15,Conta de Luz,moradia,151.20,saida
2023-10-18,Combustível,transporte,205.00,saida
2023-10-20,Academia,saude,79.00,saida

2023-11-01,Salário,receita,4400.00,entrada
2023-11-02,Aluguel,moradia,1100.00,saida
2023-11-03,Supermercado,alimentacao,230.00,saida
2023-11-05,Netflix,lazer,44.90,saida
2023-11-06,Combustível,transporte,105.00,saida
2023-11-08,Restaurante,alimentacao,110.00,saida
2023-11-15,Conta de Luz,moradia,146.00,saida
2023-11-18,Supermercado,alimentacao,215.00,saida
2023-11-20,Academia,saude,79.00,saida
2023-11-24,Combustível,transporte,110.00,saida

2023-12-01,Salário,receita,4400.00,entrada
2023-12-02,Aluguel,moradia,1100.00,saida
2023-12-04,Supermercado,alimentacao,460.00,saida
2023-12-05,Netflix,lazer,44.90,saida
2023-12-07,Combustível,transporte,210.00,saida
2023-12-09,Restaurante,alimentacao,170.00,saida
2023-12-12,Uber,transporte,36.00,saida
2023-12-15,Conta de Luz,moradia,172.50,saida
2023-12-20,Academia,saude,79.00,saida
2023-12-23,Farmácia,saude,64.00,saida

2024-01-01,Salário,receita,4700.00,entrada
2024-01-02,Aluguel,moradia,1150.00,saida
2024-01-03,Supermercado,alimentacao,455.00,saida
2024-01-05,Netflix,lazer,51.90,saida
2024-01-09,Restaurante,alimentacao,118.00,saida
2024-01-12,Combustível,transporte,222.00,saida
2024-01-13,Uber,transporte,41.50,saida
2024-01-15,Conta de Luz,moradia,160.00,saida
2024-01-20,Academia,saude,89.00,saida

2024-02-01,Salário,receita,4700.00,entrada
2024-02-02,Aluguel,moradia,1150.00,saida
2024-02-05,Netflix,lazer,51.90,saida
2024-02-07,Supermercado,alimentacao,410.00,saida
2024-02-11,Restaurante,alimentacao,140.00,saida
2024-02-14,Uber,transporte,25.00,saida
2024-02-15,Conta de Luz,moradia,139.00,saida
2024-02-19,Combustível,transporte,230.00,saida
2024-02-20,Academia,saude,89.00,saida

2024-03-01,Salário,receita,4700.00,entrada
2024-03-02,Aluguel,moradia,1150.00,saida
2024-03-04,Supermercado,alimentacao,225.00,saida
2024-03-05,Netflix,lazer,51.90,saida
2024-03-07,Combustível,transporte,218.00,saida
2024-03-08,Farmácia,saude,114.50,saida
2024-03-12,Restaurante,alimentacao,125.00,saida
2024-03-15,Conta de Luz,moradia,156.30,saida
2024-03-19,Supermercado,alimentacao,230.00,saida
2024-03-20,Academia,saude,89.00,saida

2024-04-01,Salário,receita,4700.00,entrada
2024-04-02,Aluguel,moradia,1150.00,saida
2024-04-05,Netflix,lazer,51.90,saida
2024-04-06,Supermercado,alimentacao,445.00,saida
2024-04-10,Restaurante,alimentacao,95.00,saida
2024-04-13,Uber,transporte,32.00,saida
2024-04-15,Conta de Luz,moradia,151.00,saida
2024-04-18,Shopee,lazer,45.00,saida
2024-04-20,Academia,saude,89.00,saida
2024-04-26,Combustível,transporte,225.00,saida

2024-05-01,Salário,receita,4700.00,entrada
2024-05-02,Aluguel,moradia,1150.00,saida
2024-05-03,Supermercado,alimentacao,250.00,saida
2024-05-05,Netflix,lazer,51.90,saida
2024-05-08,Farmácia,saude,41.00,saida
2024-05-10,Combustível,transporte,210.00,saida
2024-05-12,Restaurante,alimentacao,150.00,saida
2024-05-15,Conta de Luz,moradia,148.90,saida
2024-05-18,Supermercado,alimentacao,210.00,saida
2024-05-20,Academia,saude,89.00,saida

2024-06-01,Salário,receita,4700.00,entrada
2024-06-02,Aluguel,moradia,1150.00,saida
2024-06-05,Netflix,lazer,51.90,saida
2024-06-07,Supermercado,alimentacao,460.00,saida
2024-06-10,Combustível,transporte,240.00,saida
2024-06-11,Restaurante,alimentacao,135.00,saida
2024-06-14,Uber,transporte,19.00,saida
2024-06-15,Conta de Luz,moradia,142.00,saida
2024-06-20,Academia,saude,89.00,saida

2024-07-01,Salário,receita,4700.00,entrada
2024-07-02,Aluguel,moradia,1150.00,saida
2024-07-04,Supermercado,alimentacao,415.00,saida
2024-07-05,Netflix,lazer,51.90,saida
2024-07-08,Farmácia,saude,88.20,saida
2024-07-11,Combustível,transporte,225.00,saida
2024-07-12,Restaurante,alimentacao,115.00,saida
2024-07-15,Conta de Luz,moradia,166.40,saida
2024-07-20,Academia,saude,89.00,saida

2024-08-01,Salário,receita,4700.00,entrada
2024-08-02,Aluguel,moradia,1150.00,saida
2024-08-03,Supermercado,alimentacao,240.00,saida
2024-08-05,Netflix,lazer,51.90,saida
2024-08-06,Combustível,transporte,130.00,saida
2024-08-09,Restaurante,alimentacao,145.00,saida
2024-08-12,Renner,vestuario,129.90,saida
2024-08-15,Conta de Luz,moradia,162.00,saida
2024-08-19,Supermercado,alimentacao,220.00,saida
2024-08-20,Academia,saude,89.00,saida
2024-08-26,Combustível,transporte,110.00,saida

2024-09-01,Salário,receita,4700.00,entrada
2024-09-02,Aluguel,moradia,1150.00,saida
2024-09-05,Netflix,lazer,51.90,saida
2024-09-06,Supermercado,alimentacao,385.00,saida
2024-09-10,Restaurante,alimentacao,110.00,saida
2024-09-13,Uber,transporte,38.50,saida
2024-09-15,Conta de Luz,moradia,149.50,saida
2024-09-18,Combustível,transporte,230.00,saida
2024-09-20,Academia,saude,89.00,saida

2024-10-01,Salário,receita,4700.00,entrada
2024-10-02,Aluguel,moradia,1150.00,saida
2024-10-04,Supermercado,alimentacao,430.00,saida
2024-10-05,Netflix,lazer,51.90,saida
2024-10-08,Farmácia,saude,52.00,saida
2024-10-10,Combustível,transporte,215.00,saida
2024-10-11,Restaurante,alimentacao,130.00,saida
2024-10-14,Uber,transporte,26.00,saida
2024-10-15,Conta de Luz,moradia,158.00,saida
2024-10-20,Academia,saude,89.00,saida

2024-11-01,Salário,receita,4700.00,entrada
2024-11-02,Aluguel,moradia,1150.00,saida
2024-11-03,Supermercado,alimentacao,210.00,saida
2024-11-05,Netflix,lazer,51.90,saida
2024-11-07,Combustível,transporte,115.00,saida
2024-11-08,Restaurante,alimentacao,120.00,saida
2024-11-15,Conta de Luz,moradia,152.40,saida
2024-11-19,Supermercado,alimentacao,265.30,saida
2024-11-20,Academia,saude,89.00,saida
2024-11-25,Combustível,transporte,120.00,saida

2024-12-01,Salário,receita,4700.00,entrada
2024-12-02,Aluguel,moradia,1150.00,saida
2024-12-04,Supermercado,alimentacao,495.00,saida
2024-12-05,Netflix,lazer,51.90,saida
2024-12-06,Combustível,transporte,220.00,saida
2024-12-09,Restaurante,alimentacao,185.00,saida
2024-12-13,Uber,transporte,44.00,saida
2024-12-15,Conta de Luz,moradia,180.20,saida
2024-12-20,Academia,saude,89.00,saida
2024-12-22,Farmácia,saude,95.40,saida

2025-01-01,Salário,receita,5000.00,entrada
2025-01-02,Aluguel,moradia,1200.00,saida
2025-01-03,Supermercado,alimentacao,440.00,saida
2025-01-05,Netflix,lazer,55.90,saida
2025-01-08,Farmácia,saude,76.00,saida
2025-01-11,Combustível,transporte,235.00,saida
2025-01-12,Restaurante,alimentacao,120.00,saida
2025-01-15,Conta de Luz,moradia,163.80,saida
2025-01-20,Academia,saude,99.00,saida
2025-01-22,Uber,transporte,31.00,saida

2025-02-01,Salário,receita,5000.00,entrada
2025-02-02,Aluguel,moradia,1200.00,saida
2025-02-04,Supermercado,alimentacao,250.00,saida
2025-02-05,Netflix,lazer,55.90,saida
2025-02-07,Combustível,transporte,130.00,saida
2025-02-09,Restaurante,alimentacao,135.00,saida
2025-02-15,Conta de Luz,moradia,148.00,saida
2025-02-18,Supermercado,alimentacao,225.50,saida
2025-02-20,Academia,saude,99.00,saida
2025-02-25,Combustível,transporte,120.00,saida

2025-03-01,Salário,receita,5000.00,entrada
2025-03-02,Aluguel,moradia,1200.00,saida
2025-03-05,Netflix,lazer,55.90,saida
2025-03-06,Supermercado,alimentacao,470.00,saida
2025-03-10,Restaurante,alimentacao,105.00,saida
2025-03-12,Combustível,transporte,245.00,saida
2025-03-14,Uber,transporte,52.00,saida
2025-03-15,Conta de Luz,moradia,171.20,saida
2025-03-20,Academia,saude,99.00,saida

2025-04-01,Salário,receita,5000.00,entrada
2025-04-02,Aluguel,moradia,1200.00,saida
2025-04-04,Supermercado,alimentacao,310.00,saida
2025-04-05,Netflix,lazer,55.90,saida
2025-04-08,Farmácia,saude,38.50,saida
2025-04-10,Amazon,beleza,112.40,saida
2025-04-12,Restaurante,alimentacao,160.00,saida
2025-04-15,Conta de Luz,moradia,165.00,saida
2025-04-18,Supermercado,alimentacao,190.00,saida
2025-04-20,Academia,saude,99.00,saida
2025-04-23,Combustível,transporte,220.00,saida

2025-05-01,Salário,receita,5000.00,entrada
2025-05-02,Aluguel,moradia,1200.00,saida
2025-05-05,Netflix,lazer,55.90,saida
2025-05-07,Supermercado,alimentacao,435.00,saida
2025-05-09,Combustível,transporte,260.00,saida
2025-05-11,Restaurante,alimentacao,125.00,saida
2025-05-14,Uber,transporte,21.00,saida
2025-05-15,Conta de Luz,moradia,159.40,saida
2025-05-18,Uber,transporte,15.50,saida
2025-05-20,Academia,saude,99.00,saida

2025-06-01,Salário,receita,5000.00,entrada
2025-06-02,Aluguel,moradia,1200.00,saida
2025-06-03,Supermercado,alimentacao,210.40,saida
2025-06-05,Netflix,lazer,55.90,saida
2025-06-06,Combustível,transporte,235.00,saida
2025-06-08,Farmácia,saude,112.00,saida
2025-06-12,Restaurante,alimentacao,140.00,saida
2025-06-15,Conta de Luz,moradia,152.00,saida
2025-06-17,Supermercado,alimentacao,255.00,saida
2025-06-20,Academia,saude,99.00,saida
2025-06-22,Uber,transporte,42.00,saida

2025-07-01,Salário,receita,5000.00,entrada
2025-07-02,Aluguel,moradia,1200.00,saida
2025-07-05,Netflix,lazer,55.90,saida
2025-07-06,Supermercado,alimentacao,480.00,saida
2025-07-10,Restaurante,alimentacao,98.00,saida
2025-07-13,Uber,transporte,34.00,saida
2025-07-15,Conta de Luz,moradia,174.10,saida
2025-07-18,Combustível,transporte,255.00,saida
2025-07-20,Academia,saude,99.00,saida

2025-08-01,Salário,receita,5000.00,entrada
2025-08-02,Aluguel,moradia,1200.00,saida
2025-08-04,Supermercado,alimentacao,290.00,saida
2025-08-05,Netflix,lazer,55.90,saida
2025-08-07,Farmácia,saude,45.20,saida
2025-08-10,Mercado Livre,eletronicos,149.00,saida
2025-08-11,Restaurante,alimentacao,155.00,saida
2025-08-15,Conta de Luz,moradia,185.30,saida
2025-08-18,Combustível,transporte,130.00,saida
2025-08-20,Academia,saude,99.00,saida
2025-08-22,Supermercado,alimentacao,215.00,saida
2025-08-26,Combustível,transporte,125.00,saida

2025-09-01,Salário,receita,5000.00,entrada
2025-09-02,Aluguel,moradia,1200.00,saida
2025-09-03,Supermercado,alimentacao,410.50,saida
2025-09-05,Netflix,lazer,55.90,saida
2025-09-09,Uber,transporte,28.00,saida
2025-09-12,Restaurante,alimentacao,115.00,saida
2025-09-15,Conta de Luz,moradia,162.00,saida
2025-09-19,Combustível,transporte,240.00,saida
2025-09-20,Academia,saude,99.00,saida
2025-09-24,Uber,transporte,19.50,saida

2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-06,Combustível,transporte,250.00,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida

2025-11-01,Salário,receita,5000.00,entrada
2025-11-02,Aluguel,moradia,1200.00,saida
2025-11-03,Supermercado,alimentacao,230.50,saida
2025-11-05,Netflix,lazer,55.90,saida
2025-11-06,Uber,transporte,22.50,saida
2025-11-10,Restaurante,alimentacao,145.00,saida
2025-11-12,Combustível,transporte,150.00,saida
2025-11-15,Conta de Luz,moradia,168.20,saida
2025-11-18,Supermercado,alimentacao,285.00,saida
2025-11-20,Academia,saude,99.00,saida
2025-11-22,Uber,transporte,41.20,saida
2025-11-27,Combustível,transporte,120.00,saida

2025-12-01,Salário,receita,5000.00,entrada
2025-12-02,Aluguel,moradia,1200.00,saida
2025-12-04,Supermercado,alimentacao,510.00,saida
2025-12-05,Netflix,lazer,55.90,saida
2025-12-08,Farmácia,saude,134.50,saida
2025-12-11,Restaurante,alimentacao,210.00,saida
2025-12-15,Conta de Luz,moradia,195.40,saida
2025-12-19,Combustível,transporte,260.00,saida
2025-12-20,Academia,saude,99.00,saida
2025-12-23,Supermercado,alimentacao,180.20,saida

2026-01-01,Salário,receita,5300.00,entrada
2026-01-02,Aluguel,moradia,1250.00,saida
2026-01-04,Supermercado,alimentacao,340.00,saida
2026-01-05,Netflix,lazer,59.90,saida
2026-01-09,Uber,transporte,18.00,saida
2026-01-11,Combustível,transporte,230.00,saida
2026-01-12,Restaurante,alimentacao,85.00,saida
2026-01-14,Uber,transporte,32.50,saida
2026-01-15,Conta de Luz,moradia,172.00,saida
2026-01-20,Academia,saude,109.00,saida
2026-01-22,Supermercado,alimentacao,195.30,saida
2026-01-25,Uber,transporte,27.00,saida

2026-02-01,Salário,receita,5300.00,entrada
2026-02-02,Aluguel,moradia,1250.00,saida
2026-02-04,Combustível,transporte,140.00,saida
2026-02-05,Netflix,lazer,59.90,saida
2026-02-06,Supermercado,alimentacao,420.00,saida
2026-02-10,Restaurante,alimentacao,160.00,saida
2026-02-15,Conta de Luz,moradia,155.00,saida
2026-02-20,Academia,saude,109.00,saida
2026-02-23,Combustível,transporte,135.00,saida

2026-03-01,Salário,receita,5300.00,entrada
2026-03-02,Aluguel,moradia,1250.00,saida
2026-03-03,Supermercado,alimentacao,210.00,saida
2026-03-05,Netflix,lazer,59.90,saida
2026-03-06,Renner,vestuario,149.90,saida
2026-03-07,Farmácia,saude,42.80,saida
2026-03-11,Restaurante,alimentacao,125.00,saida
2026-03-14,Supermercado,alimentacao,315.00,saida
2026-03-15,Conta de Luz,moradia,161.00,saida
2026-03-18,Uber,transporte,55.00,saida
2026-03-20,Academia,saude,109.00,saida
2026-03-22,Farmácia,saude,68.90,saida
2026-03-26,Combustível,transporte,245.00,saida

2026-04-01,Salário,receita,5300.00,entrada
2026-04-02,Aluguel,moradia,1250.00,saida
2026-04-04,Supermercado,alimentacao,490.00,saida
2026-04-05,Netflix,lazer,59.90,saida
2026-04-09,Restaurante,alimentacao,150.00,saida
2026-04-12,Uber,transporte,24.00,saida
2026-04-15,Conta de Luz,moradia,168.50,saida
2026-04-19,Combustível,transporte,270.00,saida
2026-04-20,Academia,saude,109.00,saida
2026-04-25,Supermercado,alimentacao,115.00,saida

2026-05-01,Salário,receita,5300.00,entrada
2026-05-02,Aluguel,moradia,1250.00,saida
2026-05-03,Uber,transporte,31.00,saida
2026-05-04,Combustível,transporte,160.00,saida
2026-05-05,Netflix,lazer,59.90,saida
2026-05-06,Supermercado,alimentacao,260.00,saida
2026-05-10,Restaurante,alimentacao,130.00,saida
2026-05-15,Conta de Luz,moradia,174.00,saida
2026-05-18,Farmácia,saude,89.50,saida
2026-05-20,Academia,saude,109.00,saida
2026-05-22,Supermercado,alimentacao,245.00,saida
2026-05-27,Combustível,transporte,110.00,saida
```
```text
OBJETIVOS DO CLIENTE (objetivos.csv)
ano,objetivo,descricao,valor

2034,Apartamento,adquirir ou dar entrada em um apartamento,450000
2030,Carro,comprar ou financiar um carro utilizando o atual como entrada,65000
2028,Investimento,investimento em acoes,50000
```
---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

O exemplo a seguir é um exemplo de contexto montado conforme os dados originais da base de conhecimento, mas, sintetizando apenas as informações mais relevantes.
```
Objetivos do Cliente:
- Apartamento: R$ 450.000,00
- Trocar de carro: R$ 65.000,00
- Investimento e fundo de reserva: R$ 50.000,00

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
