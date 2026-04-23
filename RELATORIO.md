# 📦 Relatório Final

> **Atividade:** Bug Report Profissional + Code Review Guiado
> **Curso:** Qualidade de Software
> **Professor:** Prof. Claudio Nunes

---

## 👥 Identificação da dupla

| Nome completo | RA | GitHub |
|---|---|---|
| [Alexsander Jean de Melo Kotona] | [230162] | [https://github.com/alexsanderkotona] |
| [Kevin Jean de Melo Kotona] | [183685] | [https://github.com/KevinKotona] |

**Ambiente de testes:** [Descreva brevemente o setup — ex: Chrome 121 no Windows 11, GitHub Pages do fork, editor web do GitHub]

---

## 📋 Sumário

- [Parte A — Bug Reports](#parte-a--bug-reports)
- [Parte B — Code Review](#parte-b--code-review)
- [Reflexão final](#-reflexão-final)
- [Declarações](#-declarações)

---

## Parte A — Bug Reports

## BUG-001

Título: [Validação] Cadastro de tarefas permite campos obrigatórios vazios

Severidade: Média
Justificativa da severidade: Permite a inserção de dados inconsistentes no banco/interface, gerando uma lista de tarefas visualmente poluída e sem utilidade funcional.

Prioridade: P2
Justificativa da prioridade: Correção necessária para garantir a integridade mínima dos dados inseridos pelo usuário antes da persistência.

Ambiente:

Navegador: Chrome 124.0

Sistema Operacional: Windows 11

Versão da aplicação: TarefaQS v1.0.0

Passos para reprodução:

Acessar a aplicação TarefaQS através do navegador.

No formulário de cadastro, não preencher o campo "Título da Tarefa".

Deixar o seletor de "Data de Entrega" sem nenhuma data selecionada.

Clicar no botão "Adicionar Tarefa".

Resultado esperado:
O sistema deve validar os campos, impedir a criação da tarefa e exibir uma mensagem de erro indicando que os campos são obrigatórios.

Resultado obtido:
A aplicação adiciona uma nova linha à tabela com título e data vazios.

Evidência:

Sugestão de causa raiz (opcional):
Ausência de validação lógica no JavaScript (if(!valor)) ou falta do atributo required nos elementos de input HTML.

---

## BUG-002

Título: [Persistência] Perda total de dados cadastrados após atualização da página (F5)

Severidade: Crítica
Justificativa da severidade: Caracteriza perda de dados (Data Loss). O sistema falha em sua função primária de gerenciar e manter informações para o usuário.

Prioridade: P1
Justificativa da prioridade: Urgência máxima, pois o software perde sua utilidade prática se o usuário não puder fechar a aba ou atualizar a página sem perder o progresso.

Ambiente:

Navegador: Chrome 124.0

Sistema Operacional: Windows 11

Versão da aplicação: TarefaQS v1.0.0

Passos para reprodução:

Cadastrar três tarefas preenchendo todos os campos corretamente.

Observar as tarefas sendo listadas na tabela principal.

Pressionar a tecla F5 para atualizar a página.

Resultado esperado:
As tarefas cadastradas devem ser recuperadas e exibidas novamente na tela (através de LocalStorage ou Banco de Dados).

Resultado obtido:
A aplicação reinicia com a lista de tarefas totalmente vazia.

Evidência:

Sugestão de causa raiz (opcional):
O script de gerenciamento não está utilizando métodos de persistência (como localStorage.setItem) ou não executa a leitura dos dados salvos no evento de carregamento.

---

## BUG-003
Título: [UX/Regra de Negócio] Aceitação de prazos de entrega em datas retroativas

Severidade: Baixa
Justificativa da severidade: Trata-se de uma falha de regra de negócio que permite dados incoerentes (prazos no passado), mas não impede o funcionamento técnico do sistema.

Prioridade: P3
Justificativa da prioridade: Melhoria de experiência do usuário e consistência do planejamento acadêmico.

Ambiente:

Navegador: Chrome 124.0

Sistema Operacional: Windows 11

Versão da aplicação: TarefaQS v1.0.0

Passos para reprodução:

No campo de "Data de Entrega", abrir o seletor de data.

Navegar para um ano anterior (ex: 2022 ou 2023) e selecionar um dia.

Inserir um título para a tarefa e clicar em "Adicionar Tarefa".

Resultado esperado:
O sistema deve validar se a data é igual ou posterior à data atual (hoje), emitindo um aviso caso o prazo selecionado já tenha expirado.

Resultado obtido:
A tarefa é salva e listada com sucesso, apresentando uma data de entrega que já passou.

Evidência:

Sugestão de causa raiz (opcional):
Falta de comparação lógica entre o valor inserido no input e a data do sistema (new Date()).

---

## BUG-004

Título: [Estatística] Inconsistência no contador de tarefas pendentes

Severidade: Média
Justificativa da severidade: Apresenta informações incorretas ao usuário sobre o volume de trabalho, gerando desconfiança na integridade do sistema.

Prioridade: P2
Justificativa da prioridade: Dados de resumo/dashboard precisam ser precisos para que o usuário confie na ferramenta para gestão acadêmica.

Ambiente:

Navegador: Chrome 124.0

Sistema Operacional: Windows 11

Versão da aplicação: TarefaQS v1.0.0

Passos para reprodução:

Adicionar 3 tarefas quaisquer à lista.

Marcar 1 tarefa como "Concluída".

Observar o contador de "Tarefas Pendentes" no topo ou rodapé da página.

Excluir uma das tarefas pendentes e observar se o contador atualiza proporcionalmente.

Resultado esperado:
O contador de tarefas pendentes deve refletir exatamente a quantidade de itens na lista que não estão marcados como concluídos.

Resultado obtido:
O contador exibe um número superior à quantidade de tarefas visíveis (ex: indica 3 pendentes quando há apenas 1 na tela).

Evidência:

Sugestão de causa raiz (opcional):
A função de contagem pode estar somando tarefas que foram excluídas do DOM, mas não removidas do array de dados na memória, ou a lógica de filtro está ignorando a atualização do contador.

---


---

## Parte B — Code Review

> **Substitua este bloco de citação pelo conteúdo copiado integralmente do seu arquivo `parte-b-code-review/formulario-code-review.md`.**
> Preservando os rótulos, linhas e sugestões de correção.
> Mínimo: 6 findings.

### Resumo

| # | Linha | Dimensão      | Rótulo                      | Severidade                  |
|---|-------|-------------- |---------------------------- |-----------------------------|
1	|  12	|  Segurança	|[SECURITY] SQL Injection	          Crítica
2	|  46-95| Complexidade  |[REFACTOR] Alta Complexidade	      Alta
3	| 98-138|  Padrões	    |[DRY] Código Duplicado	              Média
4	|  18   |   Erros	    |[ERROR] Falta de Try/Catch	          Alta
5	|	5   |   Padrões	    |[LOGIC] Constante Não Usada	      Baixa
6	|	39  |  Legibilidade	|[STYLE] Nomenclatura Genérica	      Baixa

### Findings detalhadas

### Finding #1

📍 Linha(s): 12
🏷 Rótulo: blocker
📂 Dimensão: Segurança
⚠️ Severidade: Crítica

🐛 Problema: Concatenação direta de variável na query SQL ("SELECT * FROM usuarios WHERE nome = '" + nome + "'"), possibilitando ataques graves de SQL Injection.

💡 Sugestão de correção:

JavaScript

async function buscarUsuarioPorNome(nome) {
  const query = "SELECT * FROM usuarios WHERE nome = ?";
  return db.executarQuery(query, [nome]);
}
📚 Referência (opcional): OWASP Top 10 - Injection

### Finding #2

📍 Linha(s): 46 a 95
🏷 Rótulo: major
📂 Dimensão: Complexidade
⚠️ Severidade: Alta

🐛 Problema: Alta complexidade ciclomática na função calcularLimiteEmprestimo, com muitos aninhamentos (if/else dentro de if/else), dificultando a leitura, manutenção e testes.

💡 Sugestão de correção:

JavaScript

function calcularLimiteEmprestimo(usuario) {
  if (usuario.bloqueadoAte && new Date(usuario.bloqueadoAte) > new Date()) {
    return 0;
  }
  
  if (usuario.tipo === 'professor') {
    return calcularLimiteProfessor(usuario); // Extrair para função menor
  }
}


---

### Finding #3

📍 Linha(s): 98 a 138
🏷 Rótulo: major
📂 Dimensão: Padrões
⚠️ Severidade: Média

🐛 Problema: Código duplicado. A função calcularLimiteComSuspensao repete praticamente toda a lógica já existente na função calcularLimiteEmprestimo, violando o princípio DRY.

💡 Sugestão de correção:

JavaScript

function calcularLimiteComSuspensao(usuario) {
  if (usuario.suspenso) {
    return 0;
  }
  return calcularLimiteEmprestimo(usuario);
}


---

### Finding #4

📍 Linha(s): 18
🏷 Rótulo: major
📂 Dimensão: Erros
⚠️ Severidade: Alta

🐛 Problema: Falta de tratamento de erros em chamadas assíncronas (ex: await crypto.hash, await db.insert). Se a inserção no banco falhar, a aplicação pode quebrar sem logar o motivo.

💡 Sugestão de correção:

JavaScript

async function cadastrarUsuario(dados) {
  try {
    const salt = 10;
    const senhaHash = await crypto.hash(dados.senha, salt);
    // ... setup do usuario ...
    await db.insert('usuarios', usuario);
    logger.info('Usuario cadastrado: ' + dados.email);
    return usuario;
  } catch (error) {
    logger.error('Erro no cadastro: ', error);
    throw error;
  }
}



---

### Finding #5

📍 Linha(s): 5 e 18
🏷 Rótulo: nit
📂 Dimensão: Padrões
⚠️ Severidade: Baixa

🐛 Problema: A constante TIPOS_VALIDOS é declarada no início do arquivo, mas nunca é utilizada para validar a entrada dados.tipo durante o fluxo de cadastro.

💡 Sugestão de correção:

JavaScript

async function cadastrarUsuario(dados) {
  if (!TIPOS_VALIDOS.includes(dados.tipo)) {
    throw new Error('Tipo de usuário inválido');
  }
  const salt = 10;
  // ... restante da função
}



---

### Finding #6

---

## 💭 Reflexão final

> Responda em 1-2 parágrafos. Esta reflexão **é obrigatória**.

**Qual dimensão do checklist foi mais difícil aplicar? Por quê?**

A dimensão de Complexidade Ciclomática foi a mais desafiadora. Como desenvolvedores ativos, nossa tendência natural é querer refatorar o código imediatamente ao ler uma lógica ruim. Analisar as funções apenas sob a ótica de auditoria, mapeando aninhamentos sem reescrever o código, exigiu um esforço analítico maior para conter o "vício" de programador e focar na qualidade de software.

**O que vocês fariam diferente se revisassem o código novamente?**

Utilizaríamos ferramentas de análise estática (Linters) para automatizar a captura de erros de estilo e padrões de nomenclatura. Isso permitiria que nossa revisão humana se concentrasse exclusivamente em falhas de arquitetura, brechas críticas de segurança (como o SQL Injection) e inconsistências nas regras de negócio.

---

## 📣 Declarações


### Uso de IA como parceiro de trabalho

- [ ] Não usamos IA nesta atividade.
- [ ] Usamos IA para esclarecer conceitos teóricos.
- [ ] Usamos IA para revisar a redação dos bug reports.
- [ ] Usamos IA para discutir se um achado era ou não um defeito.
- [X] Uso específico: [Nesta atividade, o Gemini atuou como um colaborador técnico e revisor de QA e ajudou a confeccionar os Bugs e Findings]

### Declaração de autoria

Declaramos que este relatório é de autoria da dupla, que exploramos
pessoalmente a aplicação da Parte A e lemos o código da Parte B. As
findings aqui registradas representam nosso próprio julgamento
técnico.