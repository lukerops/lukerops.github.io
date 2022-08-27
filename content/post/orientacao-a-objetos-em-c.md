---
title: "Orientação a Objetos em C"
date: 2022-08-25T05:37:05-03:00
draft: true
---

Todo mundo que entra nesse universo da programação já ouviu falar sobre o paradigma de orientação a objetos, mas você sabia que da pra aproveitar esses conceitos e aplicar em C?

## Vamos do começo

A linguagem C é uma linguagem de programação de propósito geral desenvolvida pelo Dennis Ritchie lá na década de 1970 para o sistema operacional UNIX.[^1] Porém, diferente das linguagens mais atuais, o C suporta apenas os paradigmas mais *"antigos"*, como: imperativo e processual (em inglês, *procedural*).

[^1]: [https://www.bell-labs.com/usr/dmr/www/chist.html](https://www.bell-labs.com/usr/dmr/www/chist.html)

**Mas se o C só aceita os paradigmas mais *"antigos"*, como vamos conseguir programar usando orientação a objetos? 🤔**

A resposta, apesar de parecer um pouco complexa, é na verdade bem simples. Vamos aproveitar de algumas características do C para replicar alguns dos conceitos por trás da orientação a objetos.

## Características da Linguagem C

Vou assumir que se você chegou até aqui, provavelmente, já tenha um conhecimento básico da sintaxe da linguagem C, e portanto, não vou fazer explicações em relação a isso.

### Alocação de Memoria para structs

Quando definimos uma `struct` em C estamos na verdade definindo como queremos que aquele bloquinho de variáveis seja alocado na memória, ou seja, a ordem que definimos as coisas dentro da `struct` é exatamente a ordem em que os dados serão armazenados na memória. Vamos a um exemplo:

```c
struct exemplo {
  int numero;
  char texto[10];
};
```

No código acima, temos a definição da `struct exemplo` e nela definimos duas variáveis, `numero` e `texto`, com os tipos `int` e `char[10]` respectivamente. Considerando que, por padrão, variáveis do tipo `int` ocupam 4 bytes e do tipo `char` ocupam 1 byte, então nossa `struct` ocupará:
```
4 (int) + 10 * 1 (char) = 14 bytes
```

E pela ordem da definição das variáveis, nossa `struct` será alocada na memória da seguinte forma:

```
            Memoria               (1 byte = 2 tracinhos)
[----------------------------] -> 14 bytes / 28 tracinhos
[  int   --------------------] -> int (4 bytes / 8 tracinhos)
[--------      char[10]      ] -> char[10] (10 bytes / 20 tracinhos)
```

> 🚨 Obs: os números apresentados acima servem apenas para fins educativos. Na prática os valores podem mudar pelos motivos:
> - tamanhos dos tipos primitivos serem diferentes.
> - processo de <cite>data alignment[^2]</cite> para a arquitetura da CPU utilizada.

[^2]: [https://en.wikipedia.org/wiki/Data_structure_alignment](https://en.wikipedia.org/wiki/Data_structure_alignment)

### Ponteiros como parâmetro de Função

Ponteiro é um tipo de dado que serve para aponta para um endereço de memoria onde seu dado está de fato armazenado.

#### Ponteiros de variáveis como parâmetro

Assim como qualquer outro tipo de variável, variáveis do tipo ponteiro também podem ser passados como parâmetro de uma função. Ao fazer isso, passamos a conseguir alterar o valor da variável em seu contexto original. Exemplo:

```c
/*
Para declarar uma função que recebe um ponteiro como
parâmetro é igual a declarar um argumento comum, bastando
colocar o "*".
*/

int soma(int *x, int *y) {
  return (*x) + (*y);
}

/*
Outra forma muito utilizada é a passagem de um ponteiro
como parâmetro para que se manipule o valor fora do
escopo da função.
*/

int concatena_string(const char *src, char *dst, int dst_size) {
  if ((strlen(src) + strlen(dst)) > dst_size) {
    printf("dst sem o tamanho adequado\n");
    return -1;
  }

  strcat(dst, src);
  return 0;
}
```

#### Ponteiros de Funções como parâmetro

No C é possível passar a referência de uma função como parâmetro de outra função, tornando possível implementar o que chamamos de `callback`. Essa referência é passada através de um ponteiro de função. Exemplo:

> Sempre que declaramos uma função, o nome dado a função passa automaticamente a ser uma ponteiro para a função, assim, deixando que seja possível a chamada da função em qualquer parte do nosso código.

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

int exemplo_funcao_como_argumento(int (* callback)(int x, int y), int a, int b) {
  return callback(a, b);
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

Conversão de tipos é algo comum de se fazer, porém, em C, o seu comportamento se difere dependendo da forma como esteja sendo  utilizado, e as vezes, nós não nos damos conta dessa diferença, o que pode acarretar em um bug.

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

> Perceba que, nesse caso, a declaração da variável `char *a = "a";` é equivalente a `char a[2] = "a";`.

> Em C, toda string (ou vetor de `char`) é terminada com o caractere `\0` como uma forma de dizer que a string chegou ao fim, e por isso, a variável `char *a` vai possuir o valor `a\0`, o que acarreta em um espaço de 2 bytes alocados na memória.

Na conversão de ponteiros apresentado acima não é possível prever o valor que será interpretado pelo ponteiro `int *b`. 🤯 Isso acontece porque o bloco de memoria alocado pela string "a" (2 bytes) e apontado pela variável `char *a` é menor do que os 4 bytes esperados para um inteiro, portanto, ele irá utilizar um pedaço da memória que não é dele para interpretar o inteiro, e por isso, não tem como prever o valor que será utilizado.

```
         Memoria              (1 bytes = 5 tracinhos)
[------------------------]
   │        └─ Final do bloco alocado para o char[2] na memoria
   └─ Início do bloco alocado para o char[2] na memoria

[-- char[2]  ------------] -> char[2] (2 bytes / 10 tracinhos)
   ↑
char *a

[--        int         --] -> int (4 bytes / 20 tracinhos)
   ↑
 int *b
```

## Orientação a Objetos implementados em C

Vou assumir que se você chegou até aqui, já tem familiaridade com os conceitos de orientação a objetos.

Agora que já entendemos as características do C que vão nos permitir fazer a implementação, vamos por a mão na massa. 🥳

### Definição de Classes

Já que não possuímos nenhuma estrutura de classes em C, vamos utilizar uma `struct` para representar nossas classes e vamos utilizar ponteiros de funções para representar nossos métodos.

Por se tratarem de métodos (funções que realizam operações sobre os atributos da classe), as funções que utilizaremos precisam receber uma referência para o objeto da própria classe. No exemplo a seguir eu utilizo a referência ao objeto como o parâmetro `self` nos métodos.

```c
struct veiculo {
  // Qualquer atributo que queira na classe
  int rodas;
  float kilometragem;
  float combustivel; // Litros de combustível
  int _estado;
  float _eficiencia; // km/L

  // Agora definimos os métodos da nossa classe
  int (* ligar)(struct veiculo *self);
  int (* desligar)(struct veiculo *self);
  float (* locomover)(struct veiculo *self, float km);
};
```

Assim como em algumas linguagem (ex: python), não é possível definir atributos públicos e privados em nossa `struct`, portanto, vou utilizar o mesmo padrão de nomenclatura que é utilizado pela comunidade python. Para representar atributos e métodos privados da classe, vamos utilizar o prefixo "_", como em `int _estado;`.

#### Métodos

A princípio, os métodos serão implementados como funções comuns, e posteriormente, faremos a ligação deles com a classe.

```c
/*
Vamos criar um enum pra representar os estados
do veiculo.
*/
enum veiculo_estado {
  VEICULO_DESLIGADO = 1,
  VEICULO_LIGADO,
};

/*
A função de ligar irá mudar o estado do
veiculo se tiver combustível para isso.
*/
int veiculo_ligar(struct veiculo *self) {
  if (self->combustivel == 0) {
    return -1;
  }

  self->_estado = VEICULO_LIGADO;
  return 0;
}

/*
A função de desligar apenas muda o estado do
veiculo.
*/
int veiculo_desligar(struct veiculo *self) {
  self->_estado = VEICULO_DESLIGADO;
  return 0;
}

/*
O método locomover irá aumentar na kilometragem a
quantidade de km andada pelo veiculo dado a quantidade
de combustível presente e sua eficiência, retornando
a quantidade de km andados.
*/
float veiculo_locomover(struct veiculo *self, float km) {
  if (self->_estado == VEICULO_DESLIGADO) {
    return 0;
  }

  float km_max = self->_eficiencia * self->combustivel;

  if (km_max > km) {
    self->kilometragem += km;
    self->combustivel -= km / self->_eficiencia;
    return km;
  }

  self->kilometragem += km_max;
  self->combustivel = 0;
  return km_max;
}
```

#### Construtor

Em orientação a objetos, o construtor faz parte de um tipo especial de função. Ele é responsável por inicializar os atributos de nossa instância e é declarado dentro do escopo da classe, porém, em C não temos isso definido na linguagem, e portanto, substituiremos por uma função simples definida com o seguinte padrão de nomenclatura:

```
void [nome da classe]_init([referencia do objeto], [parâmetros do construtor])
```

```c
void veiculo_init(struct veiculo *obj, float eficiencia) {
  /*
  Como no C não conseguimos definir os valores padrões
  que serão utilizados na struct, então precisamos
  inicializar todos os atributos que receberão um
  valor diferente do padrão.
  */
  ptr->_estado = VEICULO_DESLIGADO;
  ptr->_eficiencia = eficiencia;

  /*
  Vamos atribuir as funções que definimos previamente
  como métodos da nossa classe utilizando os ponteiros
  de funções.
  */
  ptr->ligar = veiculo_ligar;
  ptr->desligar = veiculo_desligar;
  ptr->locomover = veiculo_locomover;

  return;
}
```

### Herança

Uma das maiores vantagens da orientação a objetos é a presença da herança. Com ela podemos reaproveitar os métodos já definidos na classe pai e assim reaproveitar código.

Para replicar o comportamento de herança, vamos aproveitar do comportamento da alocação de memória na `struct` e definir a nossa classe pai como o primeiro elemento da `struct`. Isso facilitará na chamada dos métodos definidos na classe pai.

Para exemplificar, vamos criar duas classes filhas: `carro` e `moto`.

```c
struct carro {
  struct veiculo parent;
};

void carro_init(struct carro *obj, float eficiencia) {
  veiculo_init(&obj->parent, eficiencia);

  obj->parent.rodas = 4;

  return;
}

struct moto {
  struct veiculo parent;
};

void carro_init(struct moto *obj, float eficiencia) {
  veiculo_init(&obj->parent, eficiencia);

  obj->parent.rodas = 2;

  return;
}
``` 

### Agora vamos utilizar tudo que já definimos

Para utilizar atributos e métodos definidos na classe pai temos duas formas:
- chamar o elemento `parent` da nossa `struct`;
- aproveitar do comportamento de ponteiros e fazer a conversão de ponteiro para a classe pai.

```c
/*
Para demonstrar a utilização, primeiro precisamos
criar uma instancia da nossa classe.
*/
struct carro vw_gol;
carro_init(&vw_gol, 10.);

/*
Como falado anteriormente, podemos simplesmente chamar
o elemente "parent" da srtuct, tornando bem simples de
se chamar elementos do nível superior, porém, esse método
se torna ruim quando temos que chamar métodos de classes
do nível superior ao pai, exemplo:

supondo que vw_gol seja uma classe filha de carro, temos:
obj->vw_gol->carro->veiculo

para chamarmos métodos da classe veiculo nesse caso,
teríamos que chamar "obj.parent.parent.ligar", o que começa
a se tornar ruim.
*/
vw_gol.parent.ligar(vw_gol.parent);

/*
Podemos nos aproveitar do comportamento apresentado pela
conversão de ponteiros para nos ajudar a interpretar
dados das classes de níveis superiores, assim, tornando
mais simples a chamada dos métodos.
*/
((struct veiculo*) &vw_gol)->ligar((struct veiculo*) &vw_gol);

/*
Quando temos que utilizar métodos e atributos da classe
pai muitas vezes, podemos atribuir uma referência da
classe pai em um variável auxiliar.
*/
struct veiculo *vw_gol_veiculo = (struct veiculo*) &vw_gol;
vw_gol_veiculo->ligar(vw_gol_veiculo);
```

A conversão de ponteiros funciona apenas se definirmos a classe pai como o primeiro elemento de nossa `struct`. Com isso vamos criando camadas em volta da nossa classe, assim, podendo fazer a conversão de ponteiro para qualquer classe pai (classe interna do desenho a seguir).

Levando em consideração que `vw_gol` é uma classe filha
de `carro`, temos:

```
[[[veiculo] carro] vw_gol]
```

### Pontos de atenção

A metodologia aqui apresentada permite simular o comportamento da orientação a objetos, tornando possível a utilização desse paradigma na linguagem C, porém, existem alguns pontos de atenção. Como a utilização de métodos e atributos da classe pai é feita através da conversão de ponteiros, podemos tentar utilizar, equivocadamente, métodos e atributos de uma classe pai em um objeto que não é filha daquela classe, exemplo:

```c
// Definimos a classe "veiculo"
struct veiculo {
  int rodas;
};

// Definimos a classe "carro" como sendo filha de "veiculo"
struct carro {
  struct veiculo parent;
};

// Definimos a classe "fruta"
struct fruta {
  int calorias;
};

// Definimos a classe "banana" como sendo filha de "fruta"
struct banana {
  struct fruta parent;
};

void funcao_com_erro(struct banana *obj) {
  /*
  Apesar de "banana" não ser uma classe filha de
  "veiculo", para o compilador a conversão de ponteiros
  não está errada, e portanto, o código irá compilar,
  porém, isso produzirá um bug em runtime.
  */
  struct veiculo *conversao_errada = (struct veiculo*) obj;
  ...
}
```

## Conclusão

O texto anterior introduz a possibilidade de realizar a programação orientada a objetos utilizando puramente a linguagem C. Apresenta também alguns pontos de atenção muito importante para que não seja introduzidos bugs em runtime.

Vale ressaltar que nem todos os conceitos da orientação a objetos foram implementados (ex: polimorfismo), mas, nada impede que esses conceitos também não possam ser implementados.