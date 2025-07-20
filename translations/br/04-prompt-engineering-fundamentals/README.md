<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "a45c318dc6ebc2604f35b8b829f93af2",
  "translation_date": "2025-05-19T09:39:40+00:00",
  "source_file": "04-prompt-engineering-fundamentals/README.md",
  "language_code": "br"
}
-->
# Fundamentos da Engenharia de Prompt

## Introdução
Este módulo aborda conceitos e técnicas essenciais para criar prompts eficazes em modelos de IA generativa. A forma como você escreve seu prompt para um LLM também importa. Um prompt bem elaborado pode obter uma resposta de melhor qualidade. Mas o que exatamente significam termos como _prompt_ e _engenharia de prompt_? E como posso melhorar o _input_ do prompt que envio para o LLM? Estas são as perguntas que tentaremos responder neste capítulo e no próximo.

A _IA generativa_ é capaz de criar novo conteúdo (por exemplo, texto, imagens, áudio, código etc.) em resposta a solicitações dos usuários. Ela consegue isso usando _Modelos de Linguagem de Grande Porte_ como a série GPT ("Generative Pre-trained Transformer") da OpenAI, que são treinados para usar linguagem natural e código.

Os usuários agora podem interagir com esses modelos usando paradigmas familiares, como chat, sem precisar de qualquer expertise técnica ou treinamento. Os modelos são baseados em _prompts_ - os usuários enviam um texto de entrada (prompt) e recebem de volta a resposta da IA (completação). Eles podem então "conversar com a IA" iterativamente, em conversas de múltiplas etapas, refinando seu prompt até que a resposta atenda suas expectativas.

Os "prompts" agora se tornam a principal _interface de programação_ para aplicativos de IA generativa, dizendo aos modelos o que fazer e influenciando a qualidade das respostas retornadas. A "Engenharia de Prompt" é um campo de estudo em rápido crescimento que se concentra no _design e otimização_ de prompts para entregar respostas consistentes e de qualidade em escala.

## Objetivos de Aprendizagem

Nesta lição, aprenderemos o que é Engenharia de Prompt, por que ela importa e como podemos criar prompts mais eficazes para um determinado modelo e objetivo de aplicação. Vamos entender os conceitos principais e as melhores práticas para engenharia de prompt - e aprender sobre um ambiente interativo de "sandbox" em Jupyter Notebooks onde podemos ver esses conceitos aplicados a exemplos reais.

Ao final desta lição, seremos capazes de:

1. Explicar o que é engenharia de prompt e por que ela importa.
2. Descrever os componentes de um prompt e como eles são usados.
3. Aprender as melhores práticas e técnicas para engenharia de prompt.
4. Aplicar as técnicas aprendidas a exemplos reais, usando um endpoint da OpenAI.

## Termos-Chave

Engenharia de Prompt: A prática de projetar e refinar entradas para guiar modelos de IA na produção de saídas desejadas.
Tokenização: O processo de converter texto em unidades menores, chamadas tokens, que um modelo pode entender e processar.
LLMs Ajustados por Instrução: Modelos de Linguagem de Grande Porte (LLMs) que foram ajustados com instruções específicas para melhorar a precisão e relevância de suas respostas.

## Sandbox de Aprendizagem

A engenharia de prompt é atualmente mais arte do que ciência. A melhor maneira de melhorar nossa intuição para isso é _praticar mais_ e adotar uma abordagem de tentativa e erro que combine expertise no domínio da aplicação com técnicas recomendadas e otimizações específicas do modelo.

O Jupyter Notebook que acompanha esta lição fornece um ambiente de _sandbox_ onde você pode experimentar o que aprende - conforme avança ou como parte do desafio de código no final. Para executar os exercícios, você precisará de:

1. **Uma chave de API da Azure OpenAI** - o endpoint de serviço para um LLM implantado.
2. **Um Runtime Python** - no qual o Notebook pode ser executado.
3. **Variáveis de Ambiente Locais** - _complete agora as etapas de [CONFIGURAÇÃO](./../00-course-setup/SETUP.md?WT.mc_id=academic-105485-koreyst) para se preparar_.

O notebook vem com exercícios _iniciais_ - mas você é incentivado a adicionar suas próprias seções de _Markdown_ (descrição) e _Código_ (solicitações de prompt) para tentar mais exemplos ou ideias - e construir sua intuição para design de prompt.

## Guia Ilustrado

Quer ter uma visão geral do que esta lição cobre antes de mergulhar? Confira este guia ilustrado, que dá uma ideia dos principais tópicos abordados e os principais pontos para você refletir em cada um. O roteiro da lição leva você desde a compreensão dos conceitos principais e desafios até abordá-los com técnicas de engenharia de prompt relevantes e melhores práticas. Observe que a seção "Técnicas Avançadas" neste guia refere-se ao conteúdo abordado no _próximo_ capítulo deste currículo.

## Nossa Startup

Agora, vamos falar sobre como _este tópico_ se relaciona com nossa missão de startup de [trazer inovação em IA para a educação](https://educationblog.microsoft.com/2023/06/collaborating-to-bring-ai-innovation-to-education?WT.mc_id=academic-105485-koreyst). Queremos construir aplicativos de aprendizado personalizado movidos por IA - então vamos pensar sobre como diferentes usuários de nosso aplicativo podem "projetar" prompts:

- **Administradores** podem pedir à IA para _analisar dados do currículo para identificar lacunas na cobertura_. A IA pode resumir os resultados ou visualizá-los com código.
- **Educadores** podem pedir à IA para _gerar um plano de aula para um público-alvo e tópico específico_. A IA pode construir o plano personalizado em um formato especificado.
- **Estudantes** podem pedir à IA para _tutoriá-los em um assunto difícil_. A IA agora pode guiar os estudantes com lições, dicas e exemplos adaptados ao seu nível.

Isso é apenas a ponta do iceberg. Confira [Prompts para Educação](https://github.com/microsoft/prompts-for-edu/tree/main?WT.mc_id=academic-105485-koreyst) - uma biblioteca de prompts de código aberto curada por especialistas em educação - para ter uma noção mais ampla das possibilidades! _Experimente executar alguns desses prompts no sandbox ou usar o Playground da OpenAI para ver o que acontece!_

## O que é Engenharia de Prompt?

Começamos esta lição definindo **Engenharia de Prompt** como o processo de _projetar e otimizar_ entradas de texto (prompts) para entregar respostas consistentes e de qualidade (completions) para um determinado objetivo de aplicação e modelo. Podemos pensar nisso como um processo de 2 etapas:

- _projetar_ o prompt inicial para um determinado modelo e objetivo
- _refinar_ o prompt iterativamente para melhorar a qualidade da resposta

Este é necessariamente um processo de tentativa e erro que requer intuição e esforço do usuário para obter resultados ótimos. Então por que isso é importante? Para responder a essa pergunta, primeiro precisamos entender três conceitos:

- _Tokenização_ = como o modelo "vê" o prompt
- _LLMs Base_ = como o modelo base "processa" um prompt
- _LLMs Ajustados por Instrução_ = como o modelo pode agora ver "tarefas"

### Tokenização

Um LLM vê prompts como uma _sequência de tokens_ onde diferentes modelos (ou versões de um modelo) podem tokenizar o mesmo prompt de maneiras diferentes. Como os LLMs são treinados em tokens (e não em texto bruto), a maneira como os prompts são tokenizados tem um impacto direto na qualidade da resposta gerada.

Para obter uma intuição de como a tokenização funciona, experimente ferramentas como o [Tokenizador da OpenAI](https://platform.openai.com/tokenizer?WT.mc_id=academic-105485-koreyst) mostrado abaixo. Copie seu prompt - e veja como ele é convertido em tokens, prestando atenção em como os caracteres de espaço em branco e marcas de pontuação são tratados. Observe que este exemplo mostra um LLM mais antigo (GPT-3) - então tentar isso com um modelo mais recente pode produzir um resultado diferente.

### Conceito: Modelos Fundamentais

Uma vez que um prompt é tokenizado, a função principal do ["LLM Base"](https://blog.gopenai.com/an-introduction-to-base-and-instruction-tuned-large-language-models-8de102c785a6?WT.mc_id=academic-105485-koreyst) (ou modelo fundamental) é prever o token naquela sequência. Como os LLMs são treinados em conjuntos de dados de texto massivos, eles têm uma boa noção das relações estatísticas entre tokens e podem fazer essa previsão com alguma confiança. Observe que eles não entendem o _significado_ das palavras no prompt ou token; eles apenas veem um padrão que podem "completar" com sua próxima previsão. Eles podem continuar prevendo a sequência até serem interrompidos por intervenção do usuário ou alguma condição pré-estabelecida.

Quer ver como a conclusão baseada em prompt funciona? Insira o prompt acima no [_Playground de Chat_](https://oai.azure.com/playground?WT.mc_id=academic-105485-koreyst) do Azure OpenAI com as configurações padrão. O sistema está configurado para tratar prompts como solicitações de informações - então você deve ver uma conclusão que satisfaça esse contexto.

Mas e se o usuário quiser ver algo específico que atenda a alguns critérios ou objetivo de tarefa? É aqui que os LLMs _ajustados por instrução_ entram em cena.

### Conceito: LLMs Ajustados por Instrução

Um [LLM Ajustado por Instrução](https://blog.gopenai.com/an-introduction-to-base-and-instruction-tuned-large-language-models-8de102c785a6?WT.mc_id=academic-105485-koreyst) começa com o modelo fundamental e o ajusta com exemplos ou pares de entrada/saída (por exemplo, "mensagens" de múltiplas etapas) que podem conter instruções claras - e a resposta da IA tenta seguir essa instrução.

Isso usa técnicas como Aprendizado por Reforço com Feedback Humano (RLHF) que podem treinar o modelo para _seguir instruções_ e _aprender com feedback_ para que ele produza respostas mais adequadas a aplicações práticas e mais relevantes aos objetivos do usuário.

Vamos experimentar - revise o prompt acima, mas agora altere a _mensagem do sistema_ para fornecer a seguinte instrução como contexto:

> _Resuma o conteúdo que você recebeu para um aluno da segunda série. Mantenha o resultado em um parágrafo com 3-5 pontos._

Veja como o resultado agora é ajustado para refletir o objetivo e formato desejados? Um educador agora pode usar diretamente essa resposta em seus slides para aquela turma.

## Por que precisamos de Engenharia de Prompt?

Agora que sabemos como os prompts são processados pelos LLMs, vamos falar sobre _por que_ precisamos de engenharia de prompt. A resposta está no fato de que os LLMs atuais apresentam uma série de desafios que tornam _conclusões confiáveis e consistentes_ mais difíceis de alcançar sem esforço na construção e otimização de prompts. Por exemplo:

1. **As respostas dos modelos são estocásticas.** O _mesmo prompt_ provavelmente produzirá respostas diferentes com diferentes modelos ou versões de modelos. E pode até produzir resultados diferentes com o _mesmo modelo_ em momentos diferentes. _Técnicas de engenharia de prompt podem nos ajudar a minimizar essas variações fornecendo melhores diretrizes_.

2. **Os modelos podem fabricar respostas.** Os modelos são pré-treinados com _conjuntos de dados grandes, mas finitos_, o que significa que eles não têm conhecimento sobre conceitos fora desse escopo de treinamento. Como resultado, eles podem produzir conclusões que são imprecisas, imaginárias ou diretamente contraditórias a fatos conhecidos. _Técnicas de engenharia de prompt ajudam os usuários a identificar e mitigar essas fabricações, por exemplo, pedindo citações ou raciocínio à IA_.

3. **As capacidades dos modelos variam.** Modelos mais novos ou gerações de modelos terão capacidades mais ricas, mas também trazem peculiaridades únicas e trade-offs em custo e complexidade. _A engenharia de prompt pode nos ajudar a desenvolver melhores práticas e fluxos de trabalho que abstraem diferenças e se adaptam a requisitos específicos de modelos de maneiras escaláveis e contínuas_.

Vamos ver isso em ação no Playground da OpenAI ou Azure OpenAI:

- Use o mesmo prompt com diferentes implantações de LLM (por exemplo, OpenAI, Azure OpenAI, Hugging Face) - você viu as variações?
- Use o mesmo prompt repetidamente com a _mesma_ implantação de LLM (por exemplo, playground do Azure OpenAI) - como essas variações diferiram?

### Exemplo de Fabricações

Neste curso, usamos o termo **"fabricação"** para referenciar o fenômeno em que LLMs às vezes geram informações factualmente incorretas devido a limitações em seu treinamento ou outras restrições. Você também pode ter ouvido isso referido como _"alucinações"_ em artigos populares ou trabalhos de pesquisa. No entanto, recomendamos fortemente o uso de _"fabricação"_ como o termo para que não antropomorfizemos acidentalmente o comportamento atribuindo uma característica humana a um resultado movido por máquina. Isso também reforça as [diretrizes de IA responsável](https://www.microsoft.com/ai/responsible-ai?WT.mc_id=academic-105485-koreyst) do ponto de vista da terminologia, removendo termos que também podem ser considerados ofensivos ou não inclusivos em alguns contextos.

Quer ter uma ideia de como as fabricações funcionam? Pense em um prompt que instrua a IA a gerar conteúdo para um tópico inexistente (para garantir que não seja encontrado no conjunto de dados de treinamento). Por exemplo - eu tentei este prompt:

> **Prompt:** gerar um plano de aula sobre a Guerra Marciana de 2076.

Uma pesquisa na web me mostrou que havia relatos fictícios (por exemplo, séries de televisão ou livros) sobre guerras marcianas - mas nenhum em 2076. O senso comum também nos diz que 2076 está _no futuro_ e, portanto, não pode estar associado a um evento real.

Então, o que acontece quando executamos este prompt com diferentes provedores de LLM?

Como esperado, cada modelo (ou versão de modelo) produz respostas ligeiramente diferentes graças ao comportamento estocástico e variações de capacidade do modelo. Por exemplo, um modelo direciona um público de 8ª série, enquanto o outro assume um estudante do ensino médio. Mas todos os três modelos geraram respostas que poderiam convencer um usuário desinformado de que o evento era real.

Técnicas de engenharia de prompt como _metaprompting_ e _configuração de temperatura_ podem reduzir fabricações de modelos até certo ponto. Novas _arquiteturas_ de engenharia de prompt também incorporam novas ferramentas e técnicas de forma contínua no fluxo de prompt, para mitigar ou reduzir alguns desses efeitos.

## Estudo de Caso: GitHub Copilot

Vamos encerrar esta seção obtendo uma noção de como a engenharia de prompt é usada em soluções do mundo real, analisando um Estudo de Caso: [GitHub Copilot](https://github.com/features/copilot?WT.mc_id=academic-105485-koreyst).

GitHub Copilot é seu "Programador Pareado por IA" - ele converte prompts de texto em conclusões de código e é integrado ao seu ambiente de desenvolvimento (por exemplo, Visual Studio Code) para uma experiência de usuário contínua. Conforme documentado na série de blogs abaixo, a versão mais antiga foi baseada no modelo Codex da OpenAI - com engenheiros rapidamente percebendo a necessidade de ajustar o modelo e desenvolver melhores técnicas de engenharia de prompt, para melhorar a qualidade do código. Em julho, eles [estrearam um modelo de IA aprimorado que vai além do Codex](https://github.blog/2023-07-28-smarter-more-efficient-coding-github-copilot-goes-beyond-codex-with-improved-ai-model/?WT.mc_id=academic-105485-koreyst) para sugestões ainda mais rápidas.

Leia os posts em ordem, para seguir sua jornada de aprendizado.

Você também pode navegar pelo [blog de Engenharia](https://github.blog/category/engineering/?WT.mc_id=academic-105485-koreyst) para mais posts como [este](https://github.blog/2023-09-27-how-i-used-github-copilot-chat-to-build-a-reactjs-gallery-prototype/?WT.mc_id=academic-105485-koreyst) que mostra como esses modelos e técnicas são _aplicados_ para impulsionar aplicações do mundo real.

## Construção de Prompt

Vimos por que a engenharia de prompt é importante - agora vamos entender como os prompts são _construídos_ para que possamos avaliar diferentes técnicas para um design de prompt mais eficaz.

### Prompt Básico

Vamos começar com o prompt básico: uma entrada de texto enviada ao modelo sem outro contexto. Aqui está um exemplo - quando enviamos as primeiras palavras do hino nacional dos EUA para a [API de Completação](https://platform.openai.com/docs/api-reference/completions?WT.mc_id=academic-105485-koreyst) da OpenAI, ela instantaneamente _completa_ a resposta com as próximas linhas, ilustrando o comportamento básico de previsão.

### Prompt Complexo

Agora vamos adicionar contexto e instruções a esse prompt básico. A [API de Completação de Chat](https://learn.microsoft.com/azure/ai-services/openai/how-to/chatgpt?WT.mc_id=academic-105485-koreyst) nos permite construir um prompt complexo como uma coleção de _mensagens_ com:

- Pares de entrada/saída refletindo a entrada do _usuário_ e a resposta do _assistente_.
- Mensagem do sistema definindo o contexto para o comportamento ou personalidade do assistente.

A solicitação agora está no formato abaixo, onde a _tokenização_ captura efetivamente informações relevantes do contexto e da conversa. Agora, alterar o contexto do sistema pode ser tão impactante na qualidade das conclusões quanto as entradas fornecidas pelo usuário.

### Prompt de Instrução

Nos exemplos acima, o prompt do usuário era uma simples consulta de texto que pode ser interpretada como uma solicitação de informações. Com _prompts de instrução_, podemos usar esse texto para especificar uma tarefa em mais detalhes, fornecendo uma melhor orientação à IA. Aqui está um exemplo:

### Conteúdo Principal

Nos exemplos acima, o prompt ainda era bastante aberto, permitindo que o LLM decidisse qual parte de seu conjunto de dados pré-treinado era relevante. Com o padrão de design de _conteúdo principal_, o texto de entrada é dividido em duas partes:

- uma instrução (ação)
- conteúdo relevante (que influencia a ação)

Aqui está um exemplo onde a instrução é "resuma isso em 2 frases".

O segmento de conteúdo principal pode ser usado de várias maneiras para impulsionar instruções mais eficazes:

- **Exemplos** - em vez de dizer ao modelo o que fazer com uma instrução explícita, dê-lhe exemplos do que fazer e deixe-o inferir o padrão.
- **Pistas** - siga a instrução com uma "pista" que prepare a conclusão, guiando o modelo para respostas mais relevantes.
- **Modelos** - são 'receitas' repetíveis para prompts com espaços reservados (variáveis) que podem ser personalizadas com dados para casos de uso específicos.


Finalmente, o verdadeiro valor dos modelos está na capacidade de criar e publicar _bibliotecas de prompts_ para domínios de aplicação vertical - onde o modelo de prompt agora está _otimizado_ para refletir o contexto ou exemplos específicos da aplicação que tornam as respostas mais relevantes e precisas para o público-alvo. O repositório [Prompts para Educação](https://github.com/microsoft/prompts-for-edu?WT.mc_id=academic-105485-koreyst) é um ótimo exemplo dessa abordagem, curando uma biblioteca de prompts para o domínio da educação com ênfase em objetivos-chave como planejamento de aulas, design curricular, tutoria de estudantes, etc.

## Conteúdo de Apoio

Se pensarmos na construção de prompts como tendo uma instrução (tarefa) e um alvo (conteúdo principal), então o _conteúdo secundário_ é como um contexto adicional que fornecemos para **influenciar a saída de alguma forma**. Pode ser ajuste de parâmetros, instruções de formatação, taxonomias de tópicos, etc., que podem ajudar o modelo a _adaptar_ sua resposta para atender aos objetivos ou expectativas desejadas do usuário.

Por exemplo: Dado um catálogo de cursos com metadados extensivos (nome, descrição, nível, tags de metadados, instrutor, etc.) sobre todos os cursos disponíveis no currículo:

- podemos definir uma instrução para "resumir o catálogo de cursos para o outono de 2023"
- podemos usar o conteúdo principal para fornecer alguns exemplos da saída desejada
- podemos usar o conteúdo secundário para identificar as 5 principais "tags" de interesse.

Agora, o modelo pode fornecer um resumo no formato mostrado pelos poucos exemplos - mas se um resultado tiver várias tags, ele pode priorizar as 5 tags identificadas no conteúdo secundário.

---

## Melhores Práticas de Prompting

Agora que sabemos como os prompts podem ser _construídos_, podemos começar a pensar em como _desenhá-los_ para refletir as melhores práticas. Podemos pensar nisso em duas partes - ter a _mentalidade_ certa e aplicar as _técnicas_ corretas.

### Mentalidade de Engenharia de Prompts

A Engenharia de Prompts é um processo de tentativa e erro, então mantenha três amplos fatores orientadores em mente:

1. **Entendimento do Domínio Importa.** A precisão e relevância da resposta é uma função do _domínio_ em que essa aplicação ou usuário opera. Aplique sua intuição e expertise no domínio para **personalizar técnicas** ainda mais. Por exemplo, defina _personalidades específicas do domínio_ em seus prompts de sistema, ou use _modelos específicos do domínio_ em seus prompts de usuário. Forneça conteúdo secundário que reflita contextos específicos do domínio, ou use _pistas e exemplos específicos do domínio_ para guiar o modelo em direção a padrões de uso familiares.

2. **Entendimento do Modelo Importa.** Sabemos que os modelos são estocásticos por natureza. Mas as implementações de modelo também podem variar em termos do conjunto de dados de treinamento que usam (conhecimento pré-treinado), as capacidades que fornecem (por exemplo, via API ou SDK) e o tipo de conteúdo para o qual são otimizados (por exemplo, código vs. imagens vs. texto). Entenda os pontos fortes e limitações do modelo que você está usando e use esse conhecimento para _priorizar tarefas_ ou construir _modelos personalizados_ que sejam otimizados para as capacidades do modelo.

3. **Iteração & Validação Importam.** Os modelos estão evoluindo rapidamente, assim como as técnicas de engenharia de prompts. Como especialista no domínio, você pode ter outro contexto ou critérios para _sua_ aplicação específica, que podem não se aplicar à comunidade em geral. Use ferramentas e técnicas de engenharia de prompts para "dar um pontapé inicial" na construção de prompts, depois itere e valide os resultados usando sua própria intuição e expertise no domínio. Registre suas percepções e crie uma **base de conhecimento** (por exemplo, bibliotecas de prompts) que pode ser usada como um novo ponto de partida por outros, para iterações mais rápidas no futuro.

## Melhores Práticas

Agora vamos examinar as práticas recomendadas comuns que são recomendadas por praticantes da [OpenAI](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api?WT.mc_id=academic-105485-koreyst) e [Azure OpenAI](https://learn.microsoft.com/azure/ai-services/openai/concepts/prompt-engineering#best-practices?WT.mc_id=academic-105485-koreyst).

| O que                             | Por que                                                                                                                                                                                                                                             |
| :-------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Avalie os modelos mais recentes.  | Novas gerações de modelos provavelmente têm recursos e qualidade aprimorados - mas também podem incorrer em custos mais altos. Avalie-os para impacto, depois tome decisões de migração.                                                             |
| Separe instruções e contexto      | Verifique se seu modelo/fornecedor define _delimitadores_ para distinguir instruções, conteúdo principal e secundário de forma mais clara. Isso pode ajudar os modelos a atribuir pesos mais precisos aos tokens.                                      |
| Seja específico e claro           | Dê mais detalhes sobre o contexto desejado, resultado, comprimento, formato, estilo, etc. Isso melhorará tanto a qualidade quanto a consistência das respostas. Capture receitas em modelos reutilizáveis.                                            |
| Seja descritivo, use exemplos     | Os modelos podem responder melhor a uma abordagem de "mostrar e contar". Comece com um `zero-shot` approach where you give it an instruction (but no examples) then try `few-shot` as a refinement, providing a few examples of the desired output. Use analogies. |
| Use cues to jumpstart completions | Nudge it towards a desired outcome by giving it some leading words or phrases that it can use as a starting point for the response.                                                                                                               |
| Double Down                       | Sometimes you may need to repeat yourself to the model. Give instructions before and after your primary content, use an instruction and a cue, etc. Iterate & validate to see what works.                                                         |
| Order Matters                     | The order in which you present information to the model may impact the output, even in the learning examples, thanks to recency bias. Try different options to see what works best.                                                               |
| Give the model an “out”           | Give the model a _fallback_ completion response it can provide if it cannot complete the task for any reason. This can reduce chances of models generating false or fabricated responses.                                                         |
|                                   |                                                                                                                                                                                                                                                   |

As with any best practice, remember that _your mileage may vary_ based on the model, the task and the domain. Use these as a starting point, and iterate to find what works best for you. Constantly re-evaluate your prompt engineering process as new models and tools become available, with a focus on process scalability and response quality.

<!--
LESSON TEMPLATE:
This unit should provide a code challenge if applicable

CHALLENGE:
Link to a Jupyter Notebook with only the code comments in the instructions (code sections are empty).

SOLUTION:
Link to a copy of that Notebook with the prompts filled in and run, showing what one example could be.
-->

## Assignment

Congratulations! You made it to the end of the lesson! It's time to put some of those concepts and techniques to the test with real examples!

For our assignment, we'll be using a Jupyter Notebook with exercises you can complete interactively. You can also extend the Notebook with your own Markdown and Code cells to explore ideas and techniques on your own.

### To get started, fork the repo, then

- (Recommended) Launch GitHub Codespaces
- (Alternatively) Clone the repo to your local device and use it with Docker Desktop
- (Alternatively) Open the Notebook with your preferred Notebook runtime environment.

### Next, configure your environment variables

- Copy the `.env.copy` file in repo root to `.env` and fill in the `AZURE_OPENAI_API_KEY`, `AZURE_OPENAI_ENDPOINT` and `AZURE_OPENAI_DEPLOYMENT` valores. Volte para a [seção de Sandbox de Aprendizado](../../../04-prompt-engineering-fundamentals/04-prompt-engineering-fundamentals) para aprender como.

### Em seguida, abra o Jupyter Notebook

- Selecione o kernel de tempo de execução. Se estiver usando as opções 1 ou 2, basta selecionar o kernel padrão Python 3.10.x fornecido pelo contêiner de desenvolvimento.

Você está pronto para executar os exercícios. Observe que não há respostas _certas e erradas_ aqui - apenas explorando opções por tentativa e erro e construindo intuição sobre o que funciona para um determinado modelo e domínio de aplicação.

_Por essa razão, não há segmentos de Solução de Código nesta lição. Em vez disso, o Notebook terá células de Markdown intituladas "Minha Solução:" que mostram um exemplo de saída para referência._

## Verificação de Conhecimento

Qual das opções a seguir é um bom prompt seguindo algumas práticas recomendadas razoáveis?

1. Mostre-me uma imagem de carro vermelho
2. Mostre-me uma imagem de carro vermelho da marca Volvo e modelo XC90 estacionado à beira de um penhasco com o sol se pondo
3. Mostre-me uma imagem de carro vermelho da marca Volvo e modelo XC90

A: 2, é o melhor prompt, pois fornece detalhes sobre "o que" e vai ao específico (não apenas qualquer carro, mas uma marca e modelo específicos) e também descreve o cenário geral. 3 é o próximo melhor, pois também contém muita descrição.

## 🚀 Desafio

Veja se você consegue aproveitar a técnica de "pista" com o prompt: Complete a frase "Mostre-me uma imagem de carro vermelho da marca Volvo e ". Com o que ele responde e como você melhoraria isso?

## Ótimo Trabalho! Continue Seu Aprendizado

Quer aprender mais sobre diferentes conceitos de Engenharia de Prompts? Vá para a [página de aprendizado contínuo](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst) para encontrar outros ótimos recursos sobre este tópico.

Vá para a Lição 5, onde veremos [técnicas avançadas de prompting](../05-advanced-prompts/README.md?WT.mc_id=academic-105485-koreyst)!

**Aviso Legal**:  
Este documento foi traduzido usando o serviço de tradução por IA [Co-op Translator](https://github.com/Azure/co-op-translator). Embora nos esforcemos para garantir a precisão, esteja ciente de que traduções automáticas podem conter erros ou imprecisões. O documento original em seu idioma nativo deve ser considerado a fonte autoritativa. Para informações críticas, recomenda-se a tradução profissional humana. Não nos responsabilizamos por quaisquer mal-entendidos ou interpretações errôneas decorrentes do uso desta tradução.
