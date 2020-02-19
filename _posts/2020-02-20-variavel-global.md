---
layout: post
title: Variáveis globais devem ser evitadas?
---

# Variáveis globais devem ser evitadas?

 Em 1973, no auge de linguagens como Fortran e Cobol, um artigo intitulado "Global Variable considered harmful" escrito por W. Wulf e Mary Shaw já advertia que o uso de variáveis não locais constitui um importante fator em programas difíceis de entender. Atualmente, após quase 50 anos e o advento de muitas linguagens de programação e paradigmas de desenvolvimento, essa advertência continua válida.

 Entenda como variável global uma variável que pode ser acessada e modificada em todo o software ou em grande parte dele. 
 
 Com essa definição elas parecem inofensivas. Afinal, que mal pode causar ter uma propriedade que é visível e pode ser modificada em qualquer parte do sistema? Como resposta: muitos e dos mais difíceis de encontrar, entender e corrigir.

 Talvez o principal problema e a raiz para os demais, é que o uso de variáveis globais cria um acoplamento implícito entre os módulos que as utilizam. E a palavra implícito pode ser trocada por perigoso nesse contexto, pois esses acoplamentos não são bem representados no design da aplicação.

 Uma variável global pode simplesmente aparecer em um trecho de código: ela não foi definida, não está na lista de parâmetros e não existe um jeito fácil de descobrir o que está acontecendo com ela. Qualquer regra sobre seu uso pode ser quebrada ou esquecida a qualquer momento.

É possível também, que uma variável local seja declarada com o mesmo nome de uma variável global, causando mais problemas de compreensão de código, pois podemos estar usando uma variável global pensando ser local ou vice-versa.

 Isso significa que se você quer saber o que uma variável global faz, você precisa procurar pela sua definição, que pode ser em qualquer lugar. Depois, você precisa encontrar todos os lugares em que o valor da mesma pode ser alterada. Se você tiver sorte, não serão muitos.

 E se forem muitos? O fato é que quando uma variável global é disponibilizada, qualquer desenvolvedor, em qualquer módulo da aplicação pode fazer uso da mesma. O que inicialmente era um facilitador pode se tornar uma grande armadilha à medida que o software cresce, que a equipe muda, que refatorações são feitas.
 
 E os testes unitários? Extremamente complexos, quando forem possíveis. É praticamente impossível saber o estado inicial de uma variável declarada globalmente quando o teste é executado. Quanto menos testável é sua aplicação, menor será a segurança no momento em que as alterações forem necessárias.

E mais... Aplicações com um número considerável de variáveis com escopo além do necessário tendem a degradar muito com o tempo. Bugs aparecem inesperadamente em lugares inimagináveis e são difíceis de diagnosticar. O esforço em refatorações aumentam consideravelmente porque é necessário rastrear o uso das variáveis globais. O reúso torna-se impraticável.

 Se for inevitável, tê-las em sua aplicação, mitigue o risco. Você pode usar uma convenção de nomes que torne fácil o reconhecimento de uma variável global. Você pode ter um arquivo com todas as variáveis globais do projeto. Você pode deixar a responsabilidade de escrita centralizada em um único local e apenas a leitura acessada em toda a aplicação. 

Mas, o mais importante é ter em mente que o menor escopo de algo, é sempre mais fácil de manter. Menor acoplamento, menor o risco, maior a segurança no momento das alterações. O uso de variáveis globais é defensável em casos extremamente raros. Deixe seu software simples. Evite váriaveis globais sempre que possível. Se for possível construir a sua aplicação sem elas, muito melhor.

## Referências

W. Wulf - Global variable considered harmful
