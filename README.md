# Trabalho 2 - Exemplos de Herança e Polimorfismo

Neste trabalho foi analisado o código-fonte do **libGDX**, um framework de desenvolvimento de jogos multiplataforma em Java. A análise utilizou trechos de código do sistema de cena 2D do framework para identificar exemplos de herança e polimorfismo.

## Identificando herança

Primeiro procurei por uma classe que não tinha *extends*, porque ela seria a **superclasse** (classe pai). Assim, identifiquei a classe *Actor*. 

```java
public class Actor {

    ~Código da classe
}
```

Depois de encontrar a classe *Actor*, procurei por uma que tivesse *extends Actor*. Assim, identifiquei a classe *Group*, que é uma **subclasse** (classe filha) que **herda** de *Actor*.

```java
public class Group extends Actor implements Cullable {

    ~Código da classe

}
```
 

## Identificando polimorfismo

Depois de encontrar a superclasse e a subclasse, procurei pelos métodos com Overload e Override. Utilizei `Ctrl+F` para facilitar minha busca.

### Overload

**Como identificar:** Overload ocorre quando há **múltiplos métodos com o mesmo nome na mesma classe**, mas com **parâmetros diferentes** (quantidade, tipo ou ordem).

Classe Group

**Versão 1:** Remove um ator e retira o foco automaticamente. Versão simplificada para uso comum.
```java
 public boolean removeActor (Actor actor) {
        return removeActor(actor, true);
    }
```
**Versão 2:** Remove um ator com controle sobre o foco. Permite escolher se o ator deve perder o foco ou não, útil quando você precisa manter o estado de foco do ator.
```java
public boolean removeActor (Actor actor, boolean unfocus) {
        int index = children.indexOf(actor, true);
        if (index == -1) return false;
        removeActorAt(index, unfocus);
        return true;
    }
```

### Override

**Como identificar:** Override ocorre quando uma **subclasse redefine um método herdado da superclasse** com a **mesma assinatura** (mesmo nome, mesmos parâmetros e mesmo tipo de retorno).

**Classe Actor**

Implementação vazia (padrão). Nem todos os atores precisam desenhar algo, então a classe base deixa vazio para subclasses implementarem quando necessário.
```java
public void draw (Batch batch, float parentAlpha) {
}
```
**Classe Group**

Sobrescreve `draw()` para desenhar o grupo e todos os seus filhos. Aplica transformações (rotação, escala, posição) ao grupo inteiro, permitindo animar grupos como uma unidade única.
```java
public void draw (Batch batch, float parentAlpha) {
        if (transform) applyTransform(batch, computeTransform());
        drawChildren(batch, parentAlpha);
        if (transform) resetTransform(batch);
    }
```

## Exemplo criado a partir do código analisado
```java
// Superclasse
class Actor {
    protected String name;

    public Actor(String name) {
        this.name = name;
    }

    // Método que pode ser sobrescrito
    public void draw() {
        // Implementação vazia, como no Actor original
        System.out.println(name + ": draw() não faz nada (Actor padrão)");
    }
}

// Subclasse
class Group extends Actor {

    public Group(String name) {
        super(name);
    }

    @Override
    public void draw() {
        System.out.println(name + ": desenhando grupo com transformação e filhos");
    }
}

// Subclasse
class Image extends Actor {

    public Image(String name) {
        super(name);
    }

    @Override
    public void draw() {
        System.out.println(name + ": desenhando imagem");
    }
}

// Uso do polimorfismo
public class Main {
    public static void main(String[] args) {

        // Array de Actor pode conter diferentes tipos de atores
        Actor[] actors = {
                new Actor("Actor1"),        // draw() não faz nada
                new Group("Grupo1"),        // draw() desenha filhos com transformação
                new Image("Imagem1"),       // draw() desenha uma imagem
                new Group("Grupo2"),        // outro grupo
                new Image("Imagem2")        // outra imagem
        };

        // Polimorfismo: cada objeto chama sua própria versão de draw()
        for (Actor a : actors) {
            a.draw();
        }
        
        System.out.println("\nTodos os objetos são do tipo Actor:");
        for (Actor a : actors) {
            System.out.println("- " + a.name + " é uma instância de: " + a.getClass().getSimpleName());
        }
    }
}
```
### Código executado


https://github.com/user-attachments/assets/cd8db5a4-d0fa-445d-8f2f-2be8a5333c5f



### Explicação:
- **Actor** é a superclasse (classe base)
- **Group** e **Image** são subclasses que herdam de Actor
- Cada subclasse sobrescreve (`@Override`) o método `draw()` com comportamento específico
- O array pode guardar diferentes tipos de objetos (`Actor`, `Group`, `Image`) e todos são tratados como `Actor`


## Dificuldades

| **Pontos mais fáceis** | **Pontos menos fáceis** |
|------------------------|-------------------------|
|Entender como funciona override e overload  | Entender o que o código fonte fazia |
|Encontrar overrides e overloads |Pensar num exemplo de código  |

##
<img width="100" height="100" alt="image" src="https://github.com/user-attachments/assets/07fbaf54-1203-4e15-8c94-12fcac3835f7" />

## Referências

**LIBGDX**. libGDX: Desktop/Android/HTML5/iOS Java game development framework. Disponível em: https://github.com/libgdx/libgdx. Acesso em: 22 out. 2025.

ANDREA, INF. ELC117 - Aula 23. Disponível em: https://liascript.github.io/course/?https://raw.githubusercontent.com/AndreaInfUFSM/elc117-2025b/main/classes/23/README.md#1
### Arquivos analisados

- `gdx/src/com/badlogic/gdx/scenes/scene2d/Actor.java` - Classe base do sistema de cena 2D
- `gdx/src/com/badlogic/gdx/scenes/scene2d/Group.java` - Classe que herda de Actor
