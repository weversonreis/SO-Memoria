# Conceitos Memória RAM, memória ROM, SWAP e Paginação de Memória.

## 1. Memória RAM

Para que um computador funcione é necessário que os dados que serão trabalhados pelo processador estejam carregados na memória. Esses dados então são passados desta memória (a Memória RAM), para o processador, onde, após processados, serão movidos de volta para a Memória RAM. Assim o usuário pode executar programas e, consequentemente, utilizar a máquina. (Esta informação está correta, porém é superficial; Para mais detalhes ou um aprofundamento melhor consulte bibiliografia espefíca para Introdução à [Ciência da] Computação) Todo esse processo ocorre numa velocidade além da percepção do usuário, mas nem sempre a Memória RAM dá conta de carregar tudo o que está sendo executado em um computador, então se faz necessário o uso da Memória Virtual.

## 2. Memória ROM

Para o computador, a capacidade de memorizar informações também é essencial e, por isso, as máquinas costumam ter diversos dispositivos de armazenamento de dados. Um deles é o disco rígido, também chamado de HD. Nele são guardados os arquivos de fotos, de músicas e de softwares, que estão sempre à disposição do usuário.
Outra forma de armazenar dados no computador é por meio da Random-Access Memory (RAM). Sem ela, o computador nem chega a completar o processo de boot, por exemplo.

Aqueles que nunca ouviram falar da ROM certamente conhece um primo próximo desse tipo de memória, o CD-ROM, uma mídia ótica que permite apenas a leitura de dados. Ou seja, você não pode gravar arquivos em um CD-ROM, apenas executar ou visualizar o que já estiver nele.
Basicamente, essa é a função da memória ROM: oferecer dados apenas para leitura. Normalmente, a ROM é utilizada para armazenar firmwares, pequenos softwares que funcionam apenas no hardware para o qual foram desenvolvidos e que controlam as funções mais básicas do dispositivo.
Na ROM de uma calculadora, por exemplo, podemos encontrar as rotinas matemáticas que o estudante pode realizar ao usá-la. Já no caso de celulares, normalmente as ROMS carregam o sistema operacional e os softwares básicos do aparelho.

## 3. Troca (Swapping)

Em algumas situações não é possível manter todos os processos na memória e uma solução para essas situações é o mecanismo conhecido como swapping (troca). A gerência de memória reserva uma área do disco para esse mecanismo, que é utilizada para receber processos da memória. A execução desse processo é suspensa, com isso é dito que o mesmo sofreu uma swap-out. Mais tarde, esse mesmo processo será copiado do disco para a memória, mecanismo conhecido como swap-in. Esse mecanismo de trocas de processos no disco tem como objetivo permitir que o sistema operacional consiga executar mais processos do que caberia na memória.

Esse processo gera um grande custo de tempo de execução para os programas. Fazer a cópia do processo da memória para o disco e depois fazer o inverso é demorado.

## 4. Paginação

O espaço de endereço virtual é dividido em unidades chamadas páginas. As unidades correspondentes na memória física são chamadas molduras de página (ou quadros). As páginas e as molduras (quadros) têm sempre exatamente o mesmo tamanho.

No espaço físico (memória) tem-se várias molduras de página. Por exemplo, podem existir 05 páginas situadas no espaço de endereço virtual que são mapeadas na memória física. No entanto, o espaço de endereço virtual é maior que o físico. As outras páginas não são mapeadas. No hardware real, um bit presente/ausente em cada entrada monitora se a página é mapeada ou não.

Quando um programa tenta utilizar uma página não mapeada em uma moldura, a MMU detecta o acontecimento (que a página não está mapeada) e gera uma interrupção, passando a CPU para o Sistema Operacional. Tal interrupção é chamada falha de página. O S.O., então, seleciona uma moldura de página pouco utilizada e grava o seu conteúdo de volta ao disco, substituindo-a pela página requisitada.

Quanto à forma como a paginação pode ser implementada, podemos considerar a paginação simples e a paginação por demanda. Na primeira, todas as páginas lógicas do processo são mapeadas e carregadas para a memória física, isso supondo-se que o espaço de endereçamento de memória para um processo tenha o tamanho máximo igual à capacidade da memória física alocada para processos. No caso da paginação por demanda, apenas as páginas lógicas efetivamente acessadas pelos processo são carregadas. Nesse caso, uma página marcada como inválida na tabela de páginas de um processo pode tanto significar que a página está fora do espaço lógico de endereçamento do processo ou que simplesmente a página ainda não foi carregada. Para descobrir qual das situações é a verdadeira basta conferir o descritor de processo, que contém os limites de endereçamento lógico do processo em questão.

## 4.1 Tabelas de Página

O propósito de tabelas de página é mapear páginas virtuais em molduras de página. No entanto, existem duas questões que devem ser consideradas:

A tabela de páginas pode ser extremamente grande, sendo que cada processo necessita de sua própria tabela de páginas;
O mapeamento deve ser rápido: o mapeamento do virtual para o físico deve ser feito em cada referência da memória, o que pode ocorrer diversas vezes em uma única instrução,não devendo tomar muito tempo para não se tornar um gargalo na execução da instrução.
Neste caso, há a necessidade de um mapeamento de páginas rápido e grande. Existem duas formas básicas de projetar tabelas de páginas:

ter uma única tabela de páginas, através de uma matriz de rápidos registradores de hardware, com uma entrada para cada página virtual, indexada pelo número da página. Esta é uma solução cara se a tabela de páginas é grande;
manter a tabela de páginas inteiramente na memória principal, sendo que o hardware precisa de apenas um registrador que aponta para o início da tabela de páginas.
Esta última solução possui algumas variações que têm desempenho melhor. Uma das propostas que busca evitar o problema de ter enormes tabelas de páginas na memória todo tempo é a de Tabelas de Páginas Multinível. Neste caso, existe uma tabela de primeiro nível e diversas tabelas de segundo nível. O importante aqui é que somente as tabelas mais utilizadas estão presentemente na memória. As demais se encontram em disco. Apesar de permitir um espaço de endereçamento muito grande, o espaço ocupado de memória principal é muito pequeno.

O sistema de tabela de páginas de dois níveis pode ser expandido para três, quatro ou mais níveis, sendo que níveis adicionais dão mais flexibilidade, mas a complexidade da implementação acima de três níveis dificilmente vale a pena. Em relação aos detalhes de uma única entrada de uma tabela de páginas, seu arranjo pode depender da máquina, mas os tipos de informação usualmente são os mesmos.

O campo mais importante é o número da moldura de página, pois o objetivo do mapeamento de páginas é localizar este valor. Ao lado dele, tem-se o bit de presente/ausente. Se este bit for 1, significa que a entrada é válida e pode ser utilizada. Se for 0, a página virtual a que esta entrada pertence não está atualmente na memória. Ao acessar uma entrada da tabela de páginas com este bit configurado como zero ocorrerá uma falha de página.

Os bits "proteção" informam que tipos de acesso são permitidos. Em sua forma simples, este campo contém apenas um bit, com 0 para leitura/gravação e 1 par leitura somente. Um arranjo mais sofisticado é manter 3 bits, cada um para habilitar leitura, gravação e execução da página. Os bits "modificada" e "referenciada" monitoram a utilização da página. Ambos os bits tem como objetivo auxiliar no momento da substituição de páginas. O último bit permite que o cache seja desativado para a página, recurso importante para páginas que são mapeadas em registradores de dispositivo em vez da memória.

Alguns bits auxiliares são, normalmente, adicionados na tabela de páginas para facilitar na substituição de páginas quando a memória física estiver cheia e for necessário retirar uma página lógica da memória física para alocar outra página lógica. Tem-se o bit de modificação (dirty bit) assim que a página for carregada tem valor zero se a página for alterada, na memória física, altera-se o valor para 1, portanto se a página for a página vítima e o bit de modificação for zero não será necessário fazer a cópia da página lógica em memória para a página lógica em disco pois as duas são iguais. Bit de referência: é zero assim que a página lógica é alocada na página física, se a página for acessada altera o valor para um (a MMU altera o valor). Bit de trava: usado em páginas que não podem sair da memória física, o Sistema Operacional "tranca" uma página lógica na memória física ativando esse bit.
