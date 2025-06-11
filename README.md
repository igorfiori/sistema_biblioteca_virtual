# 📖 Sistema Avançado de Recomendação para Biblioteca Virtual

Este projeto, desenvolvido para a disciplina de **Métodos de Pesquisa e Ordenação em Estruturas de Dados da PUC PR**, implementa um Sistema de Biblioteca Virtual com um motor de recomendação de livros inteligente e aprimorado. Indo além dos requisitos básicos, o sistema utiliza uma estrutura de grafos para mapear as relações entre as obras e aplica o **algoritmo de Dijkstra** para calcular a relevância das recomendações de forma precisa.

O resultado é um sistema de sugestões que não apenas conecta livros diretamente, mas entende a "proximidade" e a relevância entre todos os títulos do acervo, oferecendo uma experiência de descoberta superior ao usuário.

---

## 📚 Visão Geral do Projeto

O objetivo central foi construir um sistema de recomendação onde os livros são **nós em um grafo** e as conexões entre eles representam **recomendações**. A grande inovação está no uso do algoritmo de **Dijkstra** para determinar o "caminho mais curto" ou a rota mais relevante entre os livros, transformando uma simples sugestão em uma análise de compatibilidade.

O projeto foi estruturado em duas fases principais:

- **Criação do Grafo de Recomendações**: Implementação de um grafo robusto usando `HashMap<Livro, Set<Livro>>` para modelar as conexões entre um acervo de mais de 10 livros, onde cada um possui ao menos duas recomendações diretas.
- **Cálculo de Relevância com Dijkstra**: Aplicação do algoritmo de Dijkstra para encontrar os caminhos mais curtos, onde uma menor distância simboliza maior afinidade e, consequentemente, uma recomendação de maior qualidade.

---

## 🛠️ Estrutura do Projeto

O projeto é composto por diversas classes Java, cada uma com uma responsabilidade específica dentro do sistema:

- **`Main.java`**: Ponto de entrada da aplicação.
- **`Livro.java`**: Representa a entidade livro, com seus respectivos atributos.
- **`Usuario.java`**: Modela o usuário da biblioteca.
- **`Emprestimo.java`**: Gerencia as informações sobre os empréstimos realizados.
- **`GerenciadorBiblioteca.java`**: Centraliza as operações relacionadas ao acervo de livros.
- **`GerenciadorUsuarios.java`**: Controla as operações relacionadas aos usuários do sistema.
- **`InterfaceUsuario.java`**: Responsável pela interação com o usuário (interface de linha de comando ou gráfica).
- **`ArvoreBinaria.java`** e **`NoArvore.java`**: Implementam a estrutura de dados de árvore binária para busca e organização dos dados.
- **`GrafoRecomendacao.java`**: Implementa um grafo para o sistema de recomendação de livros.
- **`HistoricoNavegacao.java`**: Armazena o histórico de interações do usuário.

---

## ✨ Funcionalidades e Diferenciais

O sistema foi arquitetado para ser mais do que um gerenciador de biblioteca, focando em um mecanismo de sugestão de alta performance:

- **Modelagem de Livros**: A classe `Livro.java` define a estrutura de cada livro, servindo como o nó fundamental do grafo de recomendações.
- **Grafo de Recomendações Inteligente**: A classe `GrafoRecomendacao.java` implementa o grafo, representando as relações complexas entre as obras.
- **Análise de Relevância com Dijkstra**: O diferencial do projeto. O algoritmo de Dijkstra calcula a distância entre um livro de origem e todos os outros no grafo, permitindo classificar as obras por afinidade.
- **Estrutura de Suporte**: Classes como `GerenciadorBiblioteca.java` e `GerenciadorUsuarios.java` foram criadas para gerenciar o acervo e os usuários.
- **Interface com Usuário**: A classe `InterfaceUsuario.java` é o ponto de interação do usuário com o motor de recomendações.

---

## 🛠️ Algoritmo e Implementação

O núcleo do sistema de recomendação é o algoritmo de **Dijkstra**, adaptado para um **grafo não ponderado**, onde cada aresta tem peso implícito de 1. A lógica central foi implementada da seguinte forma:

```java
/**
 * Calcula as distâncias mais curtas de um livro de origem para todos os outros
 * livros no grafo usando uma adaptação do algoritmo de Dijkstra.
 * A distância representa o nível de recomendação (quanto menor, mais forte).
 *
 * @param grafo O grafo de livros e suas recomendações diretas.
 * @param origem O livro a partir do qual as distâncias serão calculadas.
 * @return Um mapa contendo os livros alcançáveis e suas respectivas distâncias da origem.
 */
public static Map<Livro, Integer> djikstraSimples(HashMap<Livro, Set<Livro>> grafo, Livro origem) {
    Map<Livro, Integer> distancias = new HashMap<>();
    Queue<Livro> fila = new LinkedList<>();

    // A distância do livro de origem para ele mesmo é 0.
    distancias.put(origem, 0);
    fila.add(origem);

    while (!fila.isEmpty()) {
        Livro atual = fila.poll();
        int distanciaAtual = distancias.get(atual);

        // Explora os vizinhos (recomendações diretas).
        for (Livro vizinho : grafo.getOrDefault(atual, new HashSet<>())) {
            // Se o vizinho ainda não foi visitado, calcula sua distância e o adiciona à fila.
            if (!distancias.containsKey(vizinho)) {
                distancias.put(vizinho, distanciaAtual + 1);
                fila.add(vizinho);
            }
        }
    }
    return distancias;
}
```
Essa implementação permite que, ao selecionar um livro, o sistema não apenas mostre as duas recomendações diretas, mas também as recomendações das recomendações, criando uma cadeia de descoberta muito mais rica e precisa.

---

