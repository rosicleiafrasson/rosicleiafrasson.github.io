
# Records no Java 14

- O que justifica a criação de um novo tipo em Java;
- O que é um Record;
- Classe java.lang.Record;
- Tirando proveito do novo tipo record;
- Como ficam as coisas por baixo dos panos;

É possível observar nas últimas versões do Java, que esforços com o intuito de reduzir a verbosidade da linguagem tem sido constantes. No Java 14, que deve ser lançado em março deste ano, não será diferente. Liderada por Brian Goetz, a JEP 359, que será liberada como feature preview provê uma sintaxe compacta para declaração de classes imutáveis em Java. 

Nossos projetos Java, normalmente contém uma infinidade de classes que representam modelos do domínio e que servem para fazer persistência em base de dados. A estrutura desse tipo de classe é sempre composta por uma lista de atributos, construtores, métodos de acesso, métodos equals, hashCode e toString. Enfim, código repetitivo, de baixo valor e que está muito propenso a erro em refatorações. 

Você pode pensar... Mas as IDEs geram esse amontoado de código. Sim, elas geram. Mas você tem que concordar com dois pontos: o primeiro deles é que quando é feito um refactoring, é muito comum esquecermos de regerar; e o segundo, é que estamos falando de um amontoado de código para ler e entender e mesmo sendo gerado automaticamente, em caso de bugs, comportamentos inesperados é necessario analizar.


## O que é um Record

Um record é um tipo de classe que serve para modelar classe de dados, Outras linguagens como Kotlin e Scala possuem tipos similares. Ao declarar um tipo como record, o desenvolvedor expressa a intenção que o novo tipo de dado, serve para representar dados.

A sintaxe para declarar um record é muito simples, se compararmos com uma declaração habitual de uma classe, onde normalmente é necessário sobrescrever os métodos equals e hashCode, métodos de acesso e construtores.

Como já mencionado, records são uma preview feature language, isso significa que embora esteja totalmente implementada, ela ainda não é um padrão na JDK e é necessário habilitar o compilador para usar com: javac --enable-preview --release 


## Classe Record

A classe Record está definida no pacote java.lang e está ilustrada a seguir. Como é uma preview feature, existe a possibilidade de ser atualizada ou até mesmo removida nas próximas versões do Java. 

```
package java.lang;

@jdk.internal.PreviewFeature(feature=jdk.internal.PreviewFeature.Feature.RECORDS,
                             essentialAPI=true)
public abstract class Record {

    protected Record() {}

    @Override
    public abstract boolean equals(Object obj);

    @Override
    public abstract int hashCode();

    @Override
    public abstract String toString();
}
```

Record é uma classe abstrata e todas as classes que extenderem de Record, devem possuir obrigatoriamente os seguintes membros:
- um campo final e private para cada componente declarado, com o mesmo nome e mesmo tipo;
- um construtor público, chamado de canonical constructor, que inicializa cada campo com o seu argumento correpondente e impede a criação de novos construtores;
- um método de acesso público com o mesmo nome declarado e retorna o mesmo tipo para cada componente.
- método equals que deve obedecer um contrato especial, onde:  
```
R copy = new R(r.c1(), r.c2(), ..., r.cn());
``` 
isso significa que ```r.equals(copy)``` será verdadeiro;
- método hashCode que é implementado usando usando a combinação do valor de código hash de todos os componentes;
- método toString que retorna o nome da classe, o nome e o valor de cada componente.


## Usando um record

Para usar um record, é necessário declarar explicitamente um e deixar o compilador criar o arquivo de classe. Na declaração, também é necessário especificar seus componentes, com os seus respectivos tipos.

A título de exemplo, vamos declarar um record com o nome de PhoneNumber, com dois componentes: prefix e number, os dois do tipo String.

```
public record PhoneNumber(String prefix, String number) {}
```

O .class de PhoneNumber é o seguinte: 

```
public final class PhoneNumber extends java.lang.Record {
    private final java.lang.String prefix;
    private final java.lang.String number;

    public PhoneNumber(java.lang.String prefix, java.lang.String number) { /* compiled code */ }

    public java.lang.String toString() { /* compiled code */ }

    public final int hashCode() { /* compiled code */ }

    public final boolean equals(java.lang.Object o) { /* compiled code */ }

    public java.lang.String prefix() { /* compiled code */ }

    public java.lang.String number() { /* compiled code */ }
}
```

Note que foi gerada uma classe final que indica não ser possível a criação de subclasses a partir dela. E ela extende de Record, que é a classe base de todos os records e que possui os métodos abstratos que o compilador implementou.

Nessa classe temos prefix e number como atributos final. O construtor criado também possui como argumentos os dois componentes e temos dois métodos de acesso com os mesmos nomes. O construtor gerado é equivalente a:

```
public PhoneNumber(java.lang.String prefix, java.lang.String number) {
    this.prefix = prefix;
    this.number = number;
}
```

Por este motivo, instanciar um tipo record, é exatamente igual a instanciar qualquer outra classe Java. 

```
PhoneNumber phoneNumber = new PhoneNumber("048", "36466128");
```

Para acessar um atributo da classe, é um pouco diferente do que estamos habituados, já que os métodos de acesso não possuem o prefixo get.

```
phoneNumber.number();
```

O método equals também funciona um pouco diferente, já que ele obedece um contrato especial, já discutido anteriormente. Note:

```
PhoneNumber phoneNumber1 = new PhoneNumber("048", "36466128");
PhoneNumber phoneNumber2 = new PhoneNumber("048", "36466128");

System.out.println(phoneNumber1.equals(phoneNumber2));
```

Esse trecho de código deve imprimir na tela o valor true. Ficou confuso? Isso acontece porque no contrato especificado em Record, Dois Records são iguais, se eles forem do mesmo tipo e os valores de seus atributos forem os mesmos. Como os Records são imutáveis, não teremos problemas com a mudança de estado durante a execução do programa.

E para finalizar, temos implementado um método toString que inclui uma representação em string de todos os componentes com seus respectivos nomes e valores. A impressão de um PhoneNumber, deve ser algo similar a: PhoneNumber[prefix=048, number=36466128].

## Adição de novos membros em um Record

É importante salientar que Records ainda são classes. Elas podem conter anotações, métodos podem ser adicionados e variáveis estáticas podem ser criadas. Mas são classes restritas: novos atributos e construtores não são permitidos. Também não é possível extender uma classe do tipo Record.

No record PhoneNumber, declarado anteriormente, foi adicionado um novo método e uma variável estática. Veja:

```
public record PhoneNumber(String prefix, String number){

    private static final String DIVISOR = "-";
 
    public String fullPhoneNumber(){
        return prefix + DIVISOR + number;
    }
}
```

Outro ponto importante, é que a classe Record não pode ser diretamente extendida. Ocorre erro de compilação se existir a tentativa.


## Considerações finais 

Records introduzem na linguagem Java uma forma muito mais simples e concisa para declararmos classes de dados, sem a necessidade de escrevermos um amontoado de código verboso. Adaptar-se a este novo tipo é só questão de tempo. E que venha o Java 14.



