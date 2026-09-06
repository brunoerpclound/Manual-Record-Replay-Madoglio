# Template conceitual para a Skill filha

Use este template como referência ao gerar uma nova Skill a partir da demonstração.

```markdown
---
name: <nome-tecnico-em-kebab-case>
description: <o que faz + quando usar>
---

# <Nome humano da rotina>

## Objetivo
<resultado final da rotina>

## Entradas
- <parâmetro 1>
- <parâmetro 2>

## Pré-condições
- <aplicativo/sessão/arquivos necessários>

## Fluxo
### Etapa 1 — <nome>
1. Confirme <estado inicial>.
2. Localize <elemento semântico>.
3. Execute <ação>.
4. Aguarde <condição observável>.
5. Valide <estado esperado>.

### Etapa 2 — <nome>
...

## Decisões
- SE <estado>, ENTÃO <ação>, VALIDAR <resultado>.
- SENÃO <fallback seguro>.

## Critério de sucesso
- <resultado observável>

## Falhas
- Se <problema>, tente <correção limitada>.
- Se não resolver, interrompa e informe <diagnóstico>.

## Segurança
- Não armazenar credenciais na Skill.
- Não repetir indefinidamente ações que possam ter efeito duplicado.
```
