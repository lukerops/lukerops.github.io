---
title: "Orientação a Objetos em C"
date: 2022-08-25T05:37:05-03:00
draft: true
---

Todo mundo que entra nesse universo da programação já ouviu falar sobre o paradigma de orientação a objetos, mas você sabia que da pra aproveitar esses conceitos e aplicar em C?

## Vamos do começo

A linguagem C é uma linguagem de programação de propósito geral desenvolvida pelo Dennis Ritchie lá na década de 1970 para o sistema operacional UNIX.[^1] Porém, diferente das linguagens mais atuais, o C suporta apenas os paradigmas mais simples, como: imperativo e processual (em inglês, *procedural*).

[^1]: [https://www.bell-labs.com/usr/dmr/www/chist.html](https://www.bell-labs.com/usr/dmr/www/chist.html)

**Mas se o C só aceita os paradigmas mais simples, como vamos conseguir programar usando orientação a objetos? 🤔**

E a resposta, apesar de parecer um pouco complexa é na verdade bem simples. Vamos aproveitar de alguns comportamentos e características do C para replicar alguns dos conceitos por trás da orientação a objetos.

## Características da Linguagem C

Vou assumir que se você chegou até aqui, provavelmente, já tenha um conhecimento básico da sintaxe da linguagem C, e portanto, não vou fazer explicações em relação a isso.

### Alocação de Memoria para structs

Quando definimos uma `struct` em C estamos na verdade definindo como queremos que aquele bloquinho de variáveis seja alocado na memória, ou seja, a ordem que definirmos as coisas dentro da `struct` é exatamente a ordem que os dados serão armazenados na memória. Vamos a um exemplo:

```c
struct exemplo {
  int numero;
  char texto[10];
};
```

No código acima, temos a definição da `struct exemplo` e nela definimos duas variáveis, *numero* e *texto*, com os tipos `int` e `char[10]` respectivamente. Considerando que, por padrão, variáveis do tipo `int` ocupam 4 bytes e do tipo `char` ocupam 1 byte, então nossa `struct` ocupará:
```
4 (int) + 10 * 1 (char) = 14 bytes
```

E pela ordem da definição das variáveis, nossa `struct` será alocada na memória da seguinte forma:

```
            Memoria
[----------------------------] -> 14 bytes / 28 tracinhos
[| int  ||     char[10]     |] -> int (4 bytes / 8 tracinhos)
                               -> char[10] (10 bytes / 20 tracinhos)
```

> 🚨 Obs: os números apresentados acima servem apenas para fins educativos. Na prática os valores podem mudar pelos motivos:
> - tamanhos dos tipos primitivos serem diferentes.
> - processo de <cite>data alignment[^2]</cite> para a arquitetura da CPU utilizada.

[^2]: [https://en.wikipedia.org/wiki/Data_structure_alignment](https://en.wikipedia.org/wiki/Data_structure_alignment)

### Referência de Funções / Ponteiros de Funções

No C é possível passar a referência de uma função como parâmetro de outra função, tornando possível implementar o que chamamos de `callback`. Essa referência é passada através de um ponteiro de função.

Sempre que declaramos uma função, o nome dado a função passa automaticamente a ser uma ponteiro para a função, assim, deixando que seja possível a chamada da função em qualquer parte do nosso código. Exemplo:

```c
/*
Quando declaramos a função, o nome "funcao_exemplo"
passa a ser uma referência tornando possível fazer a
chamada "funcao_exempo(1,2);" de qualquer parte do
nosso código.
*/

int funcao_exemplo(int x, int y) {
  return x + y;
}

/*
Também é possível definirmos um ponteiro de função
diretamente, ficando:
[tipo de retorno] (* [nome do ponteiro])([argumentos da função])
*/

int exemplo_funcao_como_argumento(int (* callback)(int x, int y), int x, int y) {
  return callback(x, y);
}

/*
Para passar a função "funcao_exmplo" como argumento
para a função "exemplo_funcao_como_argumento" basta fazer:
exemplo_funcao_como_argumento(funcao_exemplo, 1, 2);
*/

void print() {
  // deverá imprimir "resultado: 3" na tela
  printf("resultado: %d\n", exemplo_funcao_como_argumento(funcao_exemplo, 1, 2));
}
```

### Conversão de Tipos

Conversão de tipos é algo comum de se fazer, porém, em C, o seu comportamento se difere dependendo da forma que você esteja utilizando e as vezes nós não nos damos conta dessa diferença, o que pode acarretar em um bug.

#### Conversão de Tipos Primitivos

Quando fazer a conversão de tipos primitivos (ex: `int`, `char`, `float`, ...) o compilador fica responsável por fazer a operação, resultando em uma conversão dos valores. Exemplo:

```c
int a = 10;
float b = (float) a;
// Isso irá resultar em um float com o valor b = 10.0
```

#### Conversão de Tipos de Ponteiros

Como ponteiros são apenas apontamentos para endereços de memória, quando fazemos a conversão de tipo de um ponteiro estamos na verdade dizendo como o nosso código deveria interpretar aquele dado que está salvo na memória e caso não tomemos cuidado com os tamanhos, posição e os dados que estão salvos podemos gerar um bug em nosso código. Exemplo:

```c
char a = 'a';
int b = (int) a;
/*
Dessa forma, como falado anteriormente, o compilador
fica responsável por fazer a operação, e com isso,
resultando em um inteiro com valor b = 97
(obs: isso acontece porque 'a' na tabela ascii é 97)
*/

char *a = "a";
int *b = (int*) a;
/*
Fazendo a conversão de ponteiro estamos apenas dizendo
ao nosso código como interpretar os dados que estão na
posição de memoria apontada pelo ponteiro "char *a"
*/
```

Na conversão de ponteiros apresentado acima não é possível prever o valor que será interpretado pelo ponteiro `int *b`. 🤯 Isso acontece porque o bloco de memoria alocado para a variável `char *a` é menor do que o esperado pelo inteiro, portanto, ele irá utilizar um pedaço da memória que não é dele para interpretar o inteiro e por isso não tem como prever o valor que será utilizado.

## Orientação a Objetos implementados em C

Vou assumir que se você chegou até aqui, já tem familiaridade com os conceitos de orientação a objetos.

Agora que já entendemos as características do C que vão nos permitir fazer a implementação, vamos por a mão na massa. 🥳

### Definição de Classes

Já que não possuímos nenhuma estrutura de classes em C, vamos utilizar uma `struct` para representar nossas classes e vamos utilizar ponteiros de funções para representar nossos métodos.

Por se tratarem de métodos (funções que realizam operações em cima dos atributos da classe), as funções que utilizaremos precisam receber uma referência para o objeto da própria classe.
No exemplo a seguir eu utilizo a referência ao objeto como o parâmetro `self` nos métodos.

```c
struct veiculo {
  // Qualquer atributo que queira na classe
  int rodas;
  float kilometragem;
  float combustivel;
  int _ligado;

  // Agora definimos os métodos da nossa classe
  int (* ligar)(struct veiculo *self);
  int (* locomover)(struct veiculo *self, float km);
};
```

Assim como em algumas linguagem (ex: python), não é possível definir atributos públicos e privados em nossa struct, portanto, vou utilizar o mesmo padrão de nomenclatura que é utilizado pela comunidade python. Para representar atributos e métodos privados da classe, vamos utilizar o prefixo "_", como em `int _ligado;`.

#### Construtor