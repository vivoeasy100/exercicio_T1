# Resolução da Lista de Exercícios T I
**Disciplina:** Garantia da Qualidade de Software / Gestão e Qualidade de Software  
**Professor:** Daniel Henrique Matos de Paiva  
**Aluno:** Fernando Almeida de Oliveira Braga    
**RA:** 326132695 

---

### Exercício 1: A Mudança de Mentalidade (Código Primeiro vs. Teste Primeiro)

> **a) Por que começar escrevendo o teste obriga o desenvolvedor a pensar como um "cliente" ou "usuário" da classe, em vez de pensar na implementação interna?**

**Resposta:**  
Ao escrever o teste primeiro, a implementação ainda não existe. Por conta disso, o desenvolvedor é obrigado a focar exclusivamente na **interface pública** e no **comportamento esperado** da unidade de código: como a classe será instanciada, quais métodos serão chamados, quais parâmetros serão fornecidos e qual resultado deve ser retornado.  
Isso inverte a perspectiva tradicional: em vez de se preocupar com algoritmos internos, loops, estruturas de dados ou persistência, o desenvolvedor atua como o consumidor daquela API, o que resulta em um design mais intuitivo, de baixo acoplamento e com alta coesão.

---

> **b) Em equipes que usam o fluxo tradicional, é muito comum encontrar a frase: "O código já está pronto, só falta fazer os testes". Explique por que, sob a ótica do TDD, essa frase é uma contradição e qual o risco de escrever testes depois do código de produção estar totalmente pronto.**

**Resposta:**  
* **Por que é uma contradição:** Sob a ótica do TDD (*Test-Driven Development*), o teste não é uma etapa posterior de validação, mas sim o **guia do desenvolvimento**. Se o teste ainda não foi escrito, o código de produção não deveria sequer existir (conforme a 1ª Lei do TDD). Dizer que "o código está pronto sem testes" assume que a funcionalidade foi validada sem critérios formais e automáticos.
* **Riscos de escrever testes depois:**
  1. **Código dificilmente testável:** O código escrito sem testes prévios frequentemente apresenta alto acoplamento, dependências ocultas e falta de modularidade, tornando os testes a posteriori complexos e trabalhosos de implementar.
  2. **Viés de confirmação e testes superficiais:** O desenvolvedor tende a escrever testes apenas para os caminhos felizes que ele sabe que já funcionam no código existente, ignorando casos de borda e falhas críticas.
  3. **Abandono dos testes:** Diante de prazos apertados, a etapa final de "apenas testar" costuma ser negligenciada ou descartada pela equipe.

---

### Exercício 2: O Ritmo do TDD — O Ciclo Red-Green-Refactor

> **a) Explique o que significa e o que deve ser feito pelo desenvolvedor em cada uma das três etapas:**  
> **1. RED**  
> **2. GREEN**  
> **3. REFACTOR**

**Resposta:**  
1. **RED (Vermelho):** O desenvolvedor escreve um teste unitário que define um novo requisito ou comportamento esperado antes de haver código de produção correspondente. O teste é executado e **deve obrigatoriamente falhar** (ou nem compilar). Isso comprova que o teste é válido e que está avaliando algo que ainda não foi implementado.
2. **GREEN (Verde):** O desenvolvedor escreve a quantidade estritamente necessária de código de produção para fazer o teste passar. O objetivo nesta etapa é apenas atingir o status verde o mais rápido possível, sem preocupação imediata com a elegância do código.
3. **REFACTOR (Refatorar):** Com a segurança da suíte de testes passando (em verde), o desenvolvedor melhora o design interno do código (tanto de produção quanto dos testes): remove duplicações, renomeia variáveis e métodos, melhora a legibilidade e otimiza estruturas, garantindo que o comportamento externo permaneça inalterado e os testes continuem verdes.

---

> **b) O que é o princípio da "Solução Mais Simples Possível" na fase GREEN? Por que, nessa fase, é permitido até mesmo retornar um valor fixo (hardcoded) para fazer o teste passar?**

**Resposta:**  
O princípio da "Solução Mais Simples Possível" preceitua que não se deve antecipar complexidades nem implementar regras que ainda não foram requisitadas por nenhum teste falhando (seguindo os princípios *KISS - Keep It Simple, Stupid* e *YAGNI - You Aren't Gonna Need It*).  
Retornar um valor fixo (*hardcoded*) é permitido e encorajado no início porque:
* Comprova que o mecanismo de execução do teste e a interface de comunicação estão funcionando perfeitamente;
* Força o desenvolvedor a escrever o **próximo teste** com entradas e saídas diferentes para justificar a generalização do algoritmo (técnica de triangulação);
* Evita a escrita prematura de código de produção que não possui cobertura de testes.

---

> **c) Se durante a etapa REFACTOR você alterar o código e um dos testes que antes estava verde ficar vermelho, qual deve ser a sua atitude imediata?**

**Resposta:**  
A atitude imediata deve ser **reverter a última alteração** (desfazer a modificação ou usar `git checkout`/`Ctrl+Z`) para retornar ao estado verde estável mais recente.  
A refatoração, por definição, jamais deve alterar o comportamento observável do software. Se o teste ficou vermelho, significa que uma regressão foi inserida. Em vez de tentar "consertar a refatoração" no escuro, volta-se para o código verde e refatora-se novamente em passos menores e mais seguros.

---

### Exercício 3: As Três Leis do TDD

> **a) Suponha que você precisa criar um método somar(a, b). Segundo a Lei 2, se você tentar escrever o teste chamando `calculadora.somar(2, 3)` e a classe `Calculadora` nem sequer existir no projeto, o teste compilou? Isso conta como um teste que falhou (RED)?**

**Resposta:**  
* **O teste compilou?** Não. Ocorrerá um erro de compilação informando que o símbolo/tipo `Calculadora` não existe.
* **Conta como teste que falhou (RED)?** Sim. No TDD, falhas de compilação são formalmente consideradas falhas de teste (RED). De acordo com a 2ª Lei de Uncle Bob, assim que o erro de compilação surge, o desenvolvedor deve parar de escrever o teste e escrever apenas o código de produção mínimo suficiente para fazê-lo compilar (criar a classe `Calculadora` vazia e a assinatura do método).

---

> **b) Qual é a vantagem prática de seguir loops tão curtos de desenvolvimento (que duram segundos ou poucos minutos) em vez de passar horas programando antes de rodar qualquer coisa?**

**Resposta:**  
* **Feedback instantâneo:** O desenvolvedor sabe imediatamente se a última alteração funcionou ou se quebrou algo existente.
* **Isolamento de bugs trivial:** Se um bug ocorre em um ciclo de 1 ou 2 minutos, o problema só pode estar nas últimas 3 ou 5 linhas de código digitadas, eliminando horas de depuração com *debuggers*.
* **Redução da carga cognitiva e estresse:** Trabalha-se com foco em um único problema minúsculo por vez, aumentando o foco, a previsibilidade e a confiança na estabilidade contínua do sistema.

---

### Exercício 4: O Teste como Documentação Viva

> **a) Explique a frase: "No TDD, a suíte de testes unitários funciona como uma documentação executável e sempre atualizada do sistema".**

**Resposta:**  
Diferente de manuais em PDF ou diagramas estáticos que envelhecem e deixam de refletir o comportamento real do software após alterações, os testes unitários são executados continuamente (inclusive em esteiras de CI/CD). Se o código mudar e a especificação expressa nos testes não for atendida, a compilação ou o build falha.  
Portanto, os testes descrevem exatamente **o que o sistema faz**, **como cada componente reage** sob determinadas entradas e cenários de exceção, sendo uma documentação que nunca mente e que se mantém 100% sincronizada com a implementação.

---

> **b) Por que a nomeação dos métodos de teste é crucial para essa documentação? Compare os dois nomes abaixo e diga qual expressa melhor a intenção do requisito de negócio:**  
> * **Opção A:** `@Test void teste1()`  
> * **Opção B:** `@Test void deveBloquearSaqueQuandoSaldoForInsuficiente()`

**Resposta:**  
A nomeação é crucial porque o nome do método de teste serve como a especificação formal do requisito. Em relatórios de execução ou falha, o nome do teste é a primeira informação lida pelo desenvolvedor e pela equipe.
* A **Opção B** (`deveBloquearSaqueQuandoSaldoForInsuficiente`) expressa com clareza cristalina a intenção do negócio, o cenário avaliado e a consequência esperada. Se esse teste falhar, qualquer desenvolvedor saberá exatamente qual regra de negócio foi violada sem precisar analisar o código interno do teste.
* A **Opção A** (`teste1()`) é opaca, genérica e inútil como documentação, exigindo que o leitor inspecione o código linha por linha para descobrir o que está sendo testado.

---

### Exercício 5: Baby Steps (Passos de Bebê)

> **Suponha que seu objetivo final seja criar um Validador de Senhas Fortes que deve checar:**  
> 1. Pelo menos 8 caracteres.  
> 2. Pelo menos um número.  
> 3. Pelo menos um caractere especial.  
>  
> **a) Em vez de tentar validar tudo de uma vez no primeiro teste, ordene e descreva como você dividiria esses requisitos em 3 etapas progressivas de testes (da mais simples para a mais complexa).**

**Resposta:**  
Uma divisão progressiva com foco em *baby steps* do mais simples ao mais complexo seria:

1. **Etapa 1: Validação do Comprimento Mínimo (pelo menos 8 caracteres)**
   * *Teste 1.1:* `deveRejeitarSenhaComMenosDe8Caracteres` (ex.: `"Abc1!"` -> `false`).
   * *Teste 1.2:* `deveAceitarSenhaCom8OuMaisCaracteresApenasComprimento` (ex.: `"abcdefgh"` -> `true`, focando exclusivamente na regra de tamanho nesta fase).
   * *Código de produção:* Checa apenas `senha.length() >= 8`.

2. **Etapa 2: Validação de Presença Numérica (pelo menos um número)**
   * *Teste 2.1:* `deveRejeitarSenhaCom8CaracteresSemNumero` (ex.: `"abcdefgh"` -> `false`).
   * *Teste 2.2:* `deveAceitarSenhaCom8CaracteresComNumero` (ex.: `"abcdefg1"` -> `true`).
   * *Código de produção:* Incrementa a lógica adicionando a verificação de dígitos (ex.: `senha.matches(".*[0-9].*")`).

3. **Etapa 3: Validação de Caractere Especial (pelo menos um caractere especial)**
   * *Teste 3.1:* `deveRejeitarSenhaSemCaractereEspecial` (ex.: `"abcdefg1"` -> `false`).
   * *Teste 3.2:* `deveAprovarSenhaForteCompleta` (ex.: `"Abcdef1@"` -> `true`).
   * *Código de produção:* Completa a validação incorporando a checagem de símbolos/caracteres especiais, fechando a regra completa de negócio.

---

> **b) Qual é a vantagem de avançar em pequenos passos quando você encontra um erro (bug) no código?**

**Resposta:**  
A principal vantagem é a **localização imediata da causa raiz** e a **redução drástica do tempo de depuração**.  
Como cada pequeno passo altera apenas uma regra minúscula e pouquíssimas linhas de código, se um erro surge, a causa do bug só pode estar na alteração recente que acabou de ser feita. Não há necessidade de rastrear fluxos complexos ou utilizar depuradores por horas, tornando o diagnóstico imediato e a correção trivial.
