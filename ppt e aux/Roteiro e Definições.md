# **Guia Fundamental de Inteligência Artificial Generativa**

## **Aplicações na Avaliação e Auditoria de Questões de Concursos**

Este documento estabelece os fundamentos necessários para a compreensão e a operação de Modelos de Linguagem de Grande Escala (LLMs) no ecossistema de bancas de concurso e de avaliação educacional. O objetivo é capacitar especialistas em conteúdo, professores e revisores a extraírem o máximo de rigor lógico e técnico dessas ferramentas, reduzindo os riscos de alucinação e garantindo a qualidade psicométrica dos itens. O escopo deste material não contempla formar programadores em inteligência artificial.

### **1\. Memória e IAGen: O Modelo Estático vs. A Sessão Contínua**

Um dos maiores equívocos ao interagir com uma IA é acreditar que ela "aprende" sobre você ou se recorda de regras estabelecidas em dias anteriores. **Por padrão**, os LLMs (como GPT-4, Claude ou Gemini) operam em um estado chamado *Stateless* (sem estado/sem memória persistente).

Isso significa que o "cérebro" do modelo fica congelado no tempo assim que o treinamento for concluído. Cada nova requisição (cada envio de mensagem à API) é tratada como um universo em branco. A IA não se lembra da questão que avaliou ontem, a menos que você a forneça novamente.

**O que isso significa na prática das bancas?** Se a sua banca possui uma regra específica (por exemplo: "Nossas alternativas devem ter no máximo 3 linhas e não podem usar a palavra 'exceto'"), você não pode ensinar isso à IA uma vez e esperar que ela se lembre para sempre. Essa regra precisa ser injetada automaticamente em **toda e qualquer** requisição. A única "memória" que o modelo possui é o texto que está presente na janela de conversa atual.

**Disclaimer:** Embora a arquitetura-base das APIs seja stateless, é fundamental alertar que as interfaces comerciais (como os aplicativos web do ChatGPT ou Claude) estão rapidamente introduzindo recursos stateful (como funções de "Memória" contínua). Dependendo das configurações e dos Termos de Uso, a plataforma pode passar a reter seu histórico para personalizar respostas ou até mesmo utilizar seus dados (como questões inéditas do concurso) para treinar futuras versões do modelo. Note que muitas vezes isso pode ocorrer sem exigir um consentimento explícito a cada nova sessão. Para garantir o sigilo das provas, é vital auditar as configurações de privacidade da IA utilizada ou priorizar ambientes corporativos (Enterprise) onde o uso de dados seja estritamente bloqueado.

### **2\. Engenharia de Prompts e Personalização (Persona)**

O *Prompt* é a instrução, o texto de entrada que você envia para o modelo. No entanto, a Engenharia de Prompts vai muito além de "fazer uma pergunta". Trata-se de condicionar o comportamento estatístico da IA para gerar o resultado mais adequado.

A técnica mais fundamental de personalização é a **Assunção de Persona**. Os LLMs foram treinados com uma vastidão de textos: desde fóruns de internet amadores até teses de doutorado. Se você pedir apenas "Avalie esta questão de Direito Administrativo", o modelo pode adotar um tom enciclopédico, estudantil ou superficial.

Ao personalizar o prompt com uma persona, por exemplo: *"Atue como um auditor sênior de bancas examinadoras, especialista em psicometria, taxonomia de Bloom e jurisprudência dos tribunais superiores"*, você "restringe" o espaço de busca do modelo, forçando-o a usar o vocabulário, o rigor crítico e os padrões de excelência de um profissional da área. Isso transforma uma resposta genérica em um parecer técnico detalhado e mais focado na área em questão.

### **3\. A Orquestração do Contexto**

Se o prompt é a "ordem", o contexto é o "terreno" onde essa ordem será executada. Como vimos, a IA não tem memória (ao menos não por padrão). Portanto, a Orquestração de Contexto é a habilidade de fornecer à IA, em tempo real, as bases documentais exatas das quais ela não pode se desviar.

Em um cenário de concurso público, o contexto é imprescindível. Uma questão pode estar correta a um primeiro momento, mas pode estar errada segundo a bibliografia específica exigida no edital. Orquestrar o contexto significa que, antes de mostrar a questão para a IA, o sistema (ou você) fornece:

* O trecho exato da lei (Lei Seca atualizada)  
* O nível de dificuldade esperado (Nível Médio vs. Procurador da República)  
* As regras do edital

Quando você embasa a avaliação da IA em um contexto fechado e rígido (uma técnica frequentemente automatizada via RAG \- *Retrieval-Augmented Generation*), você reduz consideravelmente as **alucinações.** Ou seja, quando a IA inventa teorias, dados históricos, leis ou fatos para tentar agradar o usuário.

Na rotina de produção de itens, o RAG atua como um "bibliotecário instantâneo". Em vez do conteudista precisar colar manualmente dezenas de páginas de um manual técnico, regra gramatical ou apostila no chat a cada nova questão elaborada, o sistema RAG varre o acervo privado da banca (editais, banco de questões anteriores e bibliografia validada) e entrega à IA apenas o trecho exato necessário para auditar aquela questão específica. Isso garante (de forma automatizada) que o elaborador não insira conceitos fora do escopo e que o gabarito reflita estritamente a literatura exigida para o cargo, blindando o trabalho da equipe desde a concepção da prova.

Além de acelerar a criação, essa mesma arquitetura RAG  é a maior aliada na fase de revisão e auditoria. Imagine que o conteudista terminou de redigir a questão e quer testar a "resistência" dela. Ao submeter esse rascunho da questão ao sistema RAG, a IA irá cruzar com todo o acervo de bibliografias exigidas, normas técnicas, fórmulas e até mesmo o histórico de questões anuladas da banca. A IA passa a atuar como um "caçador de recursos" proativo, podendo alertar: "***A alternativa B está correta segundo a literatura geral, mas há uma exceção de acordo com um autor específico exigido na página 42 do edital que abre margem para dupla interpretação***". Dessa forma, o revisor identifica "furos" e ambiguidades que os candidatos certamente usariam para pedir a anulação do item, corrigindo o problema antes mesmo da prova ir para a gráfica.

### **4\. A Moeda da IA: Compreendendo os Tokens**

Geralmente, as pessoas associam a palavra "Token" apenas à cobrança financeira ("***Paga-se X centavos por mil tokens***"). Contudo, para quem elabora fluxos de trabalho, o token possui implicações mais profundas.

Um token não é necessariamente uma palavra inteira. Para a IA, tokens são "pedaços de palavras", sílabas ou raízes morfológicas. Em inglês, 1 token equivale a cerca de 3/4 de uma palavra. Em português, devido aos acentos e às construções gramaticais mais complexas, uma única palavra pode ser dividida em 2 ou 3 tokens.

O token impacta o trabalho da banca de quatro formas:

1. **Custo Financeiro:** É o *taxímetro* da API. Você paga pelo que entra (o tamanho do seu edital no contexto) e pelo que sai (o tamanho da análise gerada).  
2. **Capacidade (Janela de Contexto):** Todo modelo tem um limite físico (ex: 128 mil tokens). É como a memória RAM de um computador. Se você tentar inserir um PDF de 5.000 páginas de uma vez só no prompt, o modelo "transbordará" e rejeitará a requisição.  
3. **Latência (Tempo de Resposta):** Quanto mais tokens a IA precisar ler e, principalmente, quanto mais tokens ela precisar gerar, mais lenta será a resposta. Em avaliações em lote (milhares de questões), isso poderia impactar a velocidade da produção.  
4. **Foco e "Perda no Meio" (Efeito U-Shape):** Mesmo que um modelo suporte uma janela gigante de tokens, a atenção dele não é uniforme. Há estudos que mostram que a IA presta muita atenção no começo do prompt e no final dele, podendo ignorar instruções perdidas no meio de um mar de tokens muito extenso.

**Saiba mais:** No estudo referência de pesquisadores da Universidade de Stanford e UC Berkeley (Liu et al., 2023, *"Lost in the Middle: How Language Models Use Long Contexts"*) comprovou que os LLMs apresentam um desempenho em formato de "U". Eles recuperam com alta precisão as informações que estão no **início** e no **final** do contexto, mas o desempenho despenca para dados "perdidos no meio" de um prompt longo. 

**⇒** Para pessoas responsáveis por elaborar e avaliar questões de concurso, há uma lição prática. Coloque o acervo bibliográfico no início do prompt, mas sempre reforce as instruções críticas de avaliação (ex.: "*verifique se cada distrator é plausível, semanticamente bem construído e livre de ambiguidades, garantindo que as alternativas incorretas atraiam apenas o candidato desavisado sem gerar margem para anulação*") no final, logo antes de colar a questão.

#### **4.1. Estimativas Práticas: Custos, Tokens e Latência**

Para planejar a operação, é vital separar o uso via interface comercial (o site da IA) do uso via integração sistêmica (API), onde o RAG realmente acontece.

* **Assinaturas (Uso Manual):** Planos como ChatGPT Plus, Claude Pro ou Gemini Advanced custam cerca de **US\$ 20 mensais** por usuário. Eles oferecem acesso aos melhores modelos, mas possuem limites estritos de mensagens (ex.: 40 a 80 mensagens a cada 3 horas) e exigem o "copiar e colar" manual, o que quebra a produtividade em escala.  
* **Uso via API (Automação e RAG):** Aqui você não paga mensalidade, mas sim pelos tokens processados. É o cenário ideal para auditar lotes de questões conectadas aos editais.  
  * **Estimativa de Tokens por Questão:** Injetar uma questão de múltipla escolha típica, acompanhada das regras do edital e de um trecho da bibliografia (contexto), consome cerca de **1.500 a 3.000 tokens de entrada**. O parecer analítico da IA (saída) costuma gerar entre **500 a 1.000 tokens**.  
  * **Custo Real:** Utilizando modelos de alta capacidade (como Claude 3.5 Sonnet ou GPT-4o), o custo médio para auditar **uma questão** gira em torno de **US\$ 0,01 a US\$ 0,03** (aproximadamente de 5 a 15 centavos de real). É um valor irrisório quando comparado ao custo financeiro, judicial e reputacional da anulação de um item na prova.  
* **Tempo de Resposta (Latência):** O tempo que a IA leva para avaliar a questão depende do raciocínio exigido.  
  * Modelos rápidos e econômicos entregam a resposta em 1 a 3 segundos (não recomendados para auditoria profunda).  
  * Modelos *Flagship* (alta capacidade lógica) levam cerca de **5 a 15 segundos** para gerar um relatório detalhado ("Chain-of-Thought").  
  * Modelos especializados em raciocínio complexo profundo (como a família OpenAI o1) podem dedicar de **15 a 45 segundos** analisando o item antes de sequer começar a digitar a resposta, garantindo um nível de segurança técnica sem precedentes, embora com um custo levemente maior.

### **5\. Os Diferentes Modelos (A Escolha do Motor)**

Não existe "uma única IA". Os modelos variam drasticamente em arquitetura e propósito. Atualmente, os principais motores lógicos são o GPT (OpenAI), Claude (Anthropic) e Gemini (Google).

Dentro de cada família, há subdivisões importantes. Existem modelos massivos e muito inteligentes (projetados para raciocínio complexo, código e análise semântica profunda) e modelos menores e mais rápidos (ótimos para classificar textos simples, traduzir ou resumir rapidamente). Para a função de **auditar o raciocínio de uma questão**, avaliar a plausibilidade de um distrator ou buscar ambiguidades que um candidato usaria para entrar com recurso, deve-se sempre optar pelos modelos de maior capacidade analítica (os chamados *Flagship Models*, ou modelos de ponta), mesmo que sejam mais custosos em termos de tokens.

Acho que dá pra discutir mais sobre os modelos…

### **6\. A "Intensidade" do Modelo: Controlando a Temperatura**

Esta é, sem dúvida, a configuração técnica mais importante para o uso de IA em avaliações formais de alto risco. Os LLMs possuem um parâmetro regulável chamado **Temperatura**, que varia de 0 a 1 (ou 0 a 2, dependendo do fornecedor).

A temperatura controla a aleatoriedade (frequentemente chamada de "criatividade") das respostas do modelo. Como as IAs funcionam prevendo a próxima palavra, a temperatura define se a IA deve escolher a palavra estatisticamente mais óbvia ou se ela pode "arriscar" opções menos prováveis para soar mais fluida e criativa.

* **Temperatura Alta (ex: 0.8 a 1.0):** Excelente para escrever poesias, criar rascunhos de marketing, sugerir ideias de temas de redação. O modelo fica "livre" para ser criativo.  
* **Temperatura Zero (0.0):** O modelo torna-se estritamente determinístico. Ele escolherá o caminho lógico mais seguro e fundamentado repetidas vezes.

**Se liga:** Ao usar a IA para cruzar o enunciado de uma questão com a bibliografia do edital ou para auditar a precisão técnica de um gabarito, **a Temperatura deve estar sempre em 0 (zero)**. Nós não queremos que a IA seja "criativa" ao interpretar o texto-base. Nós queremos um julgamento técnico, rigoroso, previsível e calcado estritamente no material fornecido.

