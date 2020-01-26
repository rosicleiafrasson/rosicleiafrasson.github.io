- deve ser uma preview feature no Java 14, com lançamento em março

- em Java, é necessário escrever muito código para que uma classe seja usável:
toString
hashCode e equals
métodos getters
um construtor público

Para uma simples classe, esses métodos são chatos, repetitivos e o tipo de coisa que pode facilmente ser gerada mecanicamente por uma IDE, e até o momento a linguagem não provê um caminho para fazer isso.

Esse gap é atualmente frustrante, principalmente quando você está lendo o código de alguém e parece que o autor está usando o gerador da IDE e usou todos os campos da classe, mas como você ter certeza sem checar cada linha da implementação? O que acontece se um campo foi adicionado durante um refactoring e os métodos não foram rejerados.

Será que todo esse código declarado é relamente necessário? 

O objetivo dos records é extender a sintaxe da linguagem e criar um caminho para dizer que a classe são os campos, apenas os campos e nada além dos campos. Você faz a declaração sobre a classe, o compilador pode ajudar criando todos os métodos automaticamente e tem todos os campos participantes em métodos como o hashCode.

O novo conceito é chamado de record

cada componente gera um campo final e um método de acesso para o valor

Para desabilitar a criação de novas instâncias de um record, um construtor chamado de canonical constructor é gerado, que tem uma lista de parâmetros que é a mesma declarada no estado.

Para usar um record, o programador precisa declarar os nomes dos componentes e os tipos que compõe um record.

Para acessar a nova feature com a preview flag em qualquer código que declara record: javac --enable-preview -source 14 NovaClasse.java

analisar o código gerado com os métodos criados

os métodos toString é implementado usando o mecanismo invokedynamic-based

você pode perceber que existe uma nova classe Record que atua como um supertipo para todas as classes record. Ela é abstrata e declara equals, hashcode e toString como métodos abstratos

Java é muito verboso e tem muita cerimônia

Escrever uma classe de dados tem baixo valor, é repetitivo e é propendo a erros: construtores, métodos de acesso , equals, hashCode e ToString.

IDEs podem ajudar a escrever esse tipo de código

A classe Record não pode ser diretamente extendida. Você pode tentar compilar e verificar o erro que ocorre

A única forma de usar record é declarando explicitamente um e deixar o compilador criar o arquivo de classe. Essa abordagem também que garante que todas as classes de registro são criados como final.

Automaticamente um record adquire alguns membros:
- um campo private final para cada componente declarado
- um método de acesso público para cada componente com o mesmo nome e tipo declarado
- um construtor público com a mesma assinatura da descrição do estado, que inicializa cada campo com o seu argumento correpondente
- implementações de equals e hashCode que dizem se dois registros são iguais se eles tem o mesmo tipo e contêm o mesmo estado
- uma implementação de tostring que inclui uma representação em string de todos os componentes do registro com seus nomes

Records não podem extender outra classe e não é possível declarar campos que não sejam os campos finais privados que correspondem aos componentes da declaração.
Podem ser declarados outros componentes estáticos

Records são implicitamente final e não podem ser abstratos. Isso significa que um record não pode ser aprimorado posteriormente por outra classe ou por outro record.

Os componentes de um record são implicitamente final.

Exceto as restrições listadas, records comportam-se como qualquer outra classe: eles podem ser genericos, eles podem implementar interfaces e eles são instanciados via palavra chave.

O corpo do record pode declarar métodos estáticos, campos estáticos, inicializadores estáticos, construtores



Como um enum, um record é uma forma restrita de classe. 

Um record tem um nome e uma descrição do estado. A descrição do estado declara os componentes de um record.

Records obedecem um contrato especial a respeito do método equals
R copy = new R(r.c1(), r.c2(), ..., r.cn());
r.equals(copy) é true

você pode sempre usar a API publica e o canonical constructor para serializar e deserializar records

se eu declarar dois records com o mesmo tipo e a mesma ordem, o que acontece?

como declarar um construtor compacto

Records, provê uma sintaxe compacta para declarar classes com dados imutáveis

Deve ser fácil e conciso declarar classes imutáveis.

Records é um novo tipo

ele representa uma forma restrita de declarar uma classe ( como os enums).

Records ainda são classes, mas elas são restritas, por exemplo, elas podem conter anotações e terem campos, métodos e construtores declarados como estáticos.
Mas eles não podem extender outras classes 

Record compacta a sintaxe de declaração de classe.

atualmente, é necessário escrever muito código repetitivo para uma classe de dados: construtores, métodos de acesso, equals, hashCode, ToString.
Para evitar esse código repetitivo, Java está planejando usar records.

antes
final class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // state-based implementations of equals, hashCode, toString
    // nothing else

depois
record Point(int x, int y) { }

Limitações
- Records não podem extender de outra classe e não é possível declarar campos que não sejam do tipo final 
- Records implicitamente são do tipo final 

Levando em conta esse novo tipo, dois novos métodos foram adicionados em java.lang.Class RecordComponent[] getRecordComponents() e boolean isRecord()

Um Record é basicamente uma data class, que pode ser entendida como um tipo especial de classe que se destina a conter dados.

Para declarar um tipo como um Record, o desenvolvedor está claramente expressando sua intenção que o tipo representa apenas dado.

A sintaxe para declarar um record é muito simples e concisa, se compararmos a uma declaração norm al de classe onde tipicamente é necessário implementar métodos como equals e hashCode

Records parecem ser uma escolha interessante quando modelamos coisas como classes de modelo de domínio( potencialmente para ser persistida via ORM ou DTOs - data transfer objects)

um bom caminho para pensar em como records são implementados na linguagem é relembrar os enums. Um enum também é uma classe com uma semântica especial com uma sintaxe mais agradável.
Como ainda são classes, muitas das funcionalidades disponíveis em classes são preservadas, então existe um equilíbrio entre simplicidade e flexibilidade nesse design.

Records são uma feature preview, isso significa que apesar de estar totalmente implementada, ainda não está padronizada na JDK e pode ser usada apenas ativando a flag.
Preview feature language podem ser atualizadas e até mesmo removidas em futuras versões.

não esqueção de habilitar a o enable preview no momento de compilar
> javac --enable-preview --release 14 Person.java
2
Note: Person.java uses preview language features.
3
Note: Recompile with -Xlint:preview for details.
 


