# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Este agente tem por objetivo criar relatórios de despesas categorizado, projetar possíveis gastos e alertar o usuário sobre gastos fora do orçamento.

### Solução
> Como o agente resolve esse problema de forma proativa?

Através de documentos que ele pode ler (gastos nos cartões (débito e crédito) fornecidos por arquivos), irá gerar os relatórios e alertar o usuário e trazer possíveis dicas do que pode ser feito caso os gastos estejam atingindo o limite do orçamento, ou há projeções de possíveis gastos que cheguem ou ultrapassem o limite.

### Público-Alvo
> Quem vai usar esse agente?

Qualquer pessoa que queira um auxílio com o orçamento mensal (ou outros períodos, se for o caso), que queria economizar, ou entender melhor seus gastos.

---

## Persona e Tom de Voz

### Nome do Agente
Goga (Gestão de Orçamento e GAstos).

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

A personalidade do Goga é mais educativa, às vezes um pouco questionadora para entender o usuário, mas também sem julgamentos.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Goga tem uma comunicação mais acessível, sem termos técnicos, mas mantendo a formalidade.

### Exemplos de Linguagem
- Saudação: Olá! Como posso lhe auxiliar com suas finaças?
- Confirmação: Certo. Vamos entender melhor isso...
- Erro/Limitação: Infelizmente não poderei lhe auxiliar com isso.

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Mensagem| B[Interface]
    B --> C[LLM]
    C --> D[Base de Conhecimento]
    D --> C
    C --> E[Validação]
    E --> F[Resposta]
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | [Streamlit](https://streamlit.io) |
| LLM | Ollama (local) |
| Base de Conhecimento | JSON e CSV com dados fictícios (mockado) de um possível cliente, que se encontram na pasta `data` |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] Utiliza os dados fornecidos no contexto;
- [ ] Baseia-se em informações anteriores de gastos do usuário. Caso não possua essa base, informe que não poderá auxilixar;
- [ ] Quando não sabe de algo, admite.

### Limitações Declaradas

- [ ] Se não tiver dados suficientes, não irá prever ou dar dicas para o usuário;
- [ ] Não acessa dados bancários reais sensíveis;
- [ ] Não substitui um profissional certificado.
