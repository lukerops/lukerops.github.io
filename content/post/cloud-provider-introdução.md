---
title: "Cloud Provider - Introdução"
date: 2022-11-29T08:56:15-03:00
draft: true
---

## Introdução

Nos dias atuais, a cloud (ou nuvem) tem se tornado cada vez comum nos ambientes das pequenas e grandes empresas, trazendo vários benefícios, principalmente para times menores e sem muito conhecimento em infraestrutura, porém, ela também trás alguns riscos para esses times. Quando abordamos o assunto, muitos acreditam que terão poder computacional infinito, custos reduzidos, alta disponibilidade, etc, com o mínimo de gerência e conhecimento de infraestrutura, redes e sistemas operacionais. É exatamente nessa ideia que está a maior armadilha.

Quando falamos sobre grandes empresas, o problema normalmente é minimizado por times responsáveis pela gestão da nuvem, dando suporte aos times de desenvolvimento, e muitos vezes, dando algumas orientações sobre como se utilizar os serviços oferecidos pelas plataformas. Porém, em empresas menores, o que normalmente observamos, é um uso mal otimizado dos serviços, muitas vezes na tentativa de migração de sistemas legados, o que pode acarretar em um custo mais elevado, menor segurança e o mais importante, a indisponibilidade dos sistemas.

A ideia de poder computacional infinito e custos reduzidos, são um dos maiores atrativos das plataformas em nuvem, mas quando paramos para analisar são dois conceitos que não fazem sentido estar reunidos. Quanto mais coisas criamos, quanto mais poder computacional precisamos, maiores serão as contas que teremos que pagar no fim do mês. A vida é assim. Nada sai de graça. Esse assunto não é tão abordado na internet, porque todos estão sempre maravilhados em como a nuvem resolveu os seus problemas, em como a nuvem é maravilhosa, mas ninguém ajuda as empresas de pequeno porte, que devido aos seus sistemas legados e sua pouca mão de obra, terão os seus custos elevados pela migração para nuvem, as pessoas e empresas que visam a privacidade e preferem hospedar os seus próprios serviços para serem donos de seus próprios dados,...

Sempre que abordamos o assunto de nuvem, muitos pensam na hora em plataformas de grandes empresas como: Google Cloud, Amazon Web Service (AWS), Microsoft Azure, Oracle Cloud, etc, porém, eles esquecem que normalmente essas empresas estão apoiadas em projetos open source para construir suas plataformas. É com tudo isso que já foi abordado anteriormente, que vou iniciar uma série de artigos apresentando, discutindo e configurando uma série de ferramentas open source visando a criação de uma nuvem privada com os principais serviços oferecidos por essas plataformas.

## Objetivos

A ideia desses artigos serão criar uma documentação de tudo que estou fazendo e como estou fazendo para criar a minha própria nuvem privada, e com isso, deixar registrado para mim mesmo, e ao mesmo tempo, ajudar todas as pessoas que se benficiariam de ter a sua própria nuvem privada.

> Uma observação muito importante a ser feita é que tudo será configurado utilizando hardware de baixo custo, os famosos Single Board Computer (SBC) com arquitetura ARM, ou seja, será uma nuvem visando o menor investimento e o menor consumo energético possível, e com isso, mostrando é viável a criação uma nuvem privada gastando pouco.
