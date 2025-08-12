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

## Registro das Reuniões:

* 08/08:

    * Discussões:
        1. Possibilidade de modelagem do problema de forma a ser multi-objetivo já que são 3 tipos de métricas diferentes que são contabilizadas por cada benchmark, importante tentar entender como otimizar cada uma
        2. É necessário entender as limitações e possibilidades de personalização da lib. Ela sempre vai rodar a política de pegar o token de maior probabilidade? É possível criar uma métrica customizável? É possível orientar um prompt diferente?
        3. Pode ser interessante fazer uma avaliação robusta de benchmarks já existentes para entender como eles performam
        4. Uma ideia de contribuição seria fazer filtros, aumentaçoes e curadorias de benchmarks já existentes de forma a bsucar e entender os elementos relvantes apra o treinamento e generalização *posteriori* do modelo.

    * Atribuições:
        1. **Estudo de métricas e experimentações (Daniel e Bea)**: A ideia é entender qual é a proposta dos cálculos de métricas e como podemos investigar e focar nisso. A métrica é composta por uma soma ponderada de três componentes que devem ser 
        2. **Registro de Equipe (Giovanni)**: Entender como registrar a equipe e o que é necessário para participar e iniciar as submissões
        3. **Repo Inicial (Caio)**: Criar um repositório inicial com scripts que facilitem as submissões e implementação de benchmarks na lib apontada
        4. **Rodas os notebooks iniciais (quem quiser)**: Rodar e entender a pipeline proposta nos ntoebooks
