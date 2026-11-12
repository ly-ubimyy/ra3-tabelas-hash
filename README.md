# Trabalho RA3 – Tabelas Hash (Amazon Books Reviews)

**Resolução de Problemas Estruturados em Computação – PUCPR**  
Professor: Andrey Cabral Meira · Monitor: Luis Gustavo Câmara Martins

Para cada usuário da base *Amazon Books Reviews* (Kaggle), o programa descobre qual é o livro de **maior média global** entre os livros que ele avaliou. Tudo é feito com uma **tabela hash própria**, com encadeamento separado, e o trabalho compara três funções hash (FNV1a, polinomial 31 e djb2).

## Integrantes

| Nome | GitHub | Parte no trabalho |
| --- | --- | --- |
| Nicolly Stefany Pedroso Cordeiro | [@ly-ubimyy](https://github.com/ly-ubimyy) | Funções hash, tabela hash com encadeamento separado e parte experimental |
| Nicole | [@nicolefutikami](https://github.com/nicolefutikami) | Leitura do CSV, descarte de linhas inválidas, melhor livro, arquivo de saída e consultas |
| Nikolly | [@nikollygsilva](https://github.com/nikollygsilva) | Ordenação (merge sort), top 10, medição de tempos e relatório |

## Links

- Vídeo: *em breve*
- Relatório: *em breve*

## O problema

A base `Books_rating.csv` tem cerca de 3 milhões de avaliações (aprox. 2,7 GB). A média de um livro é a soma de **todas** as notas que ele recebeu na base dividida pelo número de avaliações (N). Para cada usuário, o melhor livro é o de maior média entre os que ele avaliou.

**Desempate:** maior média → maior N → Title em ordem alfabética.

## Restrições seguidas

- Nenhum método pronto, exceto leitura/escrita de arquivos e `System.nanoTime`
- Sem `HashMap`, `ArrayList`, `equals`, `hashCode`, `parseInt`/`parseDouble`, `Arrays.sort`, `Math.*` e `.length`
- Apenas os tipos `int`, `float`, `double`, `char` e `String`, e vetores desses tipos
- Código inteiramente em português

## Como compilar e executar

Requisitos: Java 17 ou mais novo e cerca de 4 GB de memória livre.

1. Baixe o `Books_rating.csv` no Kaggle (*Amazon Books Reviews*) e coloque na raiz do projeto.
2. Coloque na raiz o `user_id.txt`, com um User_id por linha.
3. Compile:

```
javac -encoding UTF-8 -d out src/*.java
```

4. Execute:

```
java -Xmx4g -cp out Principal
```

Para testar só as funções hash com os valores de referência do enunciado:

```
java -cp out TesteHash
```

## Arquivos gerados

| Arquivo | Conteúdo |
| --- | --- |
| `usuario_melhor_livro.csv` | User_id, profileName, Title, media_livro, N_livro, nota_usuario |
| `usuario_melhor_livro_ordenado.csv` | A mesma lista, ordenada por média (desc), N (desc) e User_id |
| `top10_livros.csv` | Os 10 livros de maior média com N ≥ 100 |
| `experimento.csv` | Colisões, sondagens, pior caso e tempos de cada função hash |

## Estrutura do código

| Classe | Responsabilidade |
| --- | --- |
| `FuncoesHash` | FNV1a, polinomial 31 e djb2 |
| `Textos` | Guarda vários textos em um único vetor de char (início e tamanho de cada um) |
| `TabelaHash` | Tabela com 1.000.003 posições e listas encadeadas feitas com vetores |
| `LeitorCsv` | Lê o CSV caractere por caractere, tratando aspas, vírgulas e quebras de linha |
| `Dados` | Tabela de títulos (soma, N), tabela de usuários (nome, melhor livro) e avaliações |
| `Ordenacao` | Merge sort da lista de usuários |
| `Escritor` | Escreve números e textos nos arquivos e no console |
| `Experimento` | Parte experimental com as três funções hash |
| `Principal` | Junta tudo e mede o tempo de cada etapa |

## Principais decisões

- As notas são guardadas em décimos (5.0 = 50), para trabalhar só com inteiros.
- Duas médias são comparadas sem divisão: `somaA × nB` contra `somaB × nA`.
- Ordenação com **merge sort**: O(n log n) mesmo no pior caso e estável.
- A tabela tem capacidade fixa de 1.000.003 posições (número primo), sem rehash.
