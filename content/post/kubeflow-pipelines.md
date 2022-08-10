---
title: "Kubeflow Pipelines"
date: 2022-08-08T10:12:56-03:00
draft: true
tags:
- machine learning
- mlops
categories:
- mlops
- machine learning pipeline
---

![Kubeflow Logo](kubeflow-pipelines/kubeflow-logo.png)

Para começar a falar sobre `pipeline` primeiro precisamos entender um pouco sobre o que é `machine learning` e vamos fazer isso respondendo uma simples pergunta: **O que é Machine Learning?**

Segundo o site da HP:

> <cite>"O machine learning (ML) é uma subcategoria de inteligência artificial que se refere ao processo pelo qual os computadores desenvolvem o reconhecimento de padrões ou a capacidade de aprender continuamente ou fazer previsões com base em dados, e então, fazer ajustes sem serem especificamente programados para isso."[^1]</cite>

[^1]: [https://www.hpe.com/br/pt/what-is/machine-learning.html](https://www.hpe.com/br/pt/what-is/machine-learning.html)

Eu não sou da área de estatística ou de dados, então vou resumir ML como sendo um processo complexo que muitos estão começando a utilizar, mas que poucos sabem realmente como funcionam a fundo. 😬

##### 😱 Mas se ML é tão complexo, como podemos criar um pipeline que seja simples?

Calma! Os conceitos por trás do ML são complexos, mas o ferramental atual abstrae grande parte dessa complexidade tornando tudo muito mais fácil para quem não tem o `backgroud` nessa área (como eu 😅).

> **🚨 Observação Importante:** Como o foco desse post não é o ML em si, essas abstrações são suficientes, mas caso você tenha interesse na área de dados, recomendo se aprofundar mais porque todo mundo deveria conhecer e entender as ferramentas e algoritmos que utilizam no dia a dia de trabalho.

## Um exemplo simples

Para exemplificar a criação de um pipeline, primeiro precisamos criar um código de ML. Vou assumir que, se você chegou até aqui, já possui alguns conhecimentos sobre python.

### Conjunto de Dados

Em todo site que você pesquisar, vai encontrar exemplos utilizando o `dataset` (conjunto de dados) intitulado `iris`. Esse `dataset` possui informações sobre a flor iris contendo o comprimento e a largura das sépalas e pétalas, e a espécie pertencente. Vamos utilizar o `dataset` disponível [aqui](https://gist.githubusercontent.com/netj/8836201/raw/6f9306ad21398ea43cba4f7d537619d0e07d5ae3/iris.csv).

### É hora de código 💻

Para criar nosso modelo de ML vamos precisar de algumas bibliotecas e para instalá-las vamos rodar os seguinte comando:

```shell
$ pip install pandas sklearn
```

E agora o nosso incrível código python:

```python
import pandas as pd
from sklearn import metrics
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier

# Carrega o dataset
pd_dataset = pd.read_csv('iris.csv')

# Divide o dataset em dados de treino e teste
pd_train, pd_test = train_test_split(pd_dataset, test_size=0.4, stratify=pd_dataset['variety'], random_state=42)

# Prepara os dados de treino
pd_x_train = pd_train[['sepal.length', 'sepal.width', 'petal.length', 'petal.width']]
pd_y_train = pd_train.variety

# Treina uma Arvore de Decisão
mod_dt = DecisionTreeClassifier(max_depth=3, random_state=1)
mod_dt.fit(pd_x_train, pd_y_train)

# Prepara os dados de teste
pd_x_test = pd_test[['sepal.length', 'sepal.width', 'petal.length', 'petal.width']]
pd_y_test = pd_test.variety

# Faz previsões usando os dados de teste
prediction = mod_dt.predict(pd_x_test)

# Calcula a acurácia do nosso modelo
accuracy = metrics.accuracy_score(prediction, pd_y_test)
print('accuracy:', accuracy)
```

## Kubeflow

hufdhsuifhuisdf
