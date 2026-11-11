Filled example. Copy the shape and the voice, not the facts. The boxes below are the ones this change actually earned: funcionalidade is the primary type, breaking change is Não, the README item stays open, and the branch-sync item stays open because nobody compared the branch to the target.

Moderation-Microservice
Descrição
Padroniza o tratamento de erro do caminho HTTP. O controller deixa de engolir a exceção, o service lança um AppException com código e status, e o Nest responde com esse status.

Implementa [#1gaax8j7rwb](https://app.clickup.com/t/36971573/1gaax8j7rwb)

Adiciona AppException e o mapa errors, cobrindo verificação e whitelist.

Registra o ValidationPipe global e remove o validateOrReject duplicado de createVerification.

Remove os try/catch de VerificationsController, ManagerController e WhitelistController.

Traduz ER_DUP_ENTRY no update da whitelist para domainAlreadyWhitelisted (409).

Faz o SendEmailConsumer reprovar o job quando sendWarningEmail devolve error.

Sobe o tempo dos hooks de e2e de verificação e manager para 30 segundos.

Tipo de Mudança
- [ ] Corrige um Bug
- [x] Implementa Funcionalidade
- [ ] Atualiza o Readme/Documentações
- [ ] Refatoração de código
- [ ] Atualização de pacote/linguagem
- [ ] Ajuste do estilo/qualidade de código
- [ ] Ajuste de build/deploy
- [ ] Outro: **___**
Meu código introduz um BREAKING CHANGE?

- [ ] Sim
- [x] Não
Como foi testado?
Através dos testes unitários via comando npm run test.

28 suites, 232 testes, todos passando.

Checklist
- [x] Meu código e commits seguem os padrões do projeto/equipe
- [x] Eu fiz uma autorrevisão do meu código
- [ ] Fiz as atualizações necessárias no Readme/documentações
- [x] Foram adicionados testes para as mudanças (para correção de bugs/novas funcionalidades)
- [ ] Minha branch está atualizada com a versão mais atual da branch alvo
Informações adicionais
O corpo de erro passa a ser { statusCode, error, message, code }. O manager não responde mais 200 com { error }. A whitelist não transforma mais todo erro em 422: domínio inválido e update sem campos continuam 422, registro ausente fica 404 e domínio duplicado fica 409. Moderação fora de pending_review responde 409. Fila, webhook, saveErrorResult, checkUrlAccessibility, findOneOrFail, strategies, logger e cron ficaram como estavam. O item de Readme fica desmarcado porque a documentação não foi atualizada nesta mudança. O último item do checklist fica desmarcado porque não conferi se a branch está atualizada com a branch alvo.
