---
title: "Cloud Provider - Introdução"
date: 2022-11-29T08:56:15-03:00
tags:
- cloud provider
- private cloud
categories:
- cloud provider
- private cloud
---

## Introdução

Atualmente, a nuvem (ou como muitos preferem chamar, a cloud) tem se tornado cada vez mais comum nos ambientes das pequenas e grandes empresas, trazendo vários benefícios, principalmente para times menores e sem muito conhecimento em infraestrutura. Porém, ela também traz alguns riscos para esses times. Quando abordamos o assunto, muitos acreditam que terão poder computacional infinito, custos reduzidos, alta disponibilidade, com o mínimo ou praticamente nenhuma gerência e conhecimento de infraestrutura, redes e sistemas operacionais. É exatamente nessa ideia que está a maior armadilha.

Quando falamos sobre grandes empresas, o problema é normalmente minimizado por times responsáveis pela gestão da nuvem dando suporte aos times de desenvolvimento, e muitos vezes, dando algumas orientações sobre como se utilizar os serviços oferecidos pelas plataformas. Porém, em empresas menores, o que normalmente observamos é um uso mal otimizado dos serviços, muitas vezes na tentativa de migração de sistemas legados, o que pode acarretar um custo mais elevado, menor segurança e o mais importante, a indisponibilidade dos sistemas.

A ideia de poder computacional infinito e custos reduzidos, são um dos maiores atrativos das plataformas em nuvem, mas quando paramos para analisar são dois conceitos que não fazem sentido estar reunidos. Quanto mais coisas criamos, quanto mais poder computacional precisamos, maiores serão as contas que teremos que pagar no fim do mês. A vida é assim. Nada sai de graça. Esse assunto não é tão abordado na internet, porque todos estão sempre maravilhados em como a nuvem resolveu os seus problemas, em como a nuvem é maravilhosa, mas ninguém ajuda as empresas de pequeno porte, que devido aos seus sistemas legados e sua pouca mão de obra, terão os seus custos elevados pela migração para nuvem, as pessoas e empresas que visam a privacidade e preferem hospedar os seus próprios serviços para serem donos de seus próprios dados, ...

Sempre que abordamos o assunto de nuvem, muitos pensam na hora em plataformas de grandes empresas como: Google Cloud, Amazon Web Service (AWS), Microsoft Azure, Oracle Cloud, etc., porém, eles esquecem que normalmente essas empresas estão apoiadas em projetos open source para construir suas plataformas. É com tudo isso que foi falado anteriormente, que vou iniciar uma série de posts apresentando, discutindo e configurando uma série de ferramentas open source visando a criação de uma nuvem privada com os principais serviços oferecidos por essas plataformas.

## Objetivos

A ideia desses posts será criar uma documentação de tudo que estou fazendo e como estou fazendo para criar a minha própria nuvem privada, e com isso, deixar registrado para mim mesmo, e, simultaneamente, ajudar todas as pessoas que se beneficiariam de ter a sua própria nuvem privada.

> Uma observação muito importante a ser feita é que tudo será configurado utilizando hardware de baixo custo, os famosos SBCs (Single Board Computer) com arquitetura ARM, ou seja, será uma nuvem visando o menor investimento e o menor consumo energético possível, e com isso, mostrando é viável a criação uma nuvem privada gastando pouco.

## Requisitos

A característica mais importante de uma nuvem é a alta disponibilidade, ou seja, quando uma máquina falha, outra está disponível para assumir toda a carga de trabalho, e assim, mantendo todos os serviços sempre disponíveis. Com essa característica em mente, nossa nuvem privada será construída em cima de um cluster com no mínimo 3 máquinas e cada máquina precisa ter pelo menos 4 GB de RAM para suportar todas as ferramentas que serão utilizadas.

Como estamos falando de cluster, precisamos pensar na manutenibilidade de toda a solução e é por isso que utilizaremos umas das ferramentas mais utilizadas atualmente para criação e gestão de cluster, o `kubernetes`.

> Tenha em mente que nem tudo será discutido em detalhes, assim, é esperado que você tenha alguns conhecimentos básicos sobre `kubernetes` e sobre `terraform` (ferramenta que utilizaremos para configurar o cluster).

## Conclusão

Esse primeiro post era apenas para introduzir o assunto e dar um pequeno contexto do que será construído. No próximo já abordaremos a configuração das máquinas, criação de códigos terraform e apresentação de alguns conceitos que serão a base de toda nossa infraestrutura, então até a próxima.
