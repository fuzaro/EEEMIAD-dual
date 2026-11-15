# ✉️ MENSAGEM ENTRE SESSÕES · [assunto]

*Pacote fechado enviado de uma sessão que decide para uma sessão no terreno (ver [docs/pt/03](../../docs/pt/03-comunicacao-entre-sessoes.md))*

> [!IMPORTANT]
> Copie a partir da linha "Primeira linha" e envie **de uma vez**. Não inclua segredos. Para escrita, o operador precisa autorizar também na sessão que age.

```text
Primeira linha: [SOMENTE LEITURA | ESCRITA AUTORIZADA] · [o que é, em uma frase] · [sistema/arco]

Contexto: [quem pede, por quê, decisão do operador e data se for escrita]

NÃO FAÇA: [ex.: não rode job run · não grave nada · não imprima tokens ou senhas]

Passos (responda cada um com o comando executado e a saída observada):
1. [comando ou verificação]
2. [comando ou verificação]
3. [...]

Se algo bloquear (credencial ausente, 403, divergência do esperado): pare e responda com a saída; não procure a credencial por conta própria.

Formato da resposta: um bloco por passo · comando · saída (resumida se longa) · segredos omitidos.
Responda para: [nome da sessão que decide]
```

## Correção (só se necessário, uma vez)

```text
Primeira linha: Correção do item [N] da mensagem anterior · [assunto]
O certo é: [instrução correta, completa]
Restrição reafirmada: [ex.: nada roda sem o token e sem a autorização do operador aí]
```

## Registro (sessão que decide, no charter do arco)

| Mensagem | Ação | Chave 1 | Chave 2 | Resposta recebida | Reexecutado daqui | Nível R15 |
|---|---|---|---|---|---|---|
| [assunto] | leitura / escrita | [decisão, data] | [autorização na sessão que age] | [sim, data / não] | [comando → resultado] | [2 / 3b / 4] |
