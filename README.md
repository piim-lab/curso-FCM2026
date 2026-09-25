# Oficina prática: IA na auditoria de questões

Neste exercício, vamos comparar um prompt genérico com um prompt estruturado para localizar, em provas de concurso, questões que merecem revisão por possível ambiguidade, problema no gabarito ou outro indício de recurso de anulação.

O objetivo não é provar que uma questão deve ser anulada. A IA funciona como triagem: aponta evidências para uma pessoa especialista conferir. Um alerta não é um parecer jurídico nem uma decisão oficial.

## Materiais

- [Provas disponíveis](provas/): cadernos de diferentes cargos para escolher uma amostra.
- [Roteiro e definições](ppt%20e%20aux/Roteiro%20e%20Defini%C3%A7%C3%B5es.md): material de referência teórica.
- [Slides em texto](ppt%20e%20aux/Workshop%20-%20FCM%20-%20Profs%20Daniel%20e%20Danilo.txt): contexto e estrutura de prompts abordados na exposição.
- [Página do processo seletivo do IFAM](provas/Link%20FCM%20-%20IFAM.docx): referência externa incluída entre os materiais. O edital não está reproduzido neste repositório.

## Antes de começar

1. Escolha uma ferramenta de IA que aceite anexar PDFs. Os nomes dos recursos variam entre plataformas.
2. Escolha **uma prova** da pasta `provas/` e, se a ferramenta permitir, delimite a análise a uma disciplina ou a poucas questões. Uma prova inteira pode exceder o contexto disponível ou dificultar a conferência.
3. Confira se você tem autorização para enviar o arquivo à ferramenta escolhida. Não envie documentos sigilosos, dados pessoais ou questões inéditas sem autorização; prefira os materiais públicos e as regras de privacidade aprovadas pela sua instituição.
4. Para avaliar aderência ao edital, disponibilize também o edital e a bibliografia aplicáveis. Sem esses documentos, peça à IA que marque essa verificação como **não realizada**. A pasta contém provas e um link de referência, não o edital completo.
5. Tenha à mão o gabarito oficial, se disponível. Se não estiver junto ao material enviado, peça que a IA não o presuma.

## Parte 1: teste com prompt ruim

Anexe a prova escolhida e envie este prompt, sem acrescentar critérios:

> Analise esta prova e me diga quais questões podem ser anuladas.

Observe a resposta antes de orientar melhor a IA. Registre:

- A IA identificou a questão e citou o trecho que sustenta a suspeita?
- Explicou qual seria o problema ou apenas afirmou que a questão é “ambígua”?
- Inventou gabarito, regra de edital, lei ou referência que não foi fornecida?
- A resposta ajuda uma pessoa especialista a conferir o caso?

Não é necessário que a primeira resposta encontre um problema real. O propósito é observar o que fica indefinido quando o pedido não delimita contexto, critérios nem evidências.

## Parte 2: teste com prompt estruturado

Na mesma conversa ou em uma conversa nova, anexe a mesma prova. Forneça também o edital, a bibliografia e o gabarito oficial, caso estejam disponíveis e seu uso esteja autorizado. Cole e adapte o prompt abaixo:

```text
PAPEL
Atue como assistente de triagem técnica de questões de concurso. Sua função é apontar itens que mereçam revisão humana, não decidir recursos nem declarar que uma questão deve ser anulada.

CONTEXTO
Analise somente os arquivos fornecidos nesta conversa.
Prova/cargo: [informe, se souber]
Edital e bibliografia: [anexados / não fornecidos]
Gabarito oficial: [anexado / não fornecido]
Recorte desejado: [prova toda, disciplina ou números das questões]

CRITÉRIOS DE TRIAGEM
Procure evidências verificáveis de:
1. Mais de uma alternativa potencialmente correta, ou nenhuma alternativa correta.
2. Enunciado, comando ou alternativa com ambiguidade relevante para a resposta.
3. Contradição entre o enunciado, as alternativas e o gabarito fornecido.
4. Possível cobrança fora do conteúdo previsto no edital, somente se o edital e a questão estiverem disponíveis para comparação.
5. Falha material que impeça responder, como informação essencial ausente ou dado inconsistente.

REGRAS
- Não invente texto de questão, gabarito, edital, lei, jurisprudência ou bibliografia.
- Diferencie o que está literalmente no documento da sua interpretação.
- Para cada alerta, cite cargo/arquivo, número da questão e página, além de um trecho curto que possa ser localizado no original.
- Explique por que a evidência pode justificar revisão e indique também uma interpretação alternativa que preserve a validade do item, se houver.
- Não trate dificuldade, preferência pessoal ou discordância sem evidência como motivo suficiente para anulação.
- Se o PDF estiver ilegível, incompleto ou sem paginação identificável, informe a limitação e não complete lacunas por suposição.
- Se edital, bibliografia ou gabarito não tiver sido fornecido, marque a verificação correspondente como “não realizada”; não tente inferi-los.
- Use “indício para revisão”, “sem indício identificado” ou “inconclusivo”. Não emita decisão oficial.

FORMATO DE SAÍDA
Apresente uma tabela com estas colunas:
Arquivo/cargo | Questão e página | Trecho literal | Critério observado | Evidência e análise | O que falta conferir | Nível de confiança

Inclua apenas questões com algum indício concreto na tabela. Depois, informe:
1. Quais critérios não puderam ser avaliados e por quê.
2. Quais documentos ou conferências humanas são necessários antes de qualquer conclusão.
3. Se nenhuma questão tiver indício, declare isso sem criar exemplos.
```

Compare os resultados dos dois testes. A estrutura do segundo prompt torna as respostas mais rastreáveis, mas não garante que estejam corretas: confira cada trecho no PDF e cada fundamento nos documentos oficiais.

## Parte 3: modifique e teste

Escolha uma alteração e repita a análise. Não mude várias coisas ao mesmo tempo, para conseguir perceber o efeito:

- Troque o cargo ou a prova analisada.
- Limite a análise a uma disciplina ou a algumas questões.
- Acrescente o edital ou o gabarito e observe quais verificações passam a ser possíveis.
- Ajuste os critérios de triagem para o tipo de questão escolhido.
- Peça uma segunda tabela ordenada por prioridade de revisão, sem transformar prioridade em probabilidade de anulação.

Registre qual versão do prompt usou, quais arquivos forneceu e que diferenças observou. Os participantes podem compartilhar prompts e resultados, mas devem evitar tratar uma resposta da IA como validação factual.

## Roteiro rápido para demonstração ao vivo

1. Apresente uma prova e explique o recorte escolhido.
2. Execute o prompt ruim e leia a resposta procurando afirmações vagas, sem evidência ou não verificáveis.
3. Execute o prompt estruturado com a mesma prova e compare identificação da questão, citação, justificativa e limites declarados.
4. Abra o PDF e confira manualmente pelo menos um alerta e um caso que a IA considerou inconclusivo ou não sinalizou.
5. Peça ao grupo que altere um critério ou acrescente o edital e repita o teste.
6. Encerre reforçando que a IA apoia a triagem; a análise técnica e qualquer decisão permanecem sob responsabilidade humana.

## Ficha de observação

| Registro | Anotação |
| --- | --- |
| Ferramenta/modelo utilizado | |
| Arquivo, cargo e recorte analisados | |
| Documentos de contexto fornecidos | |
| Versão do prompt | Ruim / estruturado / adaptado |
| Questões apontadas e evidências citadas | |
| Afirmações que precisaram de correção | |
| Limitações observadas | |
| Próxima mudança a testar | |