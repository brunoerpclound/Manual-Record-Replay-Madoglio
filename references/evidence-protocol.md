# Protocolo de Evidências — Manual Record & Replay Madoglio

## Objetivo
Aumentar a precisão do aprendizado de tarefas rotineiras combinando três perspectivas complementares: sequência (vídeo), intenção (áudio/transcrição) e estado visual (screenshots).

## Antes de gravar
- Feche janelas irrelevantes.
- Oculte notificações.
- Não exiba senhas, tokens, códigos MFA ou dados pessoais desnecessários.
- Use dados de exemplo quando possível.
- Comece a gravação já próximo do estado inicial real da tarefa.

## Vídeo
- Até 2 minutos como padrão recomendado.
- Execute uma única tarefa por gravação.
- Faça os movimentos de maneira natural, mas sem ações irrelevantes.
- Se uma etapa depender de espera, deixe aparecer o sinal visual de conclusão.

## Áudio
Explique:
- qual é o objetivo;
- o que muda a cada dia;
- o que nunca deve mudar;
- por que cada decisão é tomada;
- o que fazer quando a tela estiver diferente;
- como reconhecer que a tarefa terminou corretamente.

## Screenshots
Capture:
- estado inicial;
- cada mudança importante;
- menus temporários relevantes;
- mensagens de confirmação;
- resultado final;
- telas de erro que façam parte de caminhos conhecidos.

Nome recomendado:
`step-001.png`, `step-002.png`, `step-003.png`...

## Contexto opcional
Crie `context.md` com:

```text
Tarefa:
Frequência:
Aplicativos:
Estado inicial:
Entrada:
Saída:
Variáveis:
Resultado esperado:
Exceções conhecidas:
```

## Critério de qualidade
A demonstração é boa quando outra pessoa, sem conhecer a rotina, consegue explicar:
1. onde a tarefa começa;
2. quais decisões existem;
3. quais dados variam;
4. como cada etapa é confirmada;
5. como reconhecer o resultado final.
