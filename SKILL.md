---
name: manual-record-replay-madoglio
description: Cria, revisa e valida Skills para tarefas rotineiras de desktop a partir de evidências como gravação curta de tela, áudio ou transcrição e screenshots ordenados. Use quando uma demonstração manual precisar virar um procedimento reutilizável em Codex ou outro harness de IA com capacidade de operar interfaces; não use para documentar um fluxo sem intenção de transformá-lo em Skill executável.
---

# Manual Record & Replay Madoglio

## Objetivo

Transformar uma demonstração humana curta de uma tarefa rotineira em uma nova Skill reutilizável, testável e robusta.

Esta é uma **Skill criadora de Skills**. Ela não deve confundir o material de treinamento com a rotina de produção. O vídeo, o áudio e os screenshots são usados para aprender e validar o procedimento; a Skill filha gerada deve conseguir executar a rotina posteriormente sem precisar reinterpretar todo o material original.

O objetivo é aproximar funcionalmente o aprendizado por demonstração do Record & Replay, sem afirmar equivalência técnica com o recurso nativo.

## Prioridade de instruções

As instruções explícitas atuais do usuário têm prioridade sobre esta Skill.

Em caso de conflito entre evidências, use esta ordem:

1. instrução explícita atual do usuário;
2. arquivo de contexto escrito pelo usuário, se existir;
3. orientação falada/transcrita;
4. screenshots ordenados;
5. vídeo ou frames derivados do vídeo;
6. inferências do agente.

Nunca transforme uma inferência incerta em uma regra rígida sem sinalizar a incerteza.

## Pacote de evidências esperado

Preferencialmente, receber os três componentes abaixo:

1. **Vídeo de gravação da tela**
   - recomendado: até 2 minutos;
   - deve mostrar apenas o fluxo relevante;
   - evitar senhas, tokens, códigos MFA, dados pessoais desnecessários e outros segredos.

2. **Áudio de orientações**
   - deve explicar intenção, regras, exceções e o motivo de etapas que não ficam claras apenas pela interface;
   - se o ambiente não permitir que o modelo consuma áudio diretamente, obter ou solicitar uma transcrição antes da reconstrução final.

3. **Screenshots de todas as etapas**
   - ordenar cronologicamente;
   - preferir nomes como `step-001.png`, `step-002.png`, etc.;
   - incluir estados intermediários importantes, mensagens de confirmação, downloads concluídos, telas de erro relevantes e resultado final.

Arquivos opcionais:
- `context.md`: objetivo, frequência, sistema utilizado, entradas e saídas;
- `variables.md`: campos que mudam a cada execução;
- exemplos de arquivos de entrada/saída.

Estrutura recomendada:

```text
evidence/
├── recording.mp4
├── guidance.m4a
├── transcript.txt
├── context.md
└── screenshots/
    ├── step-001.png
    ├── step-002.png
    ├── step-003.png
    └── ...
```

## Regra de compatibilidade multimodal

Não pressuponha que o modelo consiga consumir áudio ou vídeo bruto diretamente.

Quando áudio ou vídeo não forem suportados diretamente pelo ambiente:

- use uma ferramenta de transcrição disponível para converter o áudio em texto;
- use screenshots fornecidos pelo usuário como a linha visual principal;
- quando possível, extraia frames relevantes do vídeo apenas para conferir transições, menus temporários, estados intermediários e timing;
- se não houver ferramenta para processar o MP4, não interrompa o trabalho quando os screenshots e a transcrição forem suficientes;
- registre no relatório quais fontes foram realmente analisadas.

## Fase 1 — Inventário e higiene

Antes de reconstruir o procedimento:

1. liste os arquivos de evidência encontrados;
2. confirme a ordem cronológica dos screenshots pelo nome e conteúdo;
3. identifique se existe transcrição do áudio;
4. identifique quais fontes podem ser efetivamente analisadas no ambiente;
5. detecte material potencialmente sensível;
6. não copie senhas, tokens, cookies, chaves, códigos MFA ou segredos para a Skill gerada;
7. trate valores sensíveis como credenciais externas a serem fornecidas ou autorizadas no momento da execução.

Produza uma tabela interna de evidências com:
- arquivo;
- tipo;
- etapa(s) coberta(s);
- confiabilidade;
- observações.

## Fase 2 — Reconstrução do fluxo

Reconstrua o procedimento em unidades semânticas. Para cada etapa, capture:

- **ID da etapa**
- **Aplicativo/janela**
- **Pré-condição**
- **Estado visual de entrada**
- **Alvo semântico**: nome do botão, menu, campo, aba, arquivo ou elemento
- **Ação**
- **Valor usado**, quando houver
- **Origem do valor**: fixo, variável, calculado, data, arquivo, usuário
- **Estado esperado após a ação**
- **Critério de validação**
- **Tempo de espera ou condição de prontidão**
- **Plano alternativo**, quando razoável
- **Risco**, se houver
- **Fonte da evidência**
- **Nível de confiança**

Não reduza a demonstração a coordenadas de mouse.

Prefira instruções como:

> Localize o botão "Exportar", clique nele e confirme que o download foi iniciado.

Evite instruções como:

> Clique em x=842, y=317.

Coordenadas podem ser registradas somente como último recurso e nunca devem ser a única forma de localizar um controle.

## Fase 3 — Separar constantes de variáveis

Classifique os elementos observados em:

### Constantes
Itens que normalmente permanecem iguais:
- aplicativo;
- menu;
- caminho funcional;
- formato de saída;
- regra de validação.

### Variáveis
Itens que podem mudar:
- data;
- cliente;
- nome de arquivo;
- período;
- pasta;
- usuário;
- valores de formulário;
- filtros;
- destinatários.

Transforme as variáveis em parâmetros claros da Skill filha.

Nunca grave como constante um valor que aparenta ser apenas um exemplo da demonstração.

## Fase 4 — Detectar decisões e exceções

Procure no áudio, screenshots e vídeo evidências de:

- menus condicionais;
- pop-ups;
- mensagens de erro;
- confirmações;
- caminhos alternativos;
- carregamentos demorados;
- paginação;
- downloads;
- arquivos já existentes;
- campos opcionais;
- mudanças de janela;
- autenticação;
- permissões.

Para cada decisão, escreva uma regra do tipo:

```text
SE <estado observável>
ENTÃO <ação>
VALIDAR <resultado>
SENÃO <caminho alternativo ou interrupção segura>
```

Não invente caminhos alternativos não demonstrados quando isso puder alterar dados ou produzir efeitos irreversíveis.

## Fase 5 — Criar a Skill filha

Crie uma pasta compatível com o padrão Agent Skills e mantenha o núcleo das instruções independente de um modelo ou fornecedor específico. Metadados próprios de um harness devem ficar em arquivos opcionais, sem impedir que outro harness leia o `SKILL.md`.

O nome técnico deve:
- usar apenas letras minúsculas, números e hífens;
- ter no máximo 64 caracteres;
- descrever a tarefa aprendida.

Estrutura mínima:

```text
<nome-da-skill>/
├── SKILL.md
└── references/
    └── learned-procedure.md
```

Opcionalmente:

```text
├── scripts/
├── assets/
└── references/
    ├── learned-procedure.md
    ├── evidence-map.md
    └── test-cases.md
```

O `SKILL.md` da Skill filha deve conter frontmatter com `name` e `description`.

A descrição deve explicar claramente:
- o que a Skill faz;
- quando deve ser usada.

## Fase 6 — Regras para a Skill filha

A Skill filha deve:

1. operar por elementos e estados semânticos, não por posições fixas;
2. validar a conclusão de cada etapa importante;
3. esperar por condições observáveis, não apenas por tempos fixos;
4. usar parâmetros para dados variáveis;
5. evitar segredos embutidos;
6. distinguir leitura, edição, envio, exclusão e outras ações com efeitos diferentes;
7. interromper com diagnóstico claro quando não conseguir confirmar um estado crítico;
8. limitar tentativas repetidas para evitar loops;
9. registrar o resultado final esperado;
10. definir critérios objetivos de sucesso.

Quando Computer Use estiver disponível, a Skill filha pode usá-lo para interagir com aplicativos e páginas, respeitando permissões e limitações do ambiente.

## Fase 7 — Validação cruzada das evidências

Compare as três fontes.

### Vídeo
Use principalmente para:
- sequência;
- transições;
- duração relativa;
- menus que aparecem temporariamente;
- movimentos entre janelas.

### Áudio/transcrição
Use principalmente para:
- intenção;
- motivo das ações;
- exceções;
- regras de negócio;
- detalhes que não são visíveis.

### Screenshots
Use principalmente para:
- texto exato de botões;
- estado da interface;
- campos;
- abas;
- mensagens;
- confirmação de cada etapa.

Quando duas fontes discordarem, não escolha silenciosamente. Registre a divergência e use a hierarquia de prioridade definida nesta Skill.

## Fase 8 — Pontuação de confiança

Calcule uma pontuação de confiança de 0 a 100 para o procedimento reconstruído.

Sugestão:

- 25 pontos: cobertura dos screenshots;
- 20 pontos: clareza da orientação/transcrição;
- 15 pontos: cobertura de transições pelo vídeo ou frames;
- 20 pontos: critérios de validação observáveis;
- 20 pontos: robustez da Skill filha contra pequenas mudanças de interface.

Interpretação:

- **90–100**: alta confiança para teste controlado;
- **80–89**: boa confiança, com pequenos pontos a revisar;
- **65–79**: procedimento parcialmente aprendido;
- **abaixo de 65**: evidência insuficiente para uma Skill confiável.

A pontuação é um indicador interno de qualidade, não uma garantia de execução.

## Fase 9 — Teste com Computer Use

Quando o usuário solicitar teste e Computer Use estiver disponível:

1. faça primeiro uma execução controlada;
2. confirme os parâmetros da execução;
3. execute etapa por etapa;
4. valide estados intermediários;
5. registre divergências entre a demonstração e a interface atual;
6. atualize a Skill filha para torná-la menos frágil;
7. execute novamente quando apropriado;
8. não declare a Skill validada apenas porque ela iniciou corretamente.

Para ações irreversíveis, financeiras, destrutivas ou que enviem informação externamente, respeite as confirmações e políticas aplicáveis do produto.

## Fase 10 — Critério de aprovação

Considere a Skill filha pronta para uso rotineiro somente quando:

- o fluxo completo estiver reconstruído;
- os parâmetros variáveis estiverem identificados;
- os principais estados intermediários tiverem validação;
- o resultado final puder ser confirmado;
- não houver segredos embutidos;
- os pontos de falha comuns tiverem tratamento;
- pelo menos uma execução controlada tiver sido validada, quando Computer Use estiver disponível.

## Fase 11 — Preparação para automação recorrente

Se o usuário também quiser agendar a rotina:

1. mantenha o material de treinamento fora do caminho crítico da execução;
2. faça a automação chamar a Skill filha, não reinterpretar o vídeo diariamente;
3. documente entradas necessárias a cada execução;
4. documente dependências de login, sessão, MFA, rede, arquivos e aplicativo;
5. defina o critério de sucesso;
6. defina o comportamento em caso de falha;
7. registre o que deve ser comunicado ao usuário ao final;
8. somente configure o agendamento se o ambiente atual suportar a execução recorrente e as ferramentas necessárias.

Não prometa que uma automação continuará funcionando se depender de uma sessão local, aplicativo ou permissão que não esteja disponível no momento agendado.

## Saídas obrigatórias

Ao finalizar uma sessão de aprendizado, entregue:

### 1. Resumo do procedimento
- nome;
- objetivo;
- gatilho;
- entrada;
- saída;
- aplicativos;
- duração aproximada observada.

### 2. Procedimento reconstruído
Tabela ou lista de etapas com:
- ação;
- alvo;
- validação;
- variável;
- exceção;
- confiança.

### 3. Mapa de evidências
Mostre quais arquivos sustentam cada etapa.

### 4. Ambiguidades e riscos
Liste somente pontos relevantes.

### 5. Pontuação de confiança
Informe a nota e os fatores que a reduziram.

### 6. Skill filha
Crie ou apresente a estrutura completa da nova Skill.

### 7. Plano de teste
Explique como verificar a Skill com uma execução controlada.

### 8. Status
Use uma destas classificações:
- `APRENDIDA — PRONTA PARA TESTE`
- `TESTADA — PRONTA PARA USO`
- `PRECISA DE MAIS EVIDÊNCIAS`

## Padrão de qualidade Madoglio

Uma Skill filha criada por este método deve seguir estes princípios:

- **Ver → Entender → Generalizar → Validar → Executar**
- nunca assumir que um clique bem-sucedido significa que a etapa terminou;
- preferir estados observáveis a temporizadores fixos;
- preferir nomes e funções da interface a coordenadas;
- separar demonstração de produção;
- não esconder incertezas;
- registrar critérios de sucesso;
- preservar a possibilidade de revisão humana.

## Limite conceitual

Descreva o método como uma aproximação manual orientada por evidências ao Record & Replay.

Não afirme que ele possui a mesma telemetria, integração ou fidelidade do Record & Replay nativo. O método reconstrói o fluxo usando materiais fornecidos e ferramentas disponíveis; o Record & Replay nativo pode observar diretamente ações e conteúdo de janelas durante a demonstração.

## Exemplo de invocação

Quando o usuário disser algo equivalente a:

> Use o Manual Record & Replay Madoglio para aprender a tarefa desta pasta e criar uma nova Skill.

Faça o inventário das evidências, reconstrua o fluxo, gere a Skill filha e apresente o relatório de qualidade. Se houver evidência suficiente, prepare o teste por Computer Use. Não torne o vídeo uma dependência permanente da execução final.
