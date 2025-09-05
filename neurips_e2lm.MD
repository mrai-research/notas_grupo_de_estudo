# NeurIPs 2025 -  Competição E2LM

Esse grupo de estudo tem como objetivo participar de forma competitiva da competição [Early Training Evaluation of Language Models (E2LM)](https://e2lmc.github.io/) buscando aprender sobre a estruturação e teoria da avaliação de modelos de linguagem.

## Descrição do Problema

Essa competição tem como desafio criar um benchmark que seja capaz de inferir a capacidade de um modelo de linguagem em seus estágios iniciais de treinamento. Para isso são disponibilizados 3 modelos em três tamanhos differentes (0.5B, 1B e 3B), com duas arquiteturas e 27 checkpoints cada. A implementação deve ser por meio de uma criação de task na lib *lm-evaluation-harness* (ver notebook 4 do tutorial).

## Materiais Úteis
- [Paper](https://arxiv.org/abs/2506.07731)
- [Notebooks Iniciais](https://e2lmc.github.io/starter_kit)
- [Repositório de Desenvolvimento](https://github.com/Team-mrai/el2m-neurips)
- [Lib lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness/tree/main)


## Registro de Conteúdos:

### O que é a submissão

A ideia da submissão é: atualizar o pacote [lm-evaluation-harness-competition](https://github.com/tiiuae/lm-evaluation-harness-competition) adicionando uma nova tarefa (sua proposta).
A sua submissão pode incluir um novo dataset (não necessariamente), porém esse dataset não será submetido. Você deve fazer upload desse dataset no [HuggingFace](https://huggingface.co/new-dataset).

A submissão é um arquivo zip chamado `submission.zip` que contém dois arquivos:
- `evaluation.patch`: é um arquivo que mostra a diferença entre o seu repositório e o repositório original do `lm-evaluation-harness-competition`.
- `metadata.yaml`: um arquivo contendo uma breve descrição da sua nova tarefa:
	- `task_name`: nome da task (apenas uma descrição)
	- `task_metric`: métrica da task, deve estar implementada
	- `hf_token`: token do HF que dá permissão de leitura para o dataset criado

### O que podemos alterar em uma "task"

#### Dataset

Podemos selecionar um dataset já existente ou criar um dataset de perguntas e respostas. Esse dataset deve conter textos que são passados como entrada para o modelo, e outros textos que são esperados como saída. Por exemplo, um dataset pode conter o registro:

```
{"question" : "Quando é 1+1?", "answer" : "2"}
```


#### Prompt engineering

O dataset é composto por variáveis com valores textuais, por exemplo, um dataset de múltipla escolha pode conter a variável "question" e as possíveis respostas "choice0", "choice1", "choice2", "choice3", "choice4". Você pode fazer um prompt engineering para reescrever a entrada do modelo baseado nas variáveis presentes no seu dataset. Por exemplo:

```jinja2
"Question: {{question}}\nA) {{choice0}}\nB) {{choice1}}\nC) {{choice2}}\nD) {{choice3}}\nE) {{choice4}}\nAnswer:"
```

O prompt engineering pode ser feito usando o Jinja2 ou usando funções python.

```python
def wikitext_detokenizer(doc):
    string = doc["page"]
    # contractions
    string = string.replace("s '", "s'")
    string = re.sub(r"/' [0-9]/", r"/'[0-9]/", string)
    ...
    string = string.replace(" 's", "'s")

    return string
```

#### Tipo de geração

Devemos calcular as métricas baseadas nas palavras com maior probabilidade? Ou usando o valor da probabilidade?

Podemos alterar o que é interpretado como "saída do modelo". 

- `generate_until`: gera palavras sequencialmente (com temperatura 0 ou não) até encontrar um "END OF SEQUENCE" ou um tamanho máximo. Retorna essas palavras.
- `loglikelihood`: retorna a probabilidade de gerar a palavra target condicionado ao texto de entrada. Também retorna se o target tem a maior probabilidade entre todas palavras.
- `loglikelihood_rolling`: retorna a probabilidade de gerar **o input**. Isso mesmo, nesse tipo de geração não importa o output. Você quer saber o quão provável é o modelo gerar o texto de entrada. Isso é usado para calcular perplexity.
- `multiple_choice`: entre as "alternativas" possíveis, retorna qual delas tem a maior probabilidade pelo modelo. Por exemplo, se existem 5 alternativas representadas pelas strings "A", "B", "C", "D", "E", e o seu modelo diz que a palavra de maior probabilidade é "gato", a predição não vai ser considerada essa, e sim vai ser a string que tem a maior probabilidade entre as 5 opções.

Implementação da geração de output: [huggingface.py](https://github.com/EleutherAI/lm-evaluation-harness/blob/3bc7cc8a72c66bac8d5b830cb3ccec9a5f691b12/lm_eval/models/huggingface.py#L1357)

#### Métrica

Uma métrica é uma função python que recebe duas listas de textos. Uma lista é a de "documentos" e a outra é a de "predições". A métrica deve então verificar se cada predição é a adequada para cada documento. Métricas e tipos de geração podem se confundir um pouco. Algumas métricas implementadas na biblioteca:

Implementação das métricas: [metrics.py](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/lm_eval/api/metrics.py)

#### Filtro

Ainda não entendi...

## Registro das Reuniões:

* 08/08:

    * Discussões:
        1. Possibilidade de modelagem do problema de forma a ser multi-objetivo já que são 3 tipos de métricas diferentes que são contabilizadas por cada benchmark, importante tentar entender como otimizar cada uma
        2. É necessário entender as limitações e possibilidades de personalização da lib. Ela sempre vai rodar a política de pegar o token de maior probabilidade? É possível criar uma métrica customizável? É possível orientar um prompt diferente?
        3. Pode ser interessante fazer uma avaliação robusta de benchmarks já existentes para entender como eles performam
        4. Uma ideia de contribuição seria fazer filtros, aumentaçoes e curadorias de benchmarks já existentes de forma a bsucar e entender os elementos relvantes apra o treinamento e generalização *posteriori* do modelo.

* 05/09

	* Discussões:
		1. Ele não aprende sociologia
		2. Respostas longas são altamente punidas
		3. Modelos localmente em hadataset ```morai```
		4. Script ```submission.sh``` para gerar automaticamente o zip para entrega
		5. Problemas em rodar outra versão do dataset
		5. **Próximos passos**: 
	* Atribuições:

    * Atribuições:
        1. **Estudo de métricas e experimentações (Daniel e Bea)**: A ideia é entender qual é a proposta dos cálculos de métricas e como podemos investigar e focar nisso. A métrica é composta por uma soma ponderada de três componentes que devem ser 
        2. **Registro de Equipe (Giovanni)**: Entender como registrar a equipe e o que é necessário para participar e iniciar as submissões
        3. **Repo Inicial (Caio)**: Criar um repositório inicial com scripts que facilitem as submissões e implementação de benchmarks na lib apontada
        4. **Rodas os notebooks iniciais (quem quiser)**: Rodar e entender a pipeline proposta nos ntoebooks
