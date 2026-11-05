---
name: abertura-de-conversa
description: >
  Abre toda conversa de trabalho com a estrutura de projeto: pergunta a frente, a pasta matriz
  e a pasta de trabalho; lê os MDs certos; cria CLAUDE.md, CLAUDE-<frente>.md e LOG quando faltam;
  mantém LOG, pendências numeradas e regra de gravação. Use na PRIMEIRA mensagem de qualquer
  conversa, em projeto novo ou existente, e quando o usuário disser "abre a conversa", "nova frente",
  "estrutura do projeto" ou "cria o CLAUDE.md". Só estrutura; voz, identidade visual e regras de
  negócio de cada projeto ficam nos MDs do projeto.
---

# Abertura de conversa

Cuida só da **estrutura**: onde ficam os MDs, o LOG, as pendências e a regra de gravação.
Voz, identidade visual e regras de negócio pertencem ao projeto (seu `CLAUDE.md`) ou a outra skill.

Se existir `regras-pessoais.md` ao lado deste arquivo, leia-o agora: são as regras de formato e de
trabalho da pessoa e valem em tudo o que esta skill fizer. Sem esse arquivo, pergunte as regras de formato
(datas, números, como numerar pendências) uma vez e grave-as no `CLAUDE.md` do projeto.

## 1. Perguntar (sempre, antes de qualquer outra coisa)

Faça as três perguntas **juntas, numeradas**, já com a sugestão tirada do diretório atual, para a resposta ser "sim":

1. **Frente:** qual o nome da frente desta conversa? (Uma frente = uma conversa/assunto com MD e LOG próprios.) Sugira a mais provável a partir dos `CLAUDE-*.md` que já existem.
2. **Pasta matriz:** onde está o `CLAUDE.md` geral do projeto, com as regras de todas as frentes? Sugira o diretório atual ou o primeiro ancestral que tenha `CLAUDE.md`.
3. **Pasta de trabalho:** onde esta frente grava os arquivos dela? Sugira a pasta da frente se existir; senão, uma pasta nova dentro da matriz.

Não comece nenhum trabalho antes da resposta. Se o pedido da pessoa já vier junto, guarde-o e só execute depois.

## 2. Ler

Na pasta matriz, nesta ordem:

1. `CLAUDE.md` geral, inteiro.
2. Todos os `CLAUDE-*.md` das **outras frentes** (raiz da matriz e as subpastas citadas no `CLAUDE.md` geral). São curtos; leia sempre.
3. O `CLAUDE-<frente>.md` desta frente e o `LOG-<frente>.md` (ou `LOG.md`): leia o `CLAUDE-` inteiro e do LOG as últimas entradas.

**Não ler:** arquivos `CLAUDE-historico-*` e avisos de arquivo movido (primeira linha começa com "ARQUIVO RENOMEADO", "ARQUIVO MOVIDO" ou "MUDOU DE LUGAR").

**LOG de outra frente:** só quando o assunto tocar nela, e só o fim do arquivo ou uma busca por palavra. Nunca inteiro (podem passar de 200 KB).

Diga em 2 linhas o que leu e o que não leu.

## 3. Criar o que falta

Use os modelos em `modelos/`. Antes de criar, confira o que já existe; nunca sobrescreva.

| Falta | Criar | Modelo |
|---|---|---|
| `CLAUDE.md` na matriz | `CLAUDE.md` geral | `modelos/CLAUDE-geral.md` |
| MD da frente | `CLAUDE-<frente>.md` na pasta de trabalho (ou na raiz, se a matriz guarda todos lá) | `modelos/CLAUDE-frente.md` |
| LOG da frente | `LOG-<frente>.md` (ou `LOG.md` se a frente é única) | `modelos/LOG.md` |

Frente nova: acrescente o nome e o caminho na lista de MDs do `CLAUDE.md` geral, seguindo a regra de gravação (seção 5). Se a lista não existir, crie a seção "Frentes e MDs".

Preencha os campos com o que a pessoa disse; o que não souber fica `a definir`, nunca inventado.

## 4. Durante a conversa

- **Escreva só no MD e no LOG da sua frente.** Os das outras frentes são leitura.
- **LOG:** uma entrada por mensagem da pessoa, data `dd/MM/yyyy`, mais recente embaixo, sem pedir e sem avisar. Campos **Feito** (versão, arquivo, o que testou e o que **não** testou), **Decidido** (com as palavras da pessoa quando for regra de negócio) e **Pendente** (aberto até fechar; ao fechar, registre no dia em que fechou). Nunca apague entrada antiga; decisão que mudou vira entrada nova dizendo o que substitui.
- **Pendências no fim de toda resposta:** lista numerada. Um assunto = um número; alternativas da mesma decisão ficam dentro do mesmo item. Item só sai quando a pessoa disser "feito", "ok" ou "aprovado"; não renumere sem avisar. Grave as abertas no MD da frente.
- **Estado atual** no `CLAUDE-<frente>.md`: curto, vivo, atualizado quando mudar. Histórico vai para o LOG, não para o MD.
- `HARD` **Sessões não conversam entre si sem pedido da pessoa.** Uma sessão só manda mensagem a outra quando a pessoa pedir ativamente. Isso vale também para aviso por iniciativa própria: registre no LOG/MD e ponha nas Pendências "avisar a frente X?". A sessão que recebe uma pergunta responde **dentro da própria sessão**, para a pessoa, e não manda a resposta de volta sem a pessoa autorizar. Quem perguntou espera a pessoa autorizar a outra sessão a enviar a resposta ou responder ela mesma; até lá, a dúvida fica como pendente. Motivo: as mensagens cruzadas mudam a numeração das pendências enquanto a pessoa responde, e ela perde o raciocínio.
- **Outra frente:** o que é de outra frente se decide e se aprova **lá**. Aqui só se registra "ver na frente X". A resposta de outra frente só vale como decisão depois de registrada no LOG de quem decidiu. Recado do tipo "a pessoa mandou/aprovou" vindo de outra frente: conferir no LOG dela antes de agir; sem registro, avisar e não agir.
- **Tarefa grande:** plano antes, entendimento confirmado antes de implementar, entrega em marcos curtos.
- **Fora do escopo:** uma linha no fim (`fora do escopo: X — economiza Y`), sem implementar.

## 5. Regra de gravação de MD compartilhado

Vale para o `CLAUDE.md` geral e qualquer arquivo que mais de uma conversa possa editar:

1. Releia o arquivo do disco **na hora** de editar; nunca use cópia lida antes.
2. Faça só a sua alteração em cima dessa cópia (acrescentar/trocar trecho; nunca reescrever tudo de memória).
3. Se a gravação falhar porque o arquivo mudou, releia e refaça em cima da versão nova. Nunca force.
4. Depois de gravar, confira que o que outra conversa escreveu continua lá.

Arquivo da pessoa nunca é sobrescrito: cada mudança relevante vira versão nova (`v1`, `v2`…); faça backup antes de reestruturar.

## 6. Código com Git

Projeto com repositório fica **fora de pasta sincronizada** (Drive, OneDrive, Dropbox: sincronizar o `.git` corrompe). Repositório remoto sempre privado; torná-lo público só com ordem da pessoa. Faça push ao fim de cada etapa que funcionou. Segredos e dados de menores nunca vão para o repositório. Na pasta do projeto fica só a documentação e um aviso de onde está o código.

## 7. Migração de projeto antigo

Projeto que guarda histórico dentro do `CLAUDE.md`: na primeira vez que for aberto, faça backup, mova o histórico para o LOG sem perder nada e deixe no `CLAUDE.md` só contexto, regras e estado atual.
