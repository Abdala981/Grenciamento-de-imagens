# Gerenciador de Banco de Imagens Binárias

Este projeto implementa um Sistema Gerenciador de Banco de Dados (SGBD) para armazenamento, compressão e manipulação de imagens no formato PGM (Portable Gray Map).

O sistema foca em imagens binárias, utilizando binarização por limiar e compressão RLE (Run-Length Encoding) para otimizar o armazenamento em arquivos binários.

## Funcionalidades Principais

* **Adição e Binarização:** Leitura de arquivos PGM (formato P2 - ASCII), aplicação de limiar definido pelo usuário para binarização (transformação em 0 ou 1) e inserção no banco.
* **Compressão RLE:** Implementação do algoritmo Run-Length Encoding para compactar os dados da imagem antes da escrita no arquivo binário.
* **Indexação:** Utilização de um arquivo de índice (.idx) separado do arquivo de dados (.dat) para acesso rápido aos metadados e controle de estado.
* **Remoção Lógica:** Exclusão rápida de registros através de flags no índice, mantendo a integridade do arquivo de dados.
* **Compactação Física (Garbage Collection):** Funcionalidade para reescrever o banco de dados removendo fisicamente os registros marcados como excluídos. Implementado com complexidade de espaço constante O(1), utilizando buffers de tamanho fixo.
* **Reconstrução por Média:** Algoritmo capaz de fundir múltiplas versões de uma imagem (com o mesmo nome) calculando a média aritmética dos pixels, útil para redução de ruído em amostras múltiplas.

## Detalhes Técnicos da Implementação

O projeto foi desenvolvido inteiramente em Linguagem C, utilizando manipulação de arquivos de baixo nível e alocação dinâmica de memória.

### Estrutura de Arquivos

O sistema opera mantendo dois arquivos principais:

1.  **meu_banco.idx (Índice):** Arquivo de texto contendo os metadados. Cada linha armazena: Nome, Limiar, Largura, Altura, Offset (posição em bytes no arquivo de dados), Tamanho dos dados comprimidos e Status de remoção.
2.  **meu_banco.dat (Dados):** Arquivo binário que armazena sequencialmente os pixels das imagens processados pelo algoritmo RLE.

### Algoritmo de Compressão

Foi utilizado o RLE (Run-Length Encoding). Dado que as imagens são binarizadas, a redundância de pixels sequenciais (0s ou 1s) é alta. O algoritmo armazena apenas a contagem dessas sequências, reduzindo significativamente o espaço em disco para imagens com pouca variação de entropia.

### Gerenciamento de Memória

O sistema utiliza alocação dinâmica (malloc/free) para carregar as matrizes de imagem em memória apenas durante o processamento, garantindo eficiência no uso de RAM.

## Compilação e Execução

### Pré-requisitos
* Compilador C (GCC, Clang ou equivalente).
* Imagens de entrada no formato PGM P2 (ASCII).

### Instruções de Compilação

Para compilar o projeto utilizando o GCC, execute o seguinte comando no terminal:

```bash
gcc main.c -o ger_images
```
Execução
Para iniciar o programa:

Linux/macOS:

```Bash
./ger_images
